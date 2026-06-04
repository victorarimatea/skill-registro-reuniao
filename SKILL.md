# SKILL.md — skill-registro-reuniao

**Repositório:** skill-registro-reuniao
**Tipo:** S (Skill)
**ID:** S06
**Versão:** v1.0 — 2026-06-02
**Visibilidade:** Público
**Ecossistema:** DTD/SETIS/SES-DF — github.com/victorarimatea
**Mantenedor:** Victor Leonardo Arimatea Queiroz — Diretor de Transformação Digital

---

## Descrição

Esta skill transforma resumos de reuniões — gerados por dispositivos de transcrição
automática (ex.: PLAUD NOTE Pro), anotações livres ou sínteses — em registros
institucionais padronizados para uso em processos formais da administração pública
(ex.: SEI, registros de projeto, atas oficiais).

**Gatilhos de ativação:** "transformar essa reunião", "gerar o registro",
"fazer a ata", "montar o registro institucional", "processar essa reunião",
"registrar a reunião", ou quando o usuário colar um resumo/transcrição de
reunião no chat solicitando estruturação formal.

---

## Instruções de Execução

Ao ser ativada, esta skill conduz o processo completo em 5 etapas.
Siga exatamente a sequência abaixo.

---

### ETAPA 0 — Identificação do contexto

Antes de processar o resumo, determine:

1. **A reunião é associada a um projeto formal do ecossistema?**
   - Se sim: identificar qual projeto (ex.: P01 `prj-telessaude-poc-prisional`)
   - Se não: a reunião é avulsa — será registrada apenas no `agenda-[unidade]`

2. **Qual a unidade organizacional responsável?**
   - Padrão: DTD/SETIS/SES-DF
   - Repositório de destino padrão: `agd-dtd`

3. **Há token de acesso disponível nesta sessão?**
   - Se sim: a skill irá depositar o arquivo gerado no repositório correspondente
     via S04 ao final (Etapa 4)
   - Se não: entregar apenas o documento no chat para o usuário inserir manualmente

Se qualquer informação de contexto estiver ausente, perguntar ao usuário
antes de prosseguir para a Etapa 1.

---

### ETAPA 1 — Recebimento do input

Receber o resumo da reunião colado pelo usuário no chat.

O input pode ser:
- Resumo gerado automaticamente pelo PLAUD NOTE Pro (formato síntese)
- Transcrição bruta (texto corrido)
- Anotações livres do condutor da reunião
- Combinação de fontes

**Se o input parecer truncado ou incompleto:** informar o usuário e solicitar
confirmação antes de prosseguir.

**Se o input vier do PLAUD NOTE:** aplicar correção semântica de erros de
transcrição automática antes de processar (nomes de pessoas, siglas institucionais,
termos técnicos de saúde pública e tecnologia). Nunca inventar informações
ausentes — inferir com cautela quando o contexto permitir.

---

### ETAPA 2 — Geração do registro institucional

Aplicar o seguinte prompt ao resumo recebido, produzindo o documento
estruturado com as 8 seções obrigatórias:

---

**[INÍCIO DO PROMPT INSTITUCIONAL]**

Você atuará como assessor técnico responsável por transformar resumos de reuniões
em um REGISTRO INSTITUCIONAL PADRONIZADO para uso em processos formais (ex.: SEI).

**CONTEXTO:**
Estou conduzindo reuniões no âmbito da gestão pública em saúde (SES-DF),
envolvendo projetos estratégicos (ex.: telessaúde, transformação digital,
gestão hospitalar). Preciso transformar resumos de reuniões em registros formais,
claros, objetivos e padronizados, que possam ser utilizados em processos institucionais.

**OBJETIVO:**
Gerar um resumo estruturado da reunião com linguagem institucional, clareza
executiva e foco em:
- Tomada de decisão
- Encaminhamentos
- Riscos
- Evolução do projeto

**ESTRUTURA OBRIGATÓRIA:**

**1. TÍTULO DA REUNIÃO**
Formato: `[CLASSIFICAÇÃO] nº XX — Nome sintético e claro da reunião`

Classificações possíveis:
- ALINHAMENTO ESTRATÉGICO
- ALINHAMENTO TÁTICO
- ALINHAMENTO OPERACIONAL
- ALINHAMENTO TÉCNICO
- ALINHAMENTO TÉCNICO-OPERACIONAL
- ALINHAMENTO TÉCNICO-ESTRATÉGICO

O número sequencial (nº XX) deve ser inferido do contexto quando disponível.
Se não houver referência, usar `nº —` e registrar como pendente.

**2. METADADOS**
- Data:
- Local:
- Participantes: (nome, cargo/função, instituição)
- Formato: (presencial / remoto / híbrido)

**3. CONTEXTO**
Parágrafo curto explicando:
- Por que a reunião aconteceu
- Em que etapa do projeto ou processo ela se insere
- Qual o cenário institucional relevante

**4. PRINCIPAIS PONTOS DISCUTIDOS**
Organizar em blocos temáticos claros.
Evitar transcrição literal — foco em síntese estruturada.
Cada bloco com título em negrito.

**5. DECISÕES**
Listar apenas decisões efetivamente tomadas.
Formato: verbo no passado + objeto + responsável (quando identificável).
Exemplo: "Definido o CIR como unidade piloto para a PoC — DTD/SEAPE."

**6. ENCAMINHAMENTOS**
Lista objetiva com ações práticas decorrentes da reunião.
Formato: ação + responsável + prazo (quando disponível).
Exemplo: "Elaborar minuta do Termo de Referência — AJL/SES-DF — até 15/06/2026."

**7. PONTOS CRÍTICOS IDENTIFICADOS**
Riscos, lacunas, dependências ou fragilidades identificadas.
Sinalizar impacto potencial quando identificável.

**8. REFERÊNCIA (quando aplicável)**
"Registro elaborado a partir de síntese estruturada gerada por [fonte] em [data]."
Exemplo: "Registro elaborado a partir de síntese estruturada gerada pelo
PLAUD NOTE Pro em 2026-06-02."

**DIRETRIZES:**
- Linguagem institucional (padrão SES-DF / administração pública)
- Clareza > volume de texto
- Evitar redundância e narrativa longa
- Não copiar o texto original — interpretar e estruturar
- Priorizar utilidade prática do documento
- Destacar o que impacta o andamento do projeto
- Se houver apresentação técnica longa: resumir objetivamente, manter foco no impacto
- Se houver erros de transcrição: corrigir semanticamente
- Se faltar informação: inferir com cautela, nunca inventar

**[FIM DO PROMPT INSTITUCIONAL]**

---

### ETAPA 3 — Montagem do arquivo Markdown

Após gerar o documento com as 8 seções, montar o arquivo `.md` com:

**Front Matter YAML obrigatório:**

```yaml
---
id_registro: [gerado automaticamente: AAAA-MM-DD-[classificacao]-[descricao-slug]]
titulo:
classificacao:
numero_sequencial:
data_reuniao:
local:
formato:
participantes:
projeto_associado:      # ID do projeto P, ou "avulso"
unidade:                # ex: DTD/SETIS/SES-DF
data_registro:          # data de geração deste arquivo
versao_registro:        # 1.0
---
```

**Nomenclatura do arquivo:**
```
AAAA-MM-DD-[CLASSIFICACAO]-[descricao-curta].md
```

Exemplos:
- `2026-06-02-ALINHAMENTO-ESTRATEGICO-telessaude-poc-prisional.md`
- `2026-05-05-ALINHAMENTO-TECNICO-OPERACIONAL-totem-health360-seape.md`

A classificação no nome do arquivo usa apenas o tipo principal em maiúsculas,
sem acentos, com hífens.

---

### ETAPA 4 — Depósito e referências (sessão autenticada)

**Quando há token disponível**, acionar a S04 para:

1. **Depositar no repositório A:**
   - Caminho: `agenda-[unidade]/reunioes/AAAA/MM/[nome-do-arquivo].md`
   - Exemplo: `agenda-dtd/reunioes/2026/06/2026-06-02-ALINHAMENTO-ESTRATEGICO-...md`

2. **Se a reunião for de projeto:**
   - Adicionar entrada no `reunioes/` do repositório P correspondente
     OU criar referência no `EXECUCOES.md` do projeto apontando para o arquivo no A
   - Nunca duplicar o conteúdo — apenas referenciar

3. **Atualizar o INDICE.md do `agenda-[unidade]`** com a nova entrada

**Quando não há token:** entregar o documento no chat com instrução clara:
> "Arquivo pronto. Salve como `[nome-do-arquivo].md` e insira manualmente em
> `agenda-dtd/reunioes/AAAA/MM/`. Se associado a projeto, adicione referência
> em `[repositorio-projeto]/EXECUCOES.md`."

---

### ETAPA 5 — Auto-verificação

Antes de entregar o documento, verificar:

| Critério | Verificação |
|---|---|
| Front Matter YAML válido | Todos os campos preenchidos ou marcados como pendentes |
| 8 seções obrigatórias presentes | Headers presentes no documento |
| Seção DECISÕES não vazia | Se reunião não gerou decisões, registrar explicitamente "Nenhuma decisão formal tomada nesta reunião" |
| Seção ENCAMINHAMENTOS não vazia | Se não há ações, registrar "Nenhum encaminhamento formal registrado" |
| Nomenclatura do arquivo correta | Seguir padrão AAAA-MM-DD-[CLASSIFICACAO]-[descricao].md |
| Linguagem institucional | Sem gírias, informalidades ou abreviações não padronizadas |

---

## Princípios Fundamentais

| Princípio | Definição |
|---|---|
| **Fidelidade ao contexto** | O registro reflete o que foi discutido — sem inventar ou omitir deliberações |
| **Utilidade prática** | O documento deve ser diretamente utilizável no SEI ou em processo formal sem edição adicional |
| **Separação conteúdo/armazenamento** | A skill gera o conteúdo; a S04 cuida do armazenamento no ecossistema |
| **Referência, não duplicação** | Quando uma reunião é de projeto, o arquivo fica no A e é referenciado no P |
| **Linguagem institucional** | Padrão SES-DF / administração pública federal em todo o documento gerado |

---

## Dependências

- **S04 skill-github-orquestracao:** para depósito automático em sessões autenticadas
- **agenda-[unidade]:** repositório de destino (ex: `agd-dtd`)
- **Repositório P do projeto:** quando a reunião for associada a projeto formal
- **PLAUD NOTE Pro (opcional):** dispositivo de captura — a skill aceita qualquer formato de input

---

## Arquivos Relacionados

| Arquivo | Propósito |
|---|---|
| `WORKFLOW.md` em `wkf-registro-reuniao` | Processo organizacional completo — missão, etapas, histórico, roadmap |
| `agenda-dtd/` | Repositório privado de registros de reunião da DTD |

---

## Histórico de Versões

| Versão | Data | Alteração |
|---|---|---|
| v1.0 | 2026-06-02 | Criação — formalização do workflow de registro de reunião desenvolvido pelo Diretor de Transformação Digital com suporte do PLAUD NOTE Pro |

---

*Skill mantida pelo ecossistema DTD/SETIS/SES-DF.*
*Consulte o CONTEXTO.md em github.com/victorarimatea/hub-fonte para visão completa.*
