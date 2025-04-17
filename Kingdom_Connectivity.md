# Desafio: Kingdom Connectivity


##  Dados do Desafio:

- **Link do Desafio**: [Graph Theory: Kingdom Connectivity](https://www.hackerrank.com/challenges/kingdom-connectivity/problem)
- **Tema:** Graph Teory
- **Nível:** Difíil
- **Língaguem:** Python


## Enunciado


## Passo a Passo da Resolução:


### 1. Compreensão do Prolema:

**Descrição:** No grafo, o objetivo é encontrar quantos caminhos diferentes existem do nó 1 até o nó n (de 1 a n), ou determinar se há infinitos caminhos.

**Observação:** O resultado numérico é em modulo 10⁹

**Entradas:**

A primeira linha contém dois números inteiros:

  - n → O número de cidades do reino
 - m → O número de estradas

Tem ainda m linhas com dois inteiros que mostra as arestas que ligam as cidades.

#### Exemplo de entrada: 

- n = 5 → Tem cidades

- m = 5 → Tem 5 caminhos (arestas)

- Aresta 1: [1,2] → Aresta liga as cidades 1 e 2;

- Aresta 2: [2,4] → Aresta liga as cidades 2 e 4;

- Aresta 3: [2,3] → Aresta liga as cidades 2 e 3;

- Aresta 2: [3,4] → Aresta liga as cidades 3 e 4;

- Aresta 3: [4,5] → Aresta liga as cidades 4 e 5; 

**Importante:** O caminho é unidirecional → Grafo direcionado. 

### 2. Idea de Como Resolver

#### Como é um grafo:

#### Pensamento

1. Criar uma lista de adjacência (adj) a partir das estradas fornecidas.
2. Identificar se existe um ciclo em algum caminho de 1 até n usando DFS (Busca em profundidade.)
3. Se não houver ciclo, contar todos os caminhos distintos de 1 até n também usando DFS.

### Criação de um psudo código com base no slide 43 adaptado:

### Verificar Ciclo com DFS

        DFS-CICLO(G)
        1 Para todo v de 1 até n
        2     status[v] ← não visitado   // 0 = não visitado, 1 = visitando, 2 = finalizado
        3     tem_ciclo ← falso (Variável booleana)
        4 DFS-CICLO-VISITAR(G, 1)
        
        DFS-CICLO-VISITAR(G, v)
        1 status[v] ← visitando
        2 Para todo w em Adj(v)
        3     Se status[w] = não visitado então
        4         DFS-CICLO-VISITAR(G, w)
        5     Senão se status[w] = visitando então
        6         tem_ciclo ← verdadeiro
        7 status[v] ← finalizado

### Contar Caminhos com DFS

        DFS-CONTAR-CAMINHOS(G)
        1 contador ← 0
        2 DFS-CAMINHOS-VISITAR(G, 1)
        3 Retorne contador
        
        DFS-CAMINHOS-VISITAR(G, v)
        1 Se v = m então
        2     contador ← contador + 1
        3     Retorne
        4 Para todo w em Adj(v)
        5     DFS-CAMINHOS-VISITAR(G, w)


### 4. Criação do Código em Python
```python
import sys
import math
import os
import random
import re

# Tenta definir o limite de recursao para 20000
try:
    sys.setrecursionlimit(20000) 
except Exception as e:
    pass 

MOD = 1000000000  # Constante MOD, usada para limitar o resultado final

def countPaths(n, edges):
    # Se nao ha nos no grafo, nao ha caminhos
    if n == 0:
        print(0)
        return
    
    # Caso o grafo tenha apenas 1 no
    if n == 1:
        # Se houver uma aresta (1,1), isso pode causar um ciclo infinito
        for u_in, v_in in edges:
            if u_in == 1 and v_in == 1:
                print("INFINITE PATHS")
                return
        print(1)  # Se nao houver ciclo infinito, o numero de caminhos e 1
        return

    # Inicializa listas de adjacencia para o grafo e seu inverso
    e = [[] for _ in range(n)]  # Grafo original
    ee = [[] for _ in range(n)]  # Grafo invertido

    # Inicializa arrays para marcar nos visitados
    vis0 = [False] * n  # Marca nos visitados a partir do no 1
    vis1 = [False] * n  # Marca nos visitados a partir do no n
    vis = [False] * n  # Marca todos os nos visitados durante a busca
    memo = [-1] * n  # Memorizacao para evitar recalculos

    # Funcao DFS para explorar os nos do grafo
    def dfs(graph, u, visited_array):
        visited_array[u] = True
        for v in graph[u]:
            if not visited_array[v]:
                dfs(graph, v, visited_array)

    # Funcao recursiva para contar os caminhos
    def _count_paths_recursive(u):
        if memo[u] != -1:
            return memo[u]  # Retorna o resultado memorizado se ja foi calculado
        
        vis[u] = True  # Marca o no como visitado durante a busca
        
        ans = 0  # Contagem de caminhos
        if u == n - 1:  # Se o no for o destino final (n-1)
            ans = 1  # Um caminho encontrado
        else:
            # Para cada no adjacente, tenta contar os caminhos
            for v in e[u]:
                # Verifica se o no v e alcancavel tanto do inicio quanto do fim
                if vis0[v] and vis1[v]: 
                    # Se o no v ja foi visitado, ha um ciclo infinito
                    if vis[v]: 
                        raise ValueError("INFINITE PATHS") 
                    
                    # Chama a funcao recursiva para contar os caminhos a partir de v
                    path_count_from_v = _count_paths_recursive(v)
                    # Atualiza a contagem total de caminhos
                    ans = (ans + path_count_from_v) % MOD

        vis[u] = False  # Desmarca o no como visitado para outras buscas
        memo[u] = ans  # Memorizacao do resultado
        return ans

    # Preenche as listas de adjacencia com os dados das arestas
    for edge in edges:
        u_in, v_in = edge[0], edge[1]
        u_idx, v_idx = u_in - 1, v_in - 1  # Converte para indice 0-base
        if 0 <= u_idx < n and 0 <= v_idx < n:
             e[u_idx].append(v_idx)  # Adiciona aresta ao grafo
             ee[v_idx].append(u_idx)  # Adiciona aresta ao grafo invertido

    # Executa DFS a partir do no 1 para marcar os nos alcancaveis
    dfs(e, 0, vis0)          
    # Executa DFS a partir do no n para marcar os nos alcancaveis no grafo invertido
    if n - 1 >= 0: 
        dfs(ee, n - 1, vis1) 

    # Verifica se o no final e alcancavel a partir do no 1
    if not vis0[n - 1]:
         print(0)  # Se nao for alcancavel, nao ha caminho
    else:
        try:
            # Conta os caminhos a partir do no 1 para o no n
            result = _count_paths_recursive(0) 
            print(result)
        except ValueError as e_msg:
            # Caso ocorra um ciclo infinito, imprime a mensagem apropriada
            if str(e_msg) == "INFINITE PATHS":
                print("INFINITE PATHS")
            else:
                raise  # Levanta a excecao caso seja outro erro

# Leitura da entrada
if __name__ == '__main__':
    first_multiple_input = input().rstrip().split()

    nodes = int(first_multiple_input[0])

    m = int(first_multiple_input[1])

    edges_input = []

    # Leitura das arestas
    for _ in range(m):
        edges_input.append(list(map(int, input().rstrip().split())))

    # Chama a funcao para contar os caminhos
    countPaths(nodes, edges_input)
```

## Resolução
