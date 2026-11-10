# A fase de operação

A negociação decide o dia inteiro com base numa previsão. Na operação, a cada
15 minutos, a demanda realizada difere da prevista, e o operador confere se o
desvio viola alguma restrição de tensão. Este tutorial roda a operação num
trecho do dia e mostra como ler o resultado.

## Os dois níveis de correção

| Nível | O que o operador faz |
|---|---|
| `rede` | corrige só com o armazenamento de rede, que ele despacha diretamente |
| `leilao` | se não bastar, o mercado abre um leilão de um intervalo: a decomposição dual sobre um período, envolvendo também o armazenamento dos prosumidores |

A demanda realizada é a prevista perturbada em até ±10% por prosumidor, com
semente fixa, para que o desvio seja reproduzível
(`MARKET_REALIZED_MODE=perturb`, `MARKET_REALIZED_SPREAD=0.10`).

## Rodar

```bash
uv run tteso-operacao --first 40 --count 8 --out resultados/operacao/log.json
```

O comando refaz a negociação do dia seguinte (42 rodadas) e roda a operação
nos intervalos 40 a 47, das 10h às 12h. `--first` e `--count` escolhem o trecho;
sem eles, roda os 96 intervalos.

## Ler a saída

```text
   t  desvio kW   V antes   viol   nivel  rodadas  V depois   viol      s
  42      10.47 1.0218/1.0305      1    rede        0 1.0216/1.0290      0    0.0
  43      56.25 1.0208/1.0305      2    rede        0 1.0210/1.0290      0    0.0
  ...
8 intervalos: 4 exigiram intervencao, 4 resolvidos, 0 ainda com violacao
```

| Coluna | Significado |
|---|---|
| `t` | o intervalo de 15 min (0 a 95) |
| `desvio kW` | quanto a demanda realizada se afastou da prevista |
| `V antes` | tensão mínima e máxima com a programação acordada e a demanda realizada |
| `viol` | leituras fora de 0,97 a 1,03 pu, antes e depois da correção |
| `nivel` | `-` (nada a fazer), `rede` ou `leilao` |
| `rodadas` | rodadas do leilão, quando há |
| `V depois` | tensão mínima e máxima depois da correção |

No trecho acima, quatro intervalos passaram de 1,03 pu, e o armazenamento de
rede bastou nos quatro: a máxima voltou a 1,0290 pu.

## O que muda em relação à co-simulação

Aqui a tensão de cada intervalo vem do modelo linearizado, pela matriz de
sensibilidade. Na plataforma completa, o ponto de operação realizado é resolvido
no OpenDSS, e a operação acontece entre agentes que trocam mensagens
([co-simulation-opentes](https://grei-ufc.github.io/co-simulation-opentes/)).
Use este comando para estudar o mecanismo; os números de tensão publicados pelo
projeto vêm da co-simulação.

O registro completo de cada intervalo fica no arquivo de `--out`
([Arquivos de saída](../reference/output-files.md#log-da-operacao)).
