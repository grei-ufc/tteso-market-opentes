# tteso-market-opentes

**Mercado transativo do OpenTES, executável sem os demais simuladores**

tteso-market-opentes é o repositório do time **TTESO** (Sistemas Transativos e
Otimização) do projeto [OpenTES](https://github.com/grei-ufc/general-opentes),
do GREI-UFC. Ele traz o modelo do mercado transativo: os modelos de otimização
de cada agente, escritos em [Pyomo](https://www.pyomo.org/), a decomposição
dual que os coordena, a fase de operação, a liquidação e as figuras.

A formulação vem da tese de Lucas S. Melo (UFC, 2022, cap. 6), levada ao
alimentador IEEE 13 barras com análise por fase, para rede desequilibrada. O
mesmo código roda dentro da plataforma
[co-simulation-opentes](https://grei-ufc.github.io/co-simulation-opentes/); aqui
ele roda sozinho, com o caso de estudo embutido, sem Docker, OpenDSS ou mosaik.

## O que o projeto permite

- Rodar a negociação do dia seguinte entre prosumidores, concentrador, operador
  da rede e mercado num laço único, por decomposição dual
- Analisar a tensão por barra (como na tese) ou por fase, com inversor de quatro
  ou de três fios
- Rodar a fase de operação, em que a demanda realizada difere da prevista e o
  operador corrige o desvio
- Liquidar os mercados bilateral e spot e calcular o preço locacional (DLMP) e a
  flexibilidade de cada prosumidor
- Gerar as figuras de tensão, preço sombra, negociação e mercados, inclusive a
  partir dos resultados de uma co-simulação

## Início rápido

```bash
git clone https://github.com/grei-ufc/tteso-market-opentes.git
cd tteso-market-opentes
uv sync
export MARKET_SOLVER_PATH=$HOME/IBM/CPLEX_Studio2211/cplex/bin/x86-64_linux/cplex
uv run tteso-mercado --settle-dir resultados/mercado
```

A negociação converge em 42 rodadas. Consulte [Instalação](getting-started/installation.md)
para o solver e [Primeira Negociação](getting-started/first-negotiation.md) para
o passo a passo.

## Repositórios relacionados

| Repositório | O que tem |
|---|---|
| [co-simulation-opentes](https://grei-ufc.github.io/co-simulation-opentes/) | a plataforma completa, com agentes, rede de comunicação e rede elétrica |
| [tteso-agents-opentes](https://grei-ufc.github.io/tteso-agents-opentes/) | os agentes PADE que chamam este pacote |

A relação entre os três está em [Os repositórios do TTESO](explanation/repositories.md).

## Licença

MIT License, Smart Grids Research Group - UFC (Universidade Federal do Ceará), 2025
