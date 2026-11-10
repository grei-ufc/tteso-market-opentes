# Comparando formulações

O mesmo mercado pode olhar a tensão por barra, como na tese de referência, ou
por fase, com dois modelos de inversor. Este tutorial roda as três negociações
e compara o que cada uma custa.

| Formulação | Variáveis |
|---|---|
| por barra | `MARKET_FASE=0` |
| por fase, inversor de quatro fios (padrão) | nenhuma |
| por fase, inversor de três fios | `MARKET_DISPOSITIVO=equilibrado` |

O porquê de cada uma está em
[O modelo de mercado](../explanation/market-model.md#a-analise-por-fase).

## Rodar

```bash
uv run tteso-mercado --settle-dir resultados/por_fase
MARKET_FASE=0 uv run tteso-mercado --settle-dir resultados/por_barra
MARKET_DISPOSITIVO=equilibrado uv run tteso-mercado --settle-dir resultados/tres_fios
```

A análise por barra usa a matriz `sensitivity_day.npz`; as duas por fase, a
`sensitivity_fase.npz`. As duas vêm no pacote.

## Comparar

| Formulação | Rodadas | Energia deslocada |
|---|---:|---:|
| por barra | 48 | 6.903 kWh |
| por fase, quatro fios | 42 | 7.527 kWh |
| por fase, três fios | 63 | 8.987 kWh |

No modelo linearizado do operador, as três formulações terminam sem violação: a
coluna de violações da última rodada chega a `(0, 0)`. A diferença entre elas
aparece no fluxo de potência não linear, que este repositório não roda. Na
co-simulação, a negociação por barra deixa 134 leituras de fase acima de
1,03 pu, porque vigia só a média das fases, e a por fase deixa 9
([Resultados do co-simulation-opentes](https://grei-ufc.github.io/co-simulation-opentes/results/)).

O preço da análise por fase é deslocar 9% mais energia que a por barra. O
inversor de três fios desloca 19% mais que o de quatro, porque uma injeção igual
nas três fases mexe também nas fases que já estavam dentro da faixa.

## As figuras

```bash
uv run tteso-figuras --resultados resultados/por_barra
uv run tteso-figuras --resultados resultados/por_fase
```

Na análise por fase, as figuras de tensão e de preço sombra têm um painel por
fase; por barra, uma superfície barra × tempo.
