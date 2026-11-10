# CLI

O `uv sync` instala três comandos. Todos aceitam `--help` e leem também as
[Variáveis de ambiente](environment-variables.md).

## `tteso-mercado`

A fase de programação do dia seguinte: a negociação por decomposição dual e, com
`--settle-dir`, a liquidação. Módulo `market_opentes.dual`.

| Opção | Padrão | O que faz |
|---|---|---|
| `--config` | `market_opentes/dados/ieee13/config.json` | `config.json` do caso (alocação do armazenamento) |
| `--alpha` | `0.6` (`MARKET_ALPHA`) | passo do subgradiente |
| `--eps` | `1e-4` (`MARKET_EPS`) | tolerância do critério absoluto |
| `--max-rounds` | `300` | teto de rodadas |
| `--scenarios` | `1` | cenários por prosumidor; `1` é determinístico, `3` gera nove cenários de preço e potência |
| `--step-rule` | `constant` | passo `constant` ou `diminishing` |
| `--out` | nenhum | grava o histórico das rodadas em JSON |
| `--settle-dir` | nenhum | grava a liquidação e os dados das figuras nesta pasta |
| `--ck-eur` | nenhum (`MARKET_CK_EUR`) | calibração de `Ck` em EUR/(kW²·h); sem ela, o DLMP é sinal |

## `tteso-operacao`

Refaz a negociação e roda a fase de operação nos intervalos pedidos. Módulo
`market_opentes.operation`.

| Opção | Padrão | O que faz |
|---|---|---|
| `--config` | `market_opentes/dados/ieee13/config.json` | `config.json` do caso |
| `--first` | `0` | primeiro intervalo de 15 min (0 a 95) |
| `--count` | `96` | quantos intervalos |
| `--scenarios` | `1` | cenários por prosumidor na negociação |
| `--realized-day` | `0` | dia do reservatório usado como realização, com `MARKET_REALIZED_MODE=day` |
| `--max-rounds` | `300` | teto de rodadas da fase de programação |
| `--out` | nenhum | grava o registro de cada intervalo em JSON |

## `tteso-figuras`

As figuras de uma pasta de resultados. Módulo `market_opentes.figuras`.

| Opção | Padrão | O que faz |
|---|---|---|
| `--resultados` | `resultados/mercado` | pasta com os resultados da negociação e, se houver, da co-simulação |
| `--links` | `tisch_links.csv` do caso | topologia de rádio para a figura de erro de pacote |
| `--out-dir` | `<resultados>/figuras` | onde gravar as figuras |

Cada figura é pulada quando falta o arquivo de que ela depende
([Figuras de uma co-simulação](../how-to-guides/figures-from-co-simulation.md)).

## Pelo módulo

Os três comandos também rodam como módulos, sem os atalhos:

```bash
uv run python -m market_opentes.dual --settle-dir resultados/mercado
uv run python -m market_opentes.operation --first 40 --count 8
uv run python -m market_opentes.figuras --resultados resultados/mercado
```
