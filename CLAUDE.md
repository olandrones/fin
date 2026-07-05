# Projeto: Dashboard de Finanças Pessoais (Fin Casa)

## Sobre o usuário

Renato é estudante de nutrição (5º período) com TDAH. Trabalha com números de forma prática e aprecia dashboards visuais compactos. O app deve ser direto, sem fluff, com feedback visual imediato e persistência automática de dados.

## Objetivo do projeto

Dashboard de fluxo de caixa pessoal em **única página HTML autocontida** que:
- Projeta saldo em 53 semanas (Jun/2026 – Jun/2027) com granularidade semanal
- Calcula juros do cheque especial Itaú (8% a.m., cobrados no dia 4)
- Sincroniza dados automaticamente: localStorage (offline-first) + JSONBin (cloud)
- Suporta despesas/receitas recorrentes com data de início e término
- Simula empréstimos com calculadora de PMT

## Arquivo principal

- **index.html**: Arquivo único, ~2300 linhas, contém:
  - CSS inline (dark mode, responsive grid/flex)
  - JavaScript vanilla (sem dependências)
  - localStorage + JSONBin sync
  - Modal unificado para novo entrada (despesa/receita)

## Funcionalidades implementadas

### Estrutura de dados (state)
```
{
  startBalance: number,              // Saldo em 2026-07-04
  overdraftLimit: 11700,             // Limite do cheque especial
  overdraftRate: 8,                  // Taxa mensal cheque especial
  loanAmount: 0,                     // Valor empréstimo simulado
  loanRate: 6.5,                     // Taxa empréstimo
  loanInstallments: 12,              // Número de parcelas
  expenses: [{                       // Saídas de caixa
    name, value, day, recurring, 
    intervalDays, startDate, endDate, paid
  }],
  extraIncomes: [{                   // Receitas extras
    name, value, day, recurring,
    intervalDays, startDate, endDate
  }]
}
```

### Fluxo de projeção (computeData)
1. **Pass 1** (Income/Expenses): Agrega ganhos (salários, VR, extras, empréstimo) e despesas por semana
2. **Pass 2** (Interest): Calcula juros do cheque especial no dia 4 de cada mês (sobre saldo anterior)
3. **Balance**: Acumula saldo dia a dia; "hoje" é ponto de referência (apenas semanas futuras afetam projeção)

**Importante**: Saldo inicial ("Saldo hoje") é input do usuário e serve como base. Semanas anteriores a hoje usam este saldo; semanas futuras acumulam a partir dele.

### Rendering

#### renderChart(wd)
- Barra de meses acima (Jan, Fev, Mar, ...) 
- 53 barras coloridas: verde (>2000), amarelo (0-2000), vermelho (<0)
- Linha de zero no meio
- Tooltip: "Semana XX/YY: R$ ZZZZ"

#### renderTable(res)
- Resumo: salários, VR, extras, empréstimo, saídas totais, net, saldo, margem especial
- Detalhamento: linhas de cada despesa/receita com valores por semana
- **Clicável**: Clicar em linha de despesa incrementa `paid` (0 → 1 → 2 → ... → ✓ pago → 0)
- Mostra contador "X/Y parcelas" ou "✓ pago" em cinza/verde

#### Modal unificado (openModal)
- Campo comum: nome, valor, dia (1-30)
- Recorrência: checkbox + intervalDays (dias entre ocorrências)
- Data de término: opcional (sem fim = recorrência indefinida)
- Salva ao clicar OK; atualiza tabela e gráfico

### Cálculos especiais

#### getOccurrenceWeeks(startDate, recurring, intervalDays, endDate)
Retorna array de índices de semanas onde entrada ocorre. Usado para:
- Renderizar despesas no gráfico
- Calcular total de parcelas para "mark as paid"

#### calcPMT(pv, im, n)
Fórmula: PMT = PV × [i(1+i)^n] / [(1+i)^n - 1]
Calcula prestação mensal de empréstimo

#### R(v) e BR(v)
- BR: Formata número para R$ com 2 casas (locale pt-BR)
- R: Retorna "R$ XXX,XX" ou "–R$ XXX,XX" para negativos

### Persistência

**localStorage** (`finCasa_v5`):
- Cache imediato: toda alteração salva em JSON
- Chave única: última alteração vence (campo `_ts`)

**JSONBin** (JBIN_ID, JBIN_KEY):
- PUT a cada 800ms (debounce)
- Headers X-Master-Key + X-Access-Key
- Merge-on-load: localStorage vs JSONBin, ganha o mais recente

**Merge strategy**: `mergeState(s)` sobrescreve apenas campos definidos, preservando dados não sincronizados

## Features recentes

### Mark as paid (clicável)
- Campo `paid` (0 a totalInstallments) em cada expense
- Clicar na linha alterna: red → green/gray → red
- Mostrar "X/Y parcelas" no nome da despesa
- CSS: `.sub-exp-paid` (verde), `.paid-row` (opacity 0.6)

### Tentativa: Popup compacto (REMOVIDO)
- Foi criado para mostrar 13 meses em diverging bars (ganhos acima, gastos abaixo)
- Problema: desalinhamento temporal com semanas (não sincronizava com "hoje")
- Solução: Remover (renderCompactChart, toggleCompactPopup, Ctrl+S listener)
- Status: **Deletado em commit 773fe23**

## Valores atuais

Entrada em `state.expenses` (DEFAULT_EXPENSES):
- Sal1: R$ 4.541, dia 5, mensal (recorrente, sem fim)
- Sal2: R$ 5.490, dia 15, mensal
- Sal3: R$ 4.047, dia 30, mensal
- VR: R$ 1.221, dia 30, mensal
- Aluguel: R$ 1.200, dia 5, mensal
- Condomínio: R$ 280, dia 15, mensal
- (demais despesas variáveis)

## Próximos passos possíveis

1. **Comparativo mês-a-mês**: Visualizar variações de receita/despesa entre períodos
2. **Categorias de despesa**: Agrupar por tipo (moradia, transporte, etc.)
3. **Alertas**: Notificar quando saldo vai para negativo / ultrapassa margem especial
4. **Export/Import avançado**: CSV, gráficos em imagem
5. **Modo dark/light automático**

## Estilo de código

- Sem transpilação (ES6, arrow functions, template literals OK)
- Variáveis globais minimizadas: `state`, `weeks`, `WEEK_STARTS`, `MESES`, `DEFAULT_EXPENSES`
- Funções puras onde possível (getOccurrenceWeeks, calcPMT)
- State mutations direto em state (togglePaid modifica state.expenses[idx].paid)
- Render centralizado: `render()` chama todos os sub-renders, triggado por `onStateChange()`

## Testes manuais recomendados

1. Adicionar receita extra (data futura) → aparece no gráfico
2. Marcar despesa como paga 2x → contador muda 0/4 → 1/4 → 2/4 → 3/4 → ✓ pago
3. Desconectar internet → salva local (badge "💾 salvo local")
4. Reconectar → sincroniza (badge "☁️ sincronizado")
5. Abrir em outra aba mesma máquina → dados compartilhados (localStorage)
6. Alterar overdraftRate → simular novos juros no dia 4

## Limitações conhecidas

1. **Popup compacto**: Tentativa de visão agregada por mês falhou (temporal mismatch). Deletado.
2. **Semanas fixas**: Começa sempre Jun/04/2026. Mudar período requer refactor de WEEK_STARTS e LAST_DATE.
3. **Sem categorias**: Todas despesas em único listão; não há groupby.
4. **Sem notificações desktop**: Apenas badges na interface.
5. **Sync JSONBin**: Sem versionamento ou histórico (sobrescreve).
