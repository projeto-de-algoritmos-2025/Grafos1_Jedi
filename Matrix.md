# Matrix

## Dados do Desafio:

- **Link do Desafio**: [Graph Theory: Matrix](https://www.hackerrank.com/challenges/matrix/problem)
- **Tema:** Graph Teory
- **Nível:** Difícil
- **Língaguem:** Python

## Passo a Passo da Resolução:

### 1. Compreensão do Prolema:

O problema é considera que se tem um conjunto de cidades conectadas por estradas **bidimensionais**. Assim, o objetivo é cortar a mínima quantidade de estradas para que os nós que contêm máquinas não estejam mais conectados entre si.

**Entradas:**

- A primeira linha contém dois inteiros n e m.

- n -> Número de cidades
- m -> Número de máquinas.

- Seguem (n-1) linhas, cada uma representando uma estrada conectando duas cidades.
- Após isso, são fornecidos m números os quais representam as cidades que possuem as máquinas.

#### Exemplo de entrada: 

- N = 5 → 
- M = 3 → 
- 2 1 8 → 
- 1 0 5 → 
- 2 4 5 
- 1 3 4 ->
- 2
- 4
- 0

