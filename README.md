# skill-registro-reuniao

**Tipo:** S — Skill
**ID:** S06
**Versão:** v1.0 — 2026-06-02
**Visibilidade:** Público
**Mantenedor:** victorarimatea

---

## O que é esta skill

Transforma resumos de reuniões — gerados pelo PLAUD NOTE Pro, anotações livres
ou transcrições — em registros institucionais padronizados para uso em processos
formais da administração pública (SEI, atas oficiais, registros de projeto).

**Gatilhos:** "transformar essa reunião", "gerar o registro", "fazer a ata",
"registrar a reunião", ou quando o usuário colar um resumo no chat.

---

## Fluxo resumido

```
Resumo colado no chat
        ↓
S06 identifica contexto (projeto ou avulso)
        ↓
Gera registro com 8 seções institucionais
        ↓
Monta arquivo .md com Front Matter YAML
        ↓
[sessão autenticada] S04 deposita em agenda-dtd
        ↓
[reunião de projeto] S04 cria referência no repositório P
```

---

## Workflow associado

→ [workflow-registro-reuniao (W02)](https://github.com/victorarimatea/workflow-registro-reuniao)

## Repositório de destino

→ agenda-dtd (A01) — privado

---

## Navegação rápida

→ **[INDICE.md](./INDICE.md)**

*Repositório mantido pela DTD/SETIS/SES-DF — github.com/victorarimatea*
