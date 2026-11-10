# Arquivos de saída

## Negociação

Com `tteso-mercado --settle-dir <pasta> --out <pasta>/history.json`:

| Arquivo | Conteúdo |
|---|---|
| `history.json` | uma entrada por rodada |
| `transactions.csv` | energia e custo de cada prosumidor no mercado bilateral e no spot, no dia |
| `market_series.csv` | a potência de cada prosumidor em cada mercado, por intervalo de 15 min, com os dois preços |
| `dlmp.csv` | preço locacional por barra e fase (ou por barra) e intervalo |
| `flexibility.csv` | energia que cada porta deslocou em relação à programação inicial, e o valor dessa flexibilidade |
| `figuras.npz` | os dados das figuras: tensões, programações, λ e a trilha de cada rodada |

### Colunas

| Arquivo | Colunas |
|---|---|
| `history.json` | `round`, `d_lambda_max`, `residual_max`, `residual_rms`, `residual_rel_max`, `lambda_max`, `v_min`, `v_max`, `violations`, `seconds` |
| `transactions.csv` | `node`, `market`, `energy_kwh`, `price_eur_mwh`, `cost_eur` |
| `market_series.csv` | `node`, `t`, `bilateral_kw`, `spot_kw`, `spot_eur_mwh`, `bilateral_eur_mwh` |
| `dlmp.csv` | `node`, `fase`, `t`, `dlmp_signal`, `spot_eur_mwh`, `adder_signal` |
| `flexibility.csv` | `node`, `fase`, `energy_shifted_kwh`, `net_shift_kwh`, `value_signal`, `dlmp_mean_signal`, `dlmp_max_signal` |

A coluna `fase` só existe na análise por fase. As colunas com sufixo `_signal`
perdem o sufixo e passam a estar em EUR quando o preço é calibrado
([Calibrar o preço](../how-to-guides/calibrate-price.md)). `violations` é o par
(abaixo de 0,97 pu, acima de 1,03 pu) no modelo linearizado.

## Log da operação

Com `tteso-operacao --out <arquivo>`, uma entrada por intervalo:

| Campo | Conteúdo |
|---|---|
| `t` | o intervalo de 15 min |
| `deviation_kw` | desvio da demanda realizada em relação à prevista |
| `v_min_before`, `v_max_before`, `violations_before` | tensão e violações antes da correção |
| `level` | `-`, `rede` ou `leilao` |
| `rounds` | rodadas do leilão |
| `v_min_after`, `v_max_after`, `violations_after` | tensão e violações depois da correção |
| `seconds` | tempo de cálculo |

## Figuras

`tteso-figuras` grava em `<resultados>/figuras/`. A lista das figuras e do que
cada uma mostra está no
[co-simulation-opentes](https://grei-ufc.github.io/co-simulation-opentes/how-to-guides/generate-figures/#os-arquivos),
que usa o mesmo código.
