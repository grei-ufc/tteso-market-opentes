# Variáveis de ambiente

O pacote lê a configuração do ambiente na importação. Valem na frente de
qualquer comando: `MARKET_FASE=0 uv run tteso-mercado`.

## Formulação

| Variável | Padrão | O que muda |
|---|---|---|
| `MARKET_FASE` | `1` | `1`: tensão e preço sombra por barra e fase; `0`: por barra, com a média das fases |
| `MARKET_DISPOSITIVO` | `independente` | armazenamento por fase com inversor de quatro fios; `equilibrado`: três fios, potência igual nas fases |
| `MARKET_REGULADOR` | `previsao` | derivações do regulador fixadas pelo ponto da previsão; `neutro` as deixa em 0 |
| `MARKET_CRITERIO` | `relativo` | parada pelo desacordo relativo à potência de cada dispositivo; `absoluto`: pela variação de λ, o critério da tese |
| `MARKET_EPS_REL` | `1e-3` | tolerância do critério relativo |
| `MARKET_EPS` | `1e-4` | tolerância do critério absoluto |
| `MARKET_ALPHA` | `0.6` | passo do subgradiente |
| `MARKET_V_BACKOFF` | `1e-3` | margem nos limites de tensão do operador, contra o erro da linearização |
| `MARKET_STORAGE_PF` | `none` | fator de potência do armazenamento; `none` é a hipótese ΔQ = 0 |
| `MARKET_TERMINAL_SOC` | `1` | exige estado de carga final maior ou igual ao inicial; sem isso, as baterias descarregam juntas no último intervalo |
| `MARKET_TRAFO_KVA` | vazio | força um limite de carregamento único para todos os transformadores; vazio usa a potência de cada um no `force.json` |

## Prosumidor

| Variável | Padrão | O que muda |
|---|---|---|
| `MARKET_CVAR_BETA` | `0.0` | peso do risco (CVaR) no modelo estocástico; `0` é neutro ao risco |
| `MARKET_CVAR_ALPHA` | `0.95` | nível de confiança do CVaR |
| `MARKET_DEVIATION_PENALTY` | `0.0` | multa por desviar, no tempo real, do que foi programado; `0` é a formulação de referência |

## Operação

| Variável | Padrão | O que muda |
|---|---|---|
| `MARKET_REALIZED_MODE` | `perturb` | `perturb` perturba a programação de cada prosumidor; `day` usa um dia do reservatório de cenários |
| `MARKET_REALIZED_SPREAD` | `0.10` | no modo `perturb`, a perturbação máxima (±10%) |
| `MARKET_REALIZED_SEED` | `0` | semente da perturbação |
| `MARKET_V_ACAO_MIN`, `MARKET_V_ACAO_MAX` | `0.97`, `1.03` | limiar de ação do operador; o limite contado continua 0,97 e 1,03 pu |

## Solver, caso e preço

| Variável | Padrão | O que muda |
|---|---|---|
| `MARKET_SOLVER` | `cplex` | solver do Pyomo |
| `MARKET_SOLVER_PATH` | vazio | executável do solver, se não estiver no `PATH` |
| `MARKET_DATA_DIR`, `MARKET_GRID_DIR` | `market_opentes/dados/ieee13` | pasta de outro caso de estudo |
| `MARKET_CK_EUR` | vazio | calibração de `Ck` em EUR/(kW²·h); sem ela, o preço sombra e o DLMP saem como sinal |
| `MARKET_IGNORE_DQ` | vazio | experimento: o modelo do operador ignora dV/dQ enquanto o dispositivo injeta reativo |

A faixa de tensão do mercado, 0,97 a 1,03 pu, é fixa em `config.py`.
