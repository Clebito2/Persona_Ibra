# Como Extrair o Máximo do AntiGravity no AIOS

Para quem é isso: Membros que usam o AntiGravity e querem operar no AIOS com a mesma inteligência e precisão de quem usa Claude Code ou Gemini CLI — mas pelo caminho certo para essa plataforma.

---

## ⚙️ Entenda Primeiro: O Que é o AntiGravity?

O AIOS suporta várias plataformas. Cada uma tem um nível de integração diferente.

O AntiGravity opera em modo **workflow-based** — ou seja, sem hooks automáticos de ciclo de vida. Isso não é um bug. É uma arquitetura deliberada, integrada nativamente ao ecossistema Google Cloud (Firebase, Google MCP).

**O que muda na prática:**
- ❌ Sem verificações automáticas pré/pós-ação
- ❌ Sem rastreamento automático de sessão
- ❌ Sem audit trail nativo

**O que não muda:**
- ✅ Todos os 11 agentes disponíveis
- ✅ Todo o sistema de stories, workflows e squads
- ✅ A lógica de qualidade em 3 camadas

> [!TIP]
> A compensação é simples: você roda os validadores manualmente e carrega o contexto com intenção. Quem entende isso opera no mesmo nível de qualquer outra IDE.

---

## 📦 Passo 1 — Instale o AIOS no Seu Projeto

### Projeto novo:
```bash
npx @synkra/aios-core init meu-projeto 
```

### Projeto existente:
```bash
cd seu-projeto
npx @synkra/aios-core install 
```

### Atualização (Antes de qualquer sessão importante):
```bash
npx aios-core@latest install 
```

O instalador detecta instalações existentes, atualiza só o necessário, cria arquivos `.bak` das suas customizações e preserva configurações do projeto.

Após a instalação, o AIOS cria automaticamente o diretório `.antigravity/` com:
- `.antigravity/rules.md` — regras de comportamento dos agentes
- `.antigravity/antigravity.json` — configuração da plataforma
- `.agent/workflows/` — os workflows gerados para cada agente

---

## 🔧 Passo 2 — Sincronize os Agentes com o AntiGravity

A fonte de verdade dos agentes fica em `.aios-core/development/agents/`.

O AIOS propaga isso para o AntiGravity via sync. Rode isso sempre que atualizar o framework ou modificar agentes:

```bash
npm run sync:ide:antigravity 
```

Para verificar se está tudo sincronizado:

```bash
npm run sync:ide:check 
```

Se quiser sincronizar todas as IDEs de uma vez:

```bash
npm run sync:ide 
```

> [!IMPORTANT]
> **Por que isso importa:** No AntiGravity, os agentes são ativados via workflows gerados — não por slash commands. Se o sync não rodou, os workflows estão desatualizados e o agente opera com contexto errado.

---

## 🧠 Passo 3 — Entenda a Ativação de Agentes no AntiGravity

No Claude Code, você digita `/dev` e o agente ativa. No AntiGravity, a ativação é orientada por workflow.

**Isso significa:**
1. Cada agente tem um workflow gerado em `.agent/workflows/`
2. Você carrega esse workflow explicitamente no contexto
3. O agente responde dentro daquele frame

### Os 11 agentes disponíveis:
- `@dev` — Dex (implementação de código)
- `@qa` — Quinn (qualidade e testes)
- `@architect` — Aria (arquitetura técnica)
- `@po` — Nova (backlog e produto)
- `@pm` — Kai (estratégia de produto)
- `@sm` — River (facilitação de processo)
- `@analyst` — Zara (análise de negócio)
- `@data-engineer` — Dara (engenharia de dados)
- `@devops` — Felix (CI/CD e operações)
- `@ux-expert` — Uma (UX/UI)
- `@aios-master` — Pax (orquestração geral)

Para ativar qualquer agente no AntiGravity, o padrão é:
> *Abra o workflow de [nome-do-agente] em `.agent/workflows/` e carrega como contexto desta sessão.*

---

## 🔁 Passo 4 — O Fluxo de Trabalho Completo no AntiGravity

### Fase 1 — Planejamento
Ative os agentes de estratégia em sequência:
`@analyst` → briefing e mapeamento de requisitos
`@pm` → priorização e escopo
`@architect` → arquitetura técnica
`@ux-expert` → (se aplicável) design e UX

**Output esperado:** documentos salvos em `/docs` — PRD, arquitetura, critérios de aceite.

### Fase 2 — Preparação das Stories
```text
@aios-master *create-story 
```
Ou via `@sm` para criar histórias hiperdetalhadas a partir do PRD.

Cada story fica em `docs/stories/` e contém:
- Critérios de aceite
- Checkboxes de progresso `[ ]`
- Lista de arquivos envolvidos
- Dependências mapeadas

### Fase 3 — Desenvolvimento (AntiGravity)
Aqui é onde o AntiGravity vive. Para cada story:
1. Abra o arquivo da story no contexto
2. Ative o workflow do `@dev`:
   > *Carregue `.agent/workflows/dev.md` e execute a story `docs/stories/STORY-42.md`*
3. Ao finalizar, troque para `@qa`:
   > *Carregue `.agent/workflows/qa.md` e revise a entrega da `STORY-42`*
4. Marque os checkboxes conforme cada critério for cumprido: `[ ]` → `[x]`

> [!WARNING]
> **Regra crítica:** Nunca misture dois agentes na mesma instrução. Troque explicitamente, um de cada vez.

---

## ✅ Passo 5 — Validação Manual (O Substituto dos Hooks)

No AntiGravity, não há hooks automáticos. Você compensa com validadores manuais.

**Antes de iniciar qualquer sessão:**
```bash
npx aios-core doctor 
```

**Verificar paridade de configuração:**
```bash
npm run validate:parity 
```

**Após alterações nos agentes:**
```bash
npm run sync:ide:antigravity
npm run sync:ide:check 
```

**Ao finalizar uma story:**
```bash
npm run validate:parity
npm test
npm run lint 
```

---

## 🔑 Passo 6 — Configuração do MCP (Google)

O AntiGravity tem suporte nativo a MCP via Google. Configure em `.antigravity/antigravity.json`:

```json
{
  "mcpServers": {
    "context7": {
      "url": "https://mcp.context7.com/sse"
    }
  }
}
```

Verifique o status do MCP a qualquer momento:
```bash
aios mcp status 
```

---

## 🛡️ Passo 7 — Quality Gates no AntiGravity

O sistema de qualidade do AIOS tem 3 camadas. Todas funcionam no AntiGravity — mas você as aciona manualmente:

**Camada 1 — Local (antes de cada commit):**
```bash
npm run lint
npm run build 
```

**Camada 2 — Pré-push (antes de subir o código):**
```bash
npm run validate:parity
npm test 
```

**Camada 3 — CI/CD (automático no PR/merge):**
Esta camada roda sozinha via GitHub Actions.

---

## ⚡ As 5 Regras de Ouro do AntiGravity

1. **Contexto explícito sempre** — reabra o workflow do agente e o arquivo de story a cada sessão. Nunca assuma que o sistema "lembrou".
2. **Um agente por vez** — troque explicitamente entre @dev, @qa, @architect. Misturar quebra o contexto.
3. **Stories são a fonte da verdade** — se não está documentado na story, não existe para o agente.
4. **Valide antes de fechar a sessão** — rode `npm run validate:parity` antes de encerrar qualquer sessão de desenvolvimento.
5. **Atualize antes de projetos novos** — `npx aios-core@latest install` é o primeiro comando de todo projeto relevante.

---

## 🔍 Diagnóstico Rápido de Problemas

**Agente não responde corretamente:**
```bash
ls .agent/workflows/
npm run sync:ide:antigravity 
```

**Sync com conflito:**
```bash
npm run sync:ide -- --dry-run
```

**Rebuild completo de contexto:**
```bash
rm -rf .aios-core/core/cache/
aios rebuild 
```
