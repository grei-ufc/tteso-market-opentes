# Calibrar o preço

Por padrão, o preço sombra λ e o preço locacional (DLMP) saem como **sinal**, e
não em dinheiro. A liquidação avisa:

```text
[liquidacao] ATENCAO: com Ck = 1, lambda NAO esta em unidade monetaria. ...
```

e as colunas de preço levam o sufixo `_signal`.

## Por quê

No ótimo do concentrador, derivando `Ck (x − x_init)² + λ x` em x:

```text
2 Ck (x − x_init) + λ = 0   =>   λ = 2 Ck (x_init − x)
```

λ tem a unidade de `Ck` vezes potência. Com o `Ck = 1` adimensional da
formulação de referência, λ é numericamente um desvio de potência. A tese lê λ
como variável de controle, e não como valor financeiro.

## Como

Calibre o peso em moeda, em EUR/(kW²·h):

```bash
MARKET_CK_EUR=<valor> uv run tteso-mercado --settle-dir resultados/calibrado
```

ou, só para a liquidação, `--ck-eur <valor>` no `tteso-mercado`. Com a
calibração, λ sai em EUR/(kW·h) e o DLMP de cada barra e intervalo é

```text
DLMP(t, l) = preço_spot(t) + λ(t, l) / Δt × 1000      [EUR/MWh]
```

A escolha do valor de `Ck` não vem da formulação: é uma decisão de desenho do
mercado. A explicação completa está no início de `settlement.py` e em
[O modelo de mercado](../explanation/market-model.md).
