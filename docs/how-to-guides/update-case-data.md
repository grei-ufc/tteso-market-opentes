# Atualizar os dados do caso

O caso IEEE 13 vem dentro do pacote, em `market_opentes/dados/ieee13/`. Os
arquivos são copiados do co-simulation-opentes, onde são gerados a partir do
circuito no OpenDSS. Este repositório não gera o caso.

## Quando atualizar

Depois de o caso mudar no co-simulation-opentes: circuito, curvas de PV,
alocação de armazenamento ou matrizes de sensibilidade. Se as cópias divergirem,
os dois repositórios passam a negociar sobre casos diferentes.

## Como

No co-simulation-opentes, regenere o caso
([Regenerar o caso IEEE 13](https://grei-ufc.github.io/co-simulation-opentes/how-to-guides/regenerate-case/))
e copie os oito arquivos:

```bash
CASO=../co-simulation-opentes/simulators/grid-opentes/src/data/13Bus
for f in config.json force.json load_kw.csv pv_kw.csv spot_price.csv \
         scenario_pool.npz sensitivity_day.npz sensitivity_fase.npz; do
  cp "$CASO/$f" market_opentes/dados/ieee13/
done
```

Depois, rode a negociação e confira o número de rodadas e os arquivos de
liquidação contra o `./run.sh centralizado` de lá.

## Outro caso

Para negociar sobre outro alimentador sem substituir o IEEE 13, aponte as duas
variáveis para a pasta dele, que precisa ter os mesmos oito arquivos:

```bash
MARKET_DATA_DIR=/caminho/do/caso MARKET_GRID_DIR=/caminho/do/caso \
  uv run tteso-mercado --config /caminho/do/caso/config.json
```

O formato de cada arquivo está em [Arquivos de dados](../reference/data-files.md).
