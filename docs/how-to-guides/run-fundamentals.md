# Rodar os exercícios de fundamentação

A pasta `FUNDAMENTACAO_TEORICA/` guarda os exercícios de estudo do time, feitos
antes do mercado: meta-heurísticas, programação linear e co-simulação com o
mosaik. Eles não fazem parte do pacote e têm dependências próprias.

## Instalar

```bash
uv sync --group fundamentacao
```

O grupo traz NumPy, pandas, matplotlib, Pyomo, PySwarms, pymoo e as demais
bibliotecas dos exercícios.

## O que há

| Pasta | Conteúdo |
|---|---|
| `Exercicios_Programacao_Linear/` | exercícios de programação linear em Pyomo |
| `META-Heuristicas/PSO/` | PSO na função de Rastrigin, com NumPy, pandas e PySwarms, e a comparação com o NSGA-II |
| `META-Heuristicas/EPSO/` | EPSO, com notas sobre o código de referência do time |
| `META-Heuristicas/NSGA-ii/` | NSGA-II com o pymoo |
| `Otimizadores/CPLEX/` | CPLEX pelo docplex, sozinho e combinado com EPSO, PSO e algoritmo genético |
| `Otimizadores/MOSAIK3_5/` | demonstrações do mosaik 3.5, com `pyproject.toml` próprio |

## Rodar

```bash
uv run --group fundamentacao python "FUNDAMENTACAO_TEORICA/Exercicios_Programacao_Linear/PYOMO - PROGRAMAÇÃO LINEAR/Exercícios Pyomo 1.py"
```

Os exercícios com CPLEX usam o `docplex`, que não está no grupo e pede o CPLEX
instalado. Os do mosaik rodam no ambiente da própria pasta:

```bash
cd FUNDAMENTACAO_TEORICA/Otimizadores/MOSAIK3_5
uv sync
uv run python demo1.py
```
