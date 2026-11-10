## #TTESO: mercado transativo do OpenTES

[![uv](https://img.shields.io/endpoint?url=https://raw.githubusercontent.com/astral-sh/uv/main/assets/badge/v0.json)](https://github.com/astral-sh/uv)
[![Grei](https://img.shields.io/badge/-GREI-Black?logo=INSPIRE&logoColor=blue&color=42b85a&labelColor=white&style=flat)](https://www.linkedin.com/company/grei-ufc/?originalSubdomain=br)
[![image](https://img.shields.io/badge/-Python%20Version%20|%203.12.11-42b85a?logo=Python&logoColor=fbec41&color=42b85a&labelColor=grey&style=flat)](https://www.python.org/downloads/release/python-31112/)
[![Discord](https://img.shields.io/badge/Discord-%235865F2.svg?logo=discord&logoColor=white)](https://discord.com/channels/1415180099644297368/1415431164717564065)

**[Documentação do projeto](https://grei-ufc.github.io/tteso-market-opentes/)**: instalação, tutoriais, guias práticos, referência dos comandos, das variáveis e da API, e a formulação do mercado.

Repositório do time **TTESO** (Sistemas Transativos e Otimização) do projeto
[OpenTES](https://github.com/grei-ufc/general-opentes), do Grupo de Redes
Elétricas Inteligentes (GREI) da UFC. Aqui fica o **modelo do mercado
transativo**: os modelos de otimização de cada agente, a decomposição dual que
os coordena, a fase de operação, a liquidação e as figuras de resultado.

A formulação vem da tese de Lucas Silveira Melo (UFC, 2022, cap. 6), levada ao
alimentador IEEE 13 barras com análise por fase, para rede desequilibrada. Os
comentários do código citam as equações da tese pelo número, como (6.25).

## Onde este repositório se encaixa

O mercado foi desenvolvido dentro da plataforma de co-simulação do projeto e
extraído dela para cá. São três repositórios:

| Repositório | O que tem |
|---|---|
| [co-simulation-opentes](https://github.com/grei-ufc/co-simulation-opentes) | a plataforma completa: agentes (PADE), comunicação (OMNeT++), rede elétrica (OpenDSS) e Mosaik, cada um em seu contêiner. O mercado roda lá com as mensagens atravessando a rede simulada e a operação conferida no fluxo de potência |
| **tteso-market-opentes** (este) | só o mercado, sem os outros simuladores: a negociação inteira num laço único, a operação sobre o modelo linearizado e a liquidação |
| [tteso-agents-opentes](https://github.com/grei-ufc/tteso-agents-opentes) | os agentes PADE que chamam este pacote, cada um no seu papel, trocando mensagens FIPA |

O pacote `market_opentes` deste repositório é o mesmo código da pasta
`simulators/market-opentes` do co-simulation. A diferença é que o caso de
estudo vem junto dele, em `market_opentes/dados/ieee13/`, então nada aqui
depende de Docker, do OpenDSS ou dos outros contêineres.

## O que há aqui

```
market_opentes/
  config.py              o caso, a faixa de tensão e as chaves do armazenamento
  optimization.py        os modelos: prosumidor (6.1)-(6.9), concentrador (6.25)-(6.26), operador (6.27)-(6.29)
  dual.py                a decomposição dual, com o subgradiente (6.30)
  operation.py           a fase de operação, intervalo a intervalo, e a demanda realizada
  scenarios.py           geração e redução de cenários do prosumidor (Kantorovich)
  settlement.py          transações nos mercados bilateral e spot, preço locacional (DLMP) e flexibilidade
  figuras.py             as figuras de uma execução
  figuras_mensagens.py   as figuras do traço de mensagens (só com resultados da co-simulação)
  dados/ieee13/          o caso IEEE 13 barras (ver "Dados do caso")
docs/                    a documentação (MkDocs), publicada no GitHub Pages
FUNDAMENTACAO_TEORICA/   exercícios de meta-heurísticas, programação linear (Pyomo, CPLEX) e Mosaik
```

## Instalação

Requisitos: Python 3.12, [uv](https://github.com/astral-sh/uv) e um solver
(ver abaixo).

```bash
git clone https://github.com/grei-ufc/tteso-market-opentes.git
cd tteso-market-opentes
uv sync
```

### Solver

O padrão é o **IBM CPLEX**: o modelo do prosumidor é inteiro-misto, e os do
concentrador e do operador são quadráticos. A licença acadêmica do CPLEX é
pessoal, por isso ele não vem no repositório; cada pessoa instala o seu
(gratuito pelo IBM Academic Initiative) e aponta o executável:

```bash
export MARKET_SOLVER_PATH=$HOME/IBM/CPLEX_Studio2211/cplex/bin/x86-64_linux/cplex
```

Outro solver pode ser usado com `MARKET_SOLVER` (nome do solver no Pyomo) e
`MARKET_SOLVER_PATH`, desde que resolva problemas inteiros-mistos e
quadráticos. Os resultados de referência abaixo foram obtidos com o CPLEX 22.1.1.

## Rodar

```bash
uv run tteso-mercado --max-rounds 300 --settle-dir resultados/mercado --out resultados/mercado/history.json
uv run tteso-figuras --resultados resultados/mercado
uv run tteso-operacao --first 40 --count 16 --out resultados/operacao/log.json
```

- `tteso-mercado` roda a fase de programação do dia seguinte: cada prosumidor
  programa o seu armazenamento, e o concentrador, o operador e o mercado
  negociam por decomposição dual até o desacordo ficar abaixo do critério. A
  tabela impressa mostra, a cada rodada, o desacordo, o preço sombra máximo e a
  tensão mínima e máxima prevista. Com `--settle-dir`, grava a liquidação.
- `tteso-figuras` desenha as figuras da negociação em `resultados/mercado/figuras/`.
  Apontado para a pasta de saída de uma execução do co-simulation, desenha
  também as da operação e da comunicação.
- `tteso-operacao` refaz a negociação e roda a fase de operação nos intervalos
  pedidos: a demanda realizada difere da prevista, e o operador corrige primeiro
  com o armazenamento de rede e, se não bastar, com um leilão de um intervalo.

Os três comandos aceitam `--help`.

### O que sai

| Arquivo | Conteúdo |
|---|---|
| `history.json` | uma linha por rodada: desacordo, variação do preço sombra, tensão mínima e máxima, violações e tempo |
| `transactions.csv` | energia e custo de cada prosumidor no mercado bilateral e no spot, no dia |
| `market_series.csv` | a potência de cada prosumidor em cada mercado, por intervalo de 15 min, com os dois preços |
| `dlmp.csv` | preço locacional por barra e fase (ou por barra) e intervalo, como sinal |
| `flexibility.csv` | energia que cada porta deslocou em relação à programação inicial e o valor dessa flexibilidade, como sinal |
| `figuras.npz` | os dados das figuras |

O preço sombra sai como sinal, e não em dinheiro: com o peso `Ck = 1` da
formulação, ele é numericamente um desvio de potência. Para lê-lo como preço é
preciso calibrar o peso em moeda (`MARKET_CK_EUR`, em EUR/(kW²·h)); a
explicação está no início de `settlement.py`.

### Resultado de referência

Com a configuração padrão (por fase, inversor de quatro fios, critério
relativo), a negociação converge em **42 rodadas**, e os arquivos
`transactions.csv`, `market_series.csv`, `dlmp.csv` e `flexibility.csv` saem
idênticos aos do `./run.sh centralizado` do co-simulation-opentes (conferido em
10/11/2026).

## Variáveis de ambiente

| Variável | Padrão | Efeito |
|---|---|---|
| `MARKET_FASE` | `1` | `1`: tensão e preço sombra por barra e fase; `0`: por barra, com a média das fases |
| `MARKET_DISPOSITIVO` | `independente` | armazenamento por fase com inversor de quatro fios; `equilibrado`: três fios, potência igual nas fases |
| `MARKET_CRITERIO` | `relativo` | parada pelo desacordo relativo à potência de cada dispositivo; `absoluto`: o critério da tese |
| `MARKET_EPS_REL` | `1e-3` | tolerância do critério relativo |
| `MARKET_ALPHA` | `0.6` | passo do subgradiente |
| `MARKET_V_BACKOFF` | `1e-3` | margem nos limites de tensão do operador, contra o erro da linearização |
| `MARKET_SOLVER`, `MARKET_SOLVER_PATH` | `cplex`, vazio | solver e executável |
| `MARKET_DATA_DIR`, `MARKET_GRID_DIR` | `market_opentes/dados/ieee13` | outro caso de estudo |

Os cenários do prosumidor são escolhidos pela opção `--scenarios` dos comandos
(`1`, o padrão, é determinístico). A faixa de tensão do mercado é de 0,97 a
1,03 pu (`config.py`).

## Dados do caso

`market_opentes/dados/ieee13/` tem a topologia (`force.json`), a alocação do
armazenamento (`config.json`), as curvas de carga e de geração fotovoltaica, o
preço spot, o reservatório de dias alternativos para os cenários e as duas
matrizes de sensibilidade de tensão (por barra e por fase). Elas foram
copiadas do co-simulation-opentes, onde `./run.sh caso` as gera a partir do
circuito no OpenDSS. Se o circuito, as curvas ou a alocação mudarem lá, os
arquivos precisam ser copiados de novo para cá.

## Fundamentação teórica

A pasta `FUNDAMENTACAO_TEORICA/` guarda os exercícios de estudo do time
(meta-heurísticas como PSO e EPSO, programação linear em Pyomo e CPLEX, e
Mosaik 3.5). As bibliotecas deles ficam num grupo separado:

```bash
uv sync --group fundamentacao
```

## Referência

MELO, L. S. *Modelo de simulação computacional multidomínio para análise de
redes elétricas inteligentes com aplicação em transações econômicas de
energia*. Tese (Doutorado em Engenharia Elétrica), Universidade Federal do
Ceará, Fortaleza, 2022.

Licença MIT, em [LICENSE](LICENSE).

<div align="center">
  <a target="_blank" href="https://github.com/grei-ufc" style="background:none">
    <img src="https://github.com/grei-ufc/tsdq-dataview-opentes/blob/main/imagens/Grei2.png?raw=true">
  </a>
</div>
