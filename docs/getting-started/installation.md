# Instalação

## Requisitos

- **Python 3.12**
- **[uv](https://github.com/astral-sh/uv)**, que cria o ambiente e instala as
  dependências
- **IBM CPLEX**, o solver padrão. A licença acadêmica é pessoal (gratuita pelo
  IBM Academic Initiative), então cada pessoa instala o seu

## Instalar

```bash
git clone https://github.com/grei-ufc/tteso-market-opentes.git
cd tteso-market-opentes
uv sync
```

O `uv sync` instala o pacote `market_opentes` com o NumPy, o pandas, o Pyomo e o
matplotlib, e cria três comandos: `tteso-mercado`, `tteso-operacao` e
`tteso-figuras` ([CLI](../reference/cli.md)).

## Apontar o solver

O modelo do prosumidor é inteiro-misto, e os do concentrador e do operador são
quadráticos. Aponte o executável do CPLEX:

```bash
export MARKET_SOLVER_PATH=$HOME/IBM/CPLEX_Studio2211/cplex/bin/x86-64_linux/cplex
```

Se o `cplex` já estiver no `PATH`, a variável é dispensável. Para outro solver,
veja [Configurar o solver](../how-to-guides/configure-solver.md).

## Conferir

```bash
uv run tteso-mercado --help
```

Deve listar as opções da negociação. Siga para a
[Primeira Negociação](first-negotiation.md).

## Grupos opcionais

| Grupo | Para quê | Instalar |
|---|---|---|
| `docs` | esta documentação | `uv sync --group docs` |
| `fundamentacao` | os exercícios de `FUNDAMENTACAO_TEORICA/` | `uv sync --group fundamentacao` |
