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

## Notas técnicas

- Arquivo único HTML autocontido (sem framework, sem build)
- Persistência: JSONBin.io (bin `fin-financas`, conta Renato)
- Deploy: GitHub Pages em `olandrones.github.io/fin`
- Sem backend, sem servidor, sem banco de dados próprio
