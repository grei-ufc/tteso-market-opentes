# Arquivos de dados

O caso IEEE 13 vem em `market_opentes/dados/ieee13/`, instalado junto do pacote.
São cópias dos arquivos do co-simulation-opentes
([Atualizar os dados do caso](../how-to-guides/update-case-data.md)).

| Arquivo | Conteúdo |
|---|---|
| `force.json` | a topologia: barras, fases, linhas, transformadores e o nome de cada barra no OpenDSS |
| `config.json` | a alocação do armazenamento: de rede e de prosumidor, potência e capacidade |
| `load_kw.csv` | carga de cada barra nos 96 intervalos de 15 min, uma coluna por barra |
| `pv_kw.csv` | geração fotovoltaica de cada barra, no mesmo formato |
| `spot_price.csv` | o preço do mercado spot em cada intervalo, coluna `price`, em EUR/MWh |
| `scenario_pool.npz` | nove dias sintéticos de carga, geração e preço (`nodes`, `load`, `pv`, `price`), para os cenários do prosumidor e a demanda realizada |
| `sensitivity_day.npz` | a matriz de sensibilidade por barra: dV/dP (`s`), dV/dQ (`s_q`), a tensão de partida (`v0`) e as derivações do regulador (`taps`, `reguladores`) em cada intervalo |
| `sensitivity_fase.npz` | o mesmo por fase: linhas por (barra, fase) em `rows`, colunas pelas portas em `ports` |

A análise por barra (`MARKET_FASE=0`) lê `sensitivity_day.npz`; a por fase, o
`sensitivity_fase.npz`.

## De onde vêm

No co-simulation-opentes, o `gen_ieee13_market.py` monta o caso a partir do
circuito oficial do IEEE 13 Node Test Feeder, e o `sensitivity.py` mede as
matrizes por perturbação no OpenDSS: quanto a tensão de cada barra (ou fase)
muda com 1 kW injetado em cada ponto. As decisões de montagem estão em
[O caso IEEE 13](https://grei-ufc.github.io/co-simulation-opentes/explanation/ieee13-case/).
