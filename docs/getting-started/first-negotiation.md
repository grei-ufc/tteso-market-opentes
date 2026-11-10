# Primeira Negociação

Este passo a passo roda a negociação do dia seguinte no cenário padrão (análise
por fase, inversor de quatro fios, critério de parada relativo) e gera as
figuras. Pressupõe a [Instalação](installation.md) feita e o solver apontado.

## 1. Negociar

```bash
uv run tteso-mercado --max-rounds 300 --settle-dir resultados/mercado --out resultados/mercado/history.json
```

Leva menos de um minuto. A sequência é a da tese:

1. cada prosumidor programa o seu armazenamento, uma vez;
2. a cada rodada, o concentrador e o operador reotimizam com o preço sombra
   corrente;
3. o mercado atualiza o preço sombra pelo subgradiente e testa a convergência.

O terminal mostra uma linha por rodada, com o desacordo entre concentrador e
operador, o preço sombra máximo, a tensão mínima e máxima prevista e as
violações. A última linha deve ser:

```
convergiu em 42 rodadas (|x-y|/Pmax = 9.05e-04 <= 1e-03)
```

Em seguida vem a liquidação, que avisa que o preço sombra sai como sinal (ver
[Calibrar o preço](../how-to-guides/calibrate-price.md)) e grava os arquivos.

## 2. As figuras

```bash
uv run tteso-figuras --resultados resultados/mercado
```

Grava as figuras em `resultados/mercado/figuras/`. As da operação e da
comunicação são puladas, porque dependem de uma co-simulação. Comece por:

| Figura | O que mostra |
|---|---|
| `tensao_sem_armazenamento.png` | a tensão de cada fase ao longo do dia, sem armazenamento |
| `tensao_negociada.png` | a mesma tensão com a programação negociada, dentro de 0,97 a 1,03 pu |
| `preco_sombra.png` | o preço sombra por fase, mais negativo onde o operador precisa que o armazenamento absorva |
| `convergencia.png` | concentrador e operador chegando ao mesmo valor, rodada a rodada |

## 3. Os arquivos

`resultados/mercado/` tem `history.json`, `transactions.csv`,
`market_series.csv`, `dlmp.csv`, `flexibility.csv` e `figuras.npz`. O conteúdo
de cada um está em [Arquivos de saída](../reference/output-files.md).

Com a configuração padrão, os quatro CSV saem idênticos aos do
`./run.sh centralizado` do co-simulation-opentes.

## Próximos passos

- [A fase de operação](../tutorials/operation-phase.md)
- [Comparando formulações](../tutorials/compare-formulations.md)
- [O modelo de mercado](../explanation/market-model.md), para a formulação
