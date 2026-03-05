---
description: AI rules derived by SpecStory from the project AI interaction history
applyTo: *
---

# GitHub Copilot Instructions

Projeto: CivitAI Browser Ex

## Idioma
- Chat: português brasileiro
- Código/docs/commits: inglês

## Antes de qualquer alteração
1. Leia ./README.md
2. Leia ./AGENTS.local.md
3. Leia ./docs/PROJECT_LOG.md
4. Resuma em pt-BR: escopo, invariantes, última mudança

## Workflow obrigatório
RESEARCH → PLAN (aguardar aprovação) → EXECUTE → VALIDATE

## Após mudanças significativas
1. Atualize docs/PROJECT_LOG.md com entrada datada em português
2. Se arquitetura/features mudaram, atualize AGENTS.local.md
3. Se relevante para usuários, sugira atualizar README.md

## Restrições
- Nunca commitar AGENTS.local.md, copilot-instructions.md, PROJECT_LOG.md
- Code/commits em inglês, chat em português
- Nunca estado mutável em nível de módulo (já existe, mas não adicionar mais)
- I/O crítico com wrappers seguros

## Gradio 3.x Hard Constraint
- **Target:** A1111, Forge Classic (Gradio 3.15+)
- **NÃO usar APIs do Gradio 4+** sem verificar backport para Gradio 3
- Upstream: sd-civitai-browser-neo (Gradio 4+)
- Portar features genéricas do Neo, não APIs específicas do Gradio 4

## Twin Project Sync
- **NEO é upstream:** Receba bugfixes e features genéricas
- **EX é downstream:** Não porte Gradio 4+ ou Forge Neo folder logic
- Marque commits: `[SYNC-FROM-NEO]`, `[EX-ONLY]`
