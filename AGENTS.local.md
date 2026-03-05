# AGENTS.local.md

## Projeto

**CivitAI Browser Ex** — Extension para Stable Diffusion WebUI (A1111, Forge Classic) que permite navegar, baixar e gerenciar modelos do CivitAI diretamente na interface, com auto-organização, dashboard de uso de disco, gerenciamento de criadores e suporte nativo a Gradio 3.x.

**Versão atual:** v0.2.0-ex (Stability & Feature Sync — Neo v0.7.0 features for Gradio 3.x)

## Twin Projects

Este projeto é o fork Gradio 3.x do projeto Neo:

1. **sd-civitai-browser-ex** (este repo)
   - Target: A1111, Forge Classic, qualquer WebUI com Gradio 3.x
   - Gradio: 3.15+
   - Upstream: sd-civitai-browser-neo
   - Features: Todas as features compatíveis do Neo, sem APIs específicas do Gradio 4+

2. **sd-civitai-browser-neo**
   - Target: Forge Neo
   - Gradio: 4.40.0+
   - Features exclusivas: APIs Gradio 4+, Forge Neo folder layout avançado

**⚠️ Relação:** NEO é upstream, EX recebe features quando compatíveis com Gradio 3. Mudanças específicas do Gradio 4+ ficam apenas no Neo.

---

## Tech Stack

### Core
- **Python:** 3.10+ (compatível com A1111/Forge Classic)
- **Gradio:** 3.15+ (breaking difference vs. Gradio 4+)
- **Target WebUIs:** A1111, Forge Classic, qualquer Gradio 3.x SD WebUI
- **Aria2:** 1.x (RPC-based download engine, binaries inclusos em `aria2/win` e `aria2/lin`)

### Dependencies (requirements.txt)
- `send2trash` — Recycle bin deletion
- `ZipUnicode` — Unicode-safe ZIP extraction
- `beautifulsoup4` — HTML parsing (model info)
- `packaging` — Version comparison
- `pysocks` — Proxy support

### Runtime Dependencies (implicit)
- `requests` — HTTP client
- `Pillow` (PIL) — Image processing
- `gradio` — UI framework (provided by WebUI, must be 3.x)

### Frontend
- Vanilla JavaScript (`javascript/civitai-html.js`)
- Custom CSS (`style_html.css`, `style.css`)

---

## Comandos de Desenvolvimento

### Instalação (WebUI)
```bash
# Via Extensions → Install from URL
https://github.com/eduardoabreu81/sd-civitai-browser-ex
```

### Manual
```bash
cd extensions/
git clone https://github.com/eduardoabreu81/sd-civitai-browser-ex
cd sd-civitai-browser-ex
pip install -r requirements.txt
```

### Debug
- **Debug prints:** Settings → `civitai_debug_prints` (requires UI reload)
- **Aria2 RPC logs:** Settings → `show_log`

### Testing
- Nenhum framework de testes formal
- Manual testing via WebUI (A1111, Forge Classic)

---

## Arquitetura

### Estrutura de Pastas
```
sd-civitai-browser-ex/
├── install.py                   # Dependency install hook
├── requirements.txt             # Python dependencies
├── style_html.css              # Model card/preview styles
├── style.css                   # General UI styles
│
├── aria2/                      # Download engine binaries
│   ├── win/aria2.exe
│   └── lin/aria2
│
├── .github/
│   ├── logo.png
│   ├── copilot-instructions.md
│   └── ISSUE_TEMPLATE/
│
├── javascript/
│   └── civitai-html.js         # Frontend logic (card interaction, tile sizing, etc.)
│
└── scripts/                    # Python backend (loaded as extension)
    ├── civitai_gui.py          # Gradio UI definition (tabs, callbacks, settings)
    ├── civitai_api.py          # CivitAI API client, HTML generation, validation
    ├── civitai_download.py     # Aria2 integration, queue manager, hash verification
    ├── civitai_file_manage.py  # File ops (organize, delete, backup, dashboard, creator mgmt)
    ├── civitai_global.py       # Shared state (gl.init(), gl.download_queue, colors)
    └── download_log.py         # Queue persistence (ex_download_queue.jsonl)
```

### Fluxo de Dados
```
User (Gradio UI)
    ↓
civitai_gui.py (Gradio callbacks)
    ↓
civitai_api.py (CivitAI API) ←→ civitai_download.py (Aria2 RPC)
    ↓
civitai_file_manage.py (Filesystem)
    ↓
config_states/*.json (Metadata cache, backups, queue log)
```

### Estado Global
- `scripts/civitai_global.py` define `gl.init()` que inicializa variáveis globais (`download_queue`, `json_data`, etc.)
- ⚠️ **Padrão anti-pattern:** Estado mutável em nível de módulo (legacy design, não refatorar sem planejamento)
- **Nota:** `print()` e `debug_print()` ainda dizem "CivitAI Browser Neo" no código (bug de copy-paste, correto seria "Ex")

---

## Features Implementadas

### ✅ Core (v0.2.0-ex)
- [x] Browse CivitAI API (search, filters, sort)
- [x] Download com Aria2 (multi-connection, queue, cancel)
- [x] Queue persistence (survives session disconnect)
- [x] File integrity check (SHA256 hash validation)
- [x] Auto-organization por base model (SDXL/, Pony/, Illustrious/, etc.)
- [x] Update detection (orange borders, batch update)
- [x] Update Selected (checkbox multi-select, dynamic button)
- [x] Dashboard (disk usage stats, top files, CSV/JSON export)
- [x] Creator management (favorite ⭐, ban 🚫)
- [x] Model info overlay (preview images, meta fields)
- [x] Send to txt2img (prompt, negative, sampler, CFG)
- [x] Smart version selection (respects base model filter)
- [x] Embeddings folder auto-detection (old `embeddings/` vs new `models/embeddings/`)
- [x] Downloads survive screen lock (Win+L, RunPod SSE disconnect)
- [x] EARLY_ACCESS/NO_API safety (no stray saves, no unrelated file deletes)

### ✅ Safety
- [x] Recycle bin deletion (send2trash)
- [x] Auto-backup antes de organize/fix (last 5)
- [x] Filename sanitization (illegal chars, length limit 246 bytes UTF-8)
- [x] Conflict detection (skip existing files)
- [x] Audit log (`ex_update_audit.jsonl` para retention actions)

### ✅ UX/UI
- [x] Color-coded card borders (installed, outdated, early access)
- [x] Multi-select checkboxes (batch download)
- [x] Progress bars (download, validate, fix)
- [x] Settings persistence (search filters, tile size)
- [x] Video preview on hover
- [x] Color legend bar
- [x] Responsive tile sizing

### 🚧 Known Issues (v0.2.0-ex)
- Update detection sensível (mesma lógica do Neo)
- NSFW check impreciso (metadata CivitAI inconsistente)
- Print messages ainda dizem "Neo" em vez de "Ex" (`civitai_global.py`)

---

## Invariantes Críticos

### 1. Gradio 3.x Only
- **EX usa Gradio 3.15+** → APIs diferentes do Gradio 4+
- Não portar código do NEO sem validar compatibilidade com Gradio 3

### 2. A1111/Forge Classic Folder Layout
- Embeddings: `embeddings/` (Classic) ou `models/embeddings/` (novo)
- Auto-detection implementada: avisa se ambos existem com arquivos
- Não assume Forge Neo unified folders (ESRGAN, etc.)

### 3. Filesystem Safety
- **NUNCA delete direto:** sempre `send2trash()` ou backup antes
- **Filenames:** sanitizar + limit 246 bytes UTF-8 (Linux compat)
- **Associated files:** mover `.json`, `.png`, `.txt` junto com `.safetensors`

### 4. Aria2 RPC Lifecycle
- Start: `start_aria2_rpc()` no `civitai_download.py` (module-level)
- Auto-reconnect: se RPC crashar, restart automático
- Port: 24000, secret: `R7T5P2Q9K6`
- Cleanup: `aria2/_` → `aria2/running` file tracking

### 5. Estado Global (gl.init())
- Chamado no início de cada módulo (`gl.init()`)
- ⚠️ **Não thread-safe:** `download_queue` modificado por callbacks Gradio
- Threading: `_not_downloading` event para sincronização
- **NUNCA commitar:** `config_states/` (gitignored)

### 6. Hash Validation
- Todo download verifica SHA256 após completar
- Se divergir: delete + erro
- Hash normalizado: uppercase, strip whitespace

### 7. Queue Persistence
- `ex_download_queue.jsonl` em `config_states/`
- Estados: `queued → downloading → completed/cancelled/failed/dismissed`
- Restore banner aparece se houver `queued` entries órfãs

### 8. Update Audit Log
- `ex_update_audit.jsonl` registra scans e retention actions
- Traceability para debugging

---

## Sincronização com README

**Última sincronização:** 2026-03-05 (setup inicial)

### Divergências Conhecidas (README vs. Código)
- ✅ README menciona v0.2.0-ex features → confirmadas no código
- ✅ Tech stack: Gradio 3.x badge no README, confirmado em uso no código
- ✅ Features listadas: todas implementadas (validado via grep/read)
- ⚠️ README menciona "Ex" mas código ainda tem prints com "Neo" (bug menor)

### Checklist de Atualização
Quando features mudam, atualizar:
1. README.md (Changelog, Features, Roadmap)
2. AGENTS.local.md (este arquivo, seção Features)
3. docs/PROJECT_LOG.md (adicionar entrada datada)

---

## Pontos de Atenção para Agentes

### Threading
- `_not_downloading` event protege cancelamento
- `threading.Thread` em `civitai_download.py` para downloads
- Gradio callbacks não são thread-safe por padrão

### Gradio 3 Constraints
- `gr.update()` syntax diferente do Gradio 4+
- API methods disponíveis são limitadas vs. Gradio 4
- Não usar APIs do Gradio 4+ (checkar documentação antes de portar do Neo)

### Folder Resolution
- `contenttype_folder()` pode retornar `None` → debug print warning
- Embeddings: auto-detect + warning se ambos layouts existirem
- Não assume Forge Neo unified folders

### API Limits
- CivitAI rate limit: não documentado, usar with caution em loops
- Aria2 RPC timeout: 30s default (configurável via `aria2_flags`)

### Sync com NEO
- Features genéricas: portar do NEO para EX
- Gradio 4+ features: NÃO portar
- Forge Neo folder logic: NÃO portar (tem próprio fallback simples)

---

**Última atualização:** 2026-03-05
