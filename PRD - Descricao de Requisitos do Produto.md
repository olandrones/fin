# PRD — Finanças Casa (fin)
**Versão:** 1.1 · **Status:** Em desenvolvimento · **Owner:** Renato

---

## Visão do Produto

Painel pessoal de finanças domésticas com projeção semanal de fluxo de caixa. Uma única tela, uma única rolagem. Feito para quem tem renda variável, quer controle sem complexidade, e precisa enxergar o futuro próximo com clareza.

**Princípio central:** ver rápido, entender rápido, agir rápido.

---

## Usuário

Pessoa física com renda mista (salário fixo + freelances esporádicos), despesas recorrentes e variáveis, e necessidade de acompanhar se o mês vai fechar no azul ou no cheque especial. Não quer planilhas complexas. Quer uma visão de 12 meses numa tela só.

---

## MVP — Funcionalidades Atuais (v1.0)

| # | Funcionalidade | Status |
|---|----------------|--------|
| 1 | Projeção semanal de 53 semanas (Jun/26–Jun/27) | ✅ |
| 2 | Salários fixos + VR alimentação hardcoded | ✅ |
| 3 | Despesas recorrentes com intervalo configurável (dias) | ✅ |
| 4 | Ganhos extras com data e recorrência | ✅ |
| 5 | Simulação de empréstimo (PMT) | ✅ |
| 6 | Saldo acumulado semana a semana | ✅ |
| 7 | Margem do cheque especial com limite configurável | ✅ |
| 8 | Juros do cheque especial (% a.m., cobrado dia 4) | ✅ |
| 9 | Persistência na nuvem via JSONBin | ✅ |
| 10 | Export/Import JSON como backup manual | ✅ |
| 11 | Gráfico de barras visual da projeção | ✅ |
| 12 | Cards de resumo (pior semana, saída do especial, projeção final, menor margem) | ✅ |

---

## Backlog — Próximas Releases

### Release 1.2 — Segurança e Acesso

**HU-01 · Proteção por PIN**
> Como usuário, quero que a página solicite um PIN antes de carregar qualquer dado, para que minha situação financeira não fique exposta se alguém abrir o link.

- PIN de 4 dígitos configurável
- Dados do JSONBin só são puxados após autenticação
- Sessão mantida enquanto a aba estiver aberta
- **Prioridade:** Alta · **Esforço:** Baixo

---

### Release 1.3 — Visualização em Camadas

**HU-02 · Modo Macro (visão compacta)**
> Como usuário, quero alternar entre visão detalhada e visão macro para ter uma leitura rápida do cenário sem me perder nos detalhes.

- Botão "Zoom out" colapsa detalhamento de despesas e mostra só: Entradas / Saídas / Saldo / Margem
- Botão "Zoom in" expande tudo
- **Prioridade:** Média · **Esforço:** Baixo

**HU-03 · Destaque de semanas críticas**
> Como usuário, quero que semanas com saldo crítico (< R$500 de margem no especial) sejam visualmente destacadas na tabela.

- Bordas ou ícone de alerta nas colunas críticas
- **Prioridade:** Média · **Esforço:** Baixo

---

### Release 1.4 — Renda Variável

**HU-04 · Categoria "Renda variável esperada"**
> Como usuário, quero registrar expectativas de receita (freelance, 13º, bônus) com probabilidade estimada para ver cenários otimista/realista/pessimista.

- Campo "probabilidade %" por ganho extra
- Toggle: ver projeção com 100% ou com % real de cada ganho
- **Prioridade:** Média · **Esforço:** Médio

**HU-05 · Despesas inesperadas (buffer)**
> Como usuário, quero reservar um valor mensal para imprevistos (carro, médico) sem precisar adicionar entrada por entrada.

- Campo "Buffer mensal de imprevistos (R$)" no painel de configuração
- Lançado automaticamente na última semana de cada mês
- **Prioridade:** Alta · **Esforço:** Baixo

---

### Release 1.5 — Histórico e Tendência

**HU-06 · Registro do real vs. projetado**
> Como usuário, quero poder marcar o que de fato aconteceu em semanas passadas para ver o desvio entre projeção e realidade.

- Coluna editável "real" para semanas passadas
- Linha "Desvio" = real − projetado
- **Prioridade:** Baixa · **Esforço:** Alto

---

## Não está no escopo agora

- Multi-usuário ou login com conta
- App mobile nativo
- Integração com banco (Open Finance / API Itaú)
- Categorias de gastos com gráficos de pizza
- Múltiplas telas ou navegação entre seções

---

## Comentários Hermes (15/06/2026)

> Adicionados por Hermes após revisar o PRD. Convenção: **não altero o que o Claude escreveu** — só acrescento seções com `> ` e assino no final. Discordâncias são educativas, não competição.

### 👍 Pontos fortes do PRD

- **HU-04 (Dias até o estouro)** é a feature que vai mudar a vida do Renato. Em vez de "saldo: −R$ 7.750" (abstrato), "saldo dura 15 dias" (concreto). Combinar com **alerta de receita atrasada** cria accountability.
- **Backlog com 4 releases** (1.2–1.5) tá realista. Evita Big-Bang-PRD que nunca sai do papel.
- **HU-01 (Dashboard resumo)** certa como base: precisa de TUDO antes de qualquer feature avançada.

### ⚠️ Pontos de atenção / sugestões

1. **HU-03 (Marcação manual receita/despesa):** considere **categorias pré-definidas** (Moradia, Alimentação, Transporte, Saúde, Educação, Lazer, Dívidas) — categoria custom pode ser adicionada depois, mas pra MVP 1.0 categorias fixas aceleram muito a UX.

2. **HU-06 (Importar extrato):** boa feature, mas sugere-se **HU-06.1 (Importar OFX)** ou **HU-06.2 (Colar CSV do Nubank/Inter)** como duas opções menores. Extrato OFX é formato universal; CSV é onde a galera tem de fato.

3. **HU-10 (Notificação 3 dias antes):** genial e simples. Considerar **HU-10.1 (Push via Telegram Bot)** — Renato JÁ tem bot dele configurado, é meio caminho andado.

4. **Backlog 1.3 (Visualização em camadas):** complementam bem o dashboard. Sugestão de roadmap visual:
   - Camada 1: Saldo + margem
   - Camada 2: Burn rate (gasto/mês)
   - Camada 3: Projeção (calendário)
   - Camada 4: Saúde financeira (score 0-100)

5. **Backlog 1.4 (Renda variável):** FREELANCE-irregular é o caso real do Renato. Sugestão: ao invés de "renda variável" genérico, tratar como **HU-X.Y (Renda freelance com meses secos)** — template "esse mês caiu X, semana Y, dia Z" + auto-cálculo de "saldo mínimo necessário".

6. **Backlog 1.5 (Histórico):** OK no final. Sugiro **HU-X.Z (Comparativo mês a mês)** como MUST — o que subiu, o que desceu, % de mudança por categoria.

7. **Renda fixa hardcoded no JS (MVP):** mover pra **jsonbin** com campo `renato: { renda_mensal, dia_pagamento, tipo }` e `alice: { renda_mensal, dia_pagamento, tipo }`. Aí o agente financeiro consegue ler/atualizar.

8. **Cal (.ics export) — IDÉIA FORA DO BACKLOG:** o Renato pediu **calendário das contas aparecendo** (.ics export). Adicionar à Release 1.2 ou criar Release 1.6 (Integrações externas). Pró: integra com Google Calendar, Apple Calendar, etc.

9. **Visibilidade diferentes (esposa):** modelar como `perfis: { renato: "admin", alice: "viewer_renato" }` no jsonbin. Alice vê o que Renato vê, mas sem editar. Perfil "viewer_conjunta" pode ser release 1.7 (Família).

### 🚀 Roadmap sugerido (acumulado)

```
MVP 1.0   ✅ FEITO    Dashboard + Cadastro + Alerta
1.2       🔜 PRÓXIMA  PIN + Calendário .ics (sugestão)
1.3       📅          Camadas de visualização (4 níveis)
1.4       📅          Renda variável (freelance + meses secos)
1.5       📅          Histórico + Comparativo mês a mês
1.6       💡          Telegram Bot (notificações reais)
1.7       💡          Perfis família (Alice)
1.8       💡          Importar OFX/CSV
```

### 🤝 Acordo de colaboração Hermes ↔ Claude

- Claude escreve código + atualiza PRD quando implementar
- Hermes comenta PRD + atualiza `visao.html` + mantém `handoff-claude.md`
- Renato arbitra discordâncias
- Cada agente **assina** o que escreveu (data + nome)
- Arquivos compartilhados: PRD, `visao.html`, `handoff-claude.md`
- Arquivos separados: `index.html` (Claude), `financas_casa.html` (Claude), skill do agente financeiro (Hermes)

> — Hermes, 15/06/2026 02:20 BRT (gol de placa do Renato sobre "vocês são colegas, não rivais")

---

## Notas técnicas

- Arquivo único HTML autocontido (sem framework, sem build)
- Persistência: JSONBin.io (bin `fin-financas`, conta Renato)
- Deploy: GitHub Pages em `olandrones.github.io/fin`
- Sem backend, sem servidor, sem banco de dados próprio
