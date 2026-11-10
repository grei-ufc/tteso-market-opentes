# Changelog

## Documentação no padrão do OpenTES

- Site de documentação no modelo do `tsre-der-opentes`, publicado no GitHub
  Pages pelo workflow de CI, com a API gerada das docstrings.
- `tteso-mercado` e `tteso-operacao` passam a usar teto de 300 rodadas por
  padrão. Antes, o teto de 30 do módulo parava a negociação antes de convergir,
  e o `tteso-operacao` rodava a operação sobre uma programação incompleta.

## 11/2026: o mercado transativo

- O repositório passa a se chamar `tteso-market-opentes` e recebe o mercado
  extraído do `co-simulation-opentes`: o pacote `market_opentes`, com o caso
  IEEE 13 embutido, os comandos `tteso-mercado`, `tteso-operacao` e
  `tteso-figuras`, e a página da formulação.
- As bibliotecas da fundamentação teórica passam a um grupo opcional
  (`uv sync --group fundamentacao`).

## 12/2025: fundamentação teórica

- Exercícios de meta-heurísticas (PSO, EPSO, NSGA-II), programação linear em
  Pyomo e CPLEX, e demonstrações do mosaik 3.5.

## 11/2025: ambiente do time

- README de instalação com o uv e `pyproject.toml` do TTESO.

## 09/2025: criação

- Repositório criado como `tteso-tes-opentes`, para as soluções de sistemas de
  energia transativos do OpenTES.
