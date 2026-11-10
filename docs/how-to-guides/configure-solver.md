# Configurar o solver

Os modelos são escritos em Pyomo e resolvidos pelo solver que
`MARKET_SOLVER` e `MARKET_SOLVER_PATH` indicarem. O padrão é o IBM CPLEX.

## O que o solver precisa resolver

| Modelo | Tipo |
|---|---|
| prosumidor, (6.1) a (6.9) | inteiro-misto: variáveis binárias impedem carga e descarga no mesmo intervalo |
| concentrador, (6.25) e (6.26) | quadrático |
| operador, (6.27) a (6.29) | quadrático |

Um solver alternativo precisa resolver os três tipos.

## CPLEX

```bash
export MARKET_SOLVER_PATH=$HOME/IBM/CPLEX_Studio2211/cplex/bin/x86-64_linux/cplex
```

A licença acadêmica é pessoal, pelo IBM Academic Initiative, e por isso o CPLEX
não vem no repositório. Com o CPLEX, o pacote liga o *crossover* do método de
barreira: sem ele, na primeira rodada, com o preço sombra zerado, o CPLEX pode
terminar como `unknown` mesmo tendo chegado ao ótimo. O motivo está comentado em
`optimization.py`.

## Outro solver

```bash
export MARKET_SOLVER=<nome no Pyomo>
export MARKET_SOLVER_PATH=<executável>      # se não estiver no PATH
```

`MARKET_SOLVER` é o nome que o `SolverFactory` do Pyomo reconhece. Os resultados
de referência desta documentação foram obtidos com o CPLEX 22.1.1; outro solver
pode convergir em outro número de rodadas.

## Quando o solver falha

Um modelo que não termina como ótimo levanta
`RuntimeError: <modelo>: solver terminou em <condição>`, com o nome do modelo
que falhou. As causas mais comuns são o executável não encontrado (confira o
`MARKET_SOLVER_PATH`) e a licença de avaliação do CPLEX, limitada a problemas
pequenos.
