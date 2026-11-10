# Ambiente de desenvolvimento

## Ambiente

```bash
git clone https://github.com/grei-ufc/tteso-market-opentes.git
cd tteso-market-opentes
uv sync --group docs
export MARKET_SOLVER_PATH=$HOME/IBM/CPLEX_Studio2211/cplex/bin/x86-64_linux/cplex
```

O `uv sync` instala o pacote em modo editável: uma mudança em `market_opentes/`
vale no próximo comando.

## Conferir uma mudança

O repositório não tem suíte de testes automatizados. A conferência é rodar a
negociação e comparar com a referência:

```bash
uv run tteso-mercado --settle-dir /tmp/antes        # antes da mudança
uv run tteso-mercado --settle-dir /tmp/depois       # depois
for f in transactions market_series dlmp flexibility; do
  cmp /tmp/antes/$f.csv /tmp/depois/$f.csv && echo "$f igual"
done
```

Se a mudança não deveria alterar o resultado, os quatro arquivos ficam iguais,
e a negociação continua em 42 rodadas.

## Manter em sincronia com a plataforma

O pacote também vive em `simulators/market-opentes` do co-simulation-opentes.
Uma correção na formulação deve ir para os dois repositórios, e o caso de
estudo, quando mudar lá, precisa ser copiado para cá
([Atualizar os dados do caso](../how-to-guides/update-case-data.md)).

## Comentários

O código e os comentários estão em português. Os comentários explicam o porquê
de cada escolha, e os modelos citam as equações da tese pelo número, como
(6.25). As docstrings alimentam a [API gerada](../reference/api/index.md).
