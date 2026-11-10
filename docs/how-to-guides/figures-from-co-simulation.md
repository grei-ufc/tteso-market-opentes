# Figuras de uma co-simulação

O `tteso-figuras` desenha também as figuras que dependem da plataforma completa:
a tensão na operação, a cada 15 min no OpenDSS, e as mensagens entre os agentes.
Basta apontá-lo para a pasta de resultados de uma execução do
co-simulation-opentes.

```bash
uv run tteso-figuras --resultados ../co-simulation-opentes/output/market_omnet
```

| Arquivo na pasta | Figuras que ele habilita |
|---|---|
| `result_negociado.csv` | `tensao_operacao.png`, `demanda_operacao.png` |
| `msg_trace.csv` (e `run.json`) | `mensagens.png`, `mensagens_distribuicao.png` |
| `figuras.npz`, `market_series.csv` | as da negociação e dos mercados |

A figura de erro de pacote por distância (`per_distancia.png`) lê a topologia de
rádio do caso; aponte-a com `--links`:

```bash
uv run tteso-figuras --resultados ../co-simulation-opentes/output/market_omnet \
  --links ../co-simulation-opentes/simulators/grid-opentes/src/data/13Bus/tisch_links.csv
```

As figuras vão para `<resultados>/figuras/`, ou para a pasta de `--out-dir`.
