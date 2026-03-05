# PROJECT_LOG

## Escopo Atual (v0.2.0-ex)

- **Extension para A1111 e Forge Classic** — Browse, download e organize modelos do CivitAI diretamente no WebUI com suporte nativo a Gradio 3.x
- **Auto-organização inteligente** — Modelos organizados por base model (SDXL/, Pony/, Illustrious/, etc.) com backup/rollback automático
- **Download robusto com Aria2** — Queue persistente, hash validation, auto-reconnect, multi-connection e tolerância a screen lock/SSE disconnect
- **Dashboard e estatísticas** — Análise de disco por categoria/arquitetura, top files, export CSV/JSON
- **Curadoria de criadores** — Sistema de favoritos ⭐ e ban 🚫 com persistência em disco

---

## Estado Rápido

**Stack:** Python 3.10+ + Gradio 3.15+ + A1111/Forge Classic + Aria2 + JavaScript vanilla  
**Features ativas:** Browse/Search, Download Queue, Auto-Organization, Update Detection, Dashboard, Creator Management, Update Selected  
**Status:** v0.2.0-ex — Estável, todas as features do Neo v0.7.0 compatíveis com Gradio 3.x portadas  
**Upstream:** sd-civitai-browser-neo (Gradio 4+, Forge Neo) — recebe features genéricas, não APIs específicas do Gradio 4+

---

## Linha do Tempo

### 2026-03-05 — Setup Inicial: Documentação Criada

**Contexto:** Criação dos arquivos de contexto para orientar agentes AI e humanos no desenvolvimento do projeto EX.

#### Arquivos Principais Mapeados
- **Backend Python:**
  - `scripts/civitai_gui.py` (~2064 linhas) — Interface Gradio 3, callbacks, settings
  - `scripts/civitai_api.py` — Client CivitAI API, geração HTML, validação
  - `scripts/civitai_download.py` — Aria2 RPC, queue manager, hash check
  - `scripts/civitai_file_manage.py` — File ops, organização, dashboard, creator mgmt
  - `scripts/civitai_global.py` — Estado global compartilhado (anti-pattern legacy)
  - `scripts/download_log.py` — Persistência JSONL da queue

- **Frontend:**
  - `javascript/civitai-html.js` — Card interaction, tile sizing, UI dynamics
  - `style_html.css` + `style.css` — Estilos customizados

- **Infra:**
  - `install.py` — Hook do WebUI para instalar dependencies
  - `aria2/` — Binários Win/Linux do Aria2

#### Features Confirmadas (README ↔ Código)
Todas as features do Neo v0.7.0 compatíveis com Gradio 3.x:
- ✅ Browse & Search com filtros avançados (base model, content type, period, sort)
- ✅ Download queue com Aria2 (multi-connection, cancel, progress)
- ✅ Queue persistence via `download_log.py` → `ex_download_queue.jsonl`
- ✅ SHA256 hash validation pós-download
- ✅ Auto-organization com backup (últimos 5 em `civitai_organization_backups.json`)
- ✅ Update detection (orange borders) com batch update
- ✅ Update Selected — checkbox multi-select, dynamic button label
- ✅ Dashboard com breakdown por categoria/arquitetura, top 10 files/categories, CSV/JSON export
- ✅ Creator management (favorite/ban) com persistência em `favoriteCreators.txt` / `bannedCreators.txt`
- ✅ Model info overlay com "Send to txt2img", LoRA syntax insertion
- ✅ Smart version selection (respeita filtro de base model ativo)
- ✅ Embeddings folder auto-detection (old vs new layout)
- ✅ Downloads survive screen lock (Win+L, RunPod SSE disconnect)
- ✅ EARLY_ACCESS/NO_API safety (no stray saves, no unrelated file deletes)
- ✅ Audit log (`ex_update_audit.jsonl` para retention actions)

#### Decisões Arquiteturais
1. **Gradio 3.x Hard Requirement:** APIs diferentes do Gradio 4+
   - Não portar código do Neo sem validar compatibilidade
   - `gr.update()` syntax, event handling, component APIs são diferentes

2. **Estado Global (`gl.init()`):** Variáveis globais compartilhadas entre módulos (legacy design do Neo)
   - `download_queue`, `json_data`, `json_info`, `recent_model`, etc.
   - Threading: `_not_downloading` event para sincronização de downloads
   - **Não refatorar sem planejamento:** usado em toda a codebase

3. **Aria2 RPC Lifecycle:** Start automático no import de `civitai_download.py`
   - Port 24000, secret `R7T5P2Q9K6`
   - Auto-reconnect se crashar durante sessão
   - Tracking file: `aria2/running`

4. **Filesystem Safety:**
   - Delete via `send2trash()` (recycle bin)
   - Sanitização de filename (illegal chars, max length 246 bytes UTF-8 para Linux compat)
   - Associated files (`.json`, `.png`, `.txt`) movem junto com model

5. **Queue Persistence:**
   - JSONL format em `config_states/ex_download_queue.jsonl`
   - Estados: `queued → downloading → completed/cancelled/failed/dismissed`
   - Restore banner aparece se houver entradas `queued` órfãs após disconnect

6. **A1111/Forge Classic Folder Differences:**
   - Embeddings: `embeddings/` (Classic) ou `models/embeddings/` (novo)
   - Auto-detection implementada: avisa se ambos existem com arquivos
   - Não assume Forge Neo unified folders (ESRGAN, etc.)

7. **Audit Log (EX-only):**
   - `ex_update_audit.jsonl` registra scans e retention actions
   - Traceability para debugging de updates

#### Pontos Sensíveis
- **Update detection sensível:** Mesma lógica do Neo — comparação por `name_match` pode falhar em edge cases
- **NSFW check impreciso:** Depende da metadata inconsistente da CivitAI
- **Folder resolver None:** Debug print quando content type desconhecido
- **Threading não totalmente thread-safe:** `gl.download_queue` modificado por callbacks Gradio sem lock explícito
- **Print messages bug:** `civitai_global.py` ainda diz "CivitAI Browser Neo" em vez de "Ex" (bug menor de copy-paste)

#### Diferenças vs. Neo (Gradio 4+)
- **Sem APIs Gradio 4+:** Não pode usar novos componentes/events do Gradio 4
- **Folder logic simplificado:** Não assume Forge Neo unified folders
- **Audit log:** EX tem `ex_update_audit.jsonl`, Neo não (feature EX-only)
- **Versioning:** Suffix `-ex` para marcar releases exclusivos

---

## Backlog

### 🐛 Bugs Conhecidos
- [ ] **Update detection:** Lógica de `name_match` pode não marcar alguns outdated models (herança do Neo)
- [ ] **NSFW check:** Sistema de `nsfwLevel` não é 100% preciso (metadata CivitAI)
- [ ] **Print messages:** `print()` e `debug_print()` dizem "Neo" em vez de "Ex" (`civitai_global.py` linha 69, 76)

### 🔧 Technical Debt
- [ ] **Estado global:** Refatorar `gl.init()` para classe/contexto thread-safe (breaking change, coordenar com Neo)
- [ ] **Threading:** Adicionar locks explícitos em `download_queue` mutations
- [ ] **Error handling:** Padronizar tratamento de exceções (atualmente mix de try/except com prints)
- [ ] **Type hints:** Adicionar type annotations (código legacy sem tipos)
- [ ] **Print prefix:** Corrigir "Neo" → "Ex" em `civitai_global.py`

### ✨ Features Planejadas (Roadmap)

#### v0.3.0-ex — Stabilization *(próxima)*
- [ ] A1111-specific path handling improvements
- [ ] Forge Classic quirks and fixes
- [ ] Testing on different Gradio 3.x minor versions
- [ ] Corrigir print prefix bug

#### v0.4.0-ex — Extended Features
- [ ] Saved search presets
- [ ] Favorites in creator search dropdown
- [ ] SHA256 cache injection (read `.json` sidecars → populate `cache.json`)

### 📝 Melhorias Futuras (Não Priorizadas)
- [ ] **Dashboard:** Gráficos interativos (se compatível com Gradio 3)
- [ ] **Search:** Autocomplete nos filtros
- [ ] **Organization:** Regras customizáveis (não apenas base model)
- [ ] **API:** Rate limit handling mais sofisticado
- [ ] **I18n:** Suporte a idiomas (atualmente inglês/português misturados)

### 🧪 Investigações
- [ ] **Performance:** Profile HTML generation (pode ser lento?)
- [ ] **Memory:** `gl.json_data` cresce indefinidamente? (leak potencial em sessões longas)
- [ ] **Gradio 3 limits:** Quais features do Neo são impossíveis de portar?

---

## Notas de Manutenção

### Ao Adicionar Features
1. Verificar se é compatível com Gradio 3.x (se não, não adicionar)
2. Se tocar filesystem: adicionar backup/rollback
3. Se tocar download: atualizar `download_log.py` states se necessário
4. Atualizar README.md (Changelog + Features)
5. Adicionar entrada neste PROJECT_LOG.md
6. Se mudar arquitetura/invariantes: atualizar AGENTS.local.md

### Ao Fazer Bugfix
1. Documentar root cause
2. Adicionar entrada datada neste log
3. Se crítico: mencionar no README Changelog
4. Considerar adicionar test case (quando/se framework de testes for implementado)

### Sync com NEO (Upstream)
- **Regra geral:** NEO é upstream, EX recebe features quando compatíveis com Gradio 3.x
- **Gradio 4+ features:** NÃO portar para EX
- **Forge Neo folder logic:** NÃO portar para EX (tem próprio fallback simples)
- **Bugfixes genéricos:** PORTAR para EX (API logic, file ops, threading, etc.)
- **Features EX-only:** Podem ir para NEO se fizerem sentido (ex: audit log)
- **Comunicação:** Marcar PRs/commits com `[NEO-ONLY]`, `[EX-ONLY]`, ou `[SYNC-FROM-NEO]`

### Testing Matrix
- A1111 (latest stable)
- Forge Classic (latest)
- Gradio 3.15, 3.20, 3.latest

---

**Última atualização:** 2026-03-05
