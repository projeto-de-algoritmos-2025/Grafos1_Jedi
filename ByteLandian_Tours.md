# Desafio: ByteLandian Tours

## Dados do Desafio:

- **Link do Desafio**: [ByteLandian Tours](https://www.hackerrank.com/challenges/bytelandian-tours/problem)
- **Tema**: Teoria dos Grafos
- **Nível**: Difícil
- **Linguagem**: Python

<img src="" alt="Descrição" width="400"/>


## Enunciado

<img src="" alt="Descrição" width="1000"/>

## Passo a Passo da Resolução:

### 1. Compreensão do Problema

**Descrição**: O problema exige que seja calculado o número de rotas possíveis entre as cidades de Byteland ao levar em consideração as estradas antigas e as novas estradas que serão adicionadas.

O desafio principal é contar o número de rotas que podem ser formadas entre as cidades, com base nas conexões fornecidas. O número de rotas possíveis será usada para um vendedor de porta em porta (salesman)

**Entradas**:
-  T → Número de casos de teste, que deve ser entre 1 e 2 → 1 <= T <= 20
- N → O número de cidades em Byteland. → 1 <= N <= 10000
- As conexões entre as cidades, fornecidas como um grafo, onde as cidades são conectadas por estradas bidimensionais.
   - A i-ésima linha contém dois inteiros ai e bi, que significa que originalmente havia uma estrada conectando cidades com números ai e bi.

#### Exemplo de entrada: 

2        ← número de casos de teste (T)
3        ← número de cidades no primeiro caso (N = 3)
0 1      ← estrada entre as cidades 0 e 1
1 2      ← estrada entre as cidades 1 e 2
5        ← número de cidades no segundo caso (N = 5)
0 1      ← estrada entre as cidades 0 e 1
1 2      ← estrada entre as cidades 1 e 2
2 3      ← estrada entre as cidades 2 e 3
2 4      ← estrada entre as cidades 2 e 4

**Importante**: As estradas são bidimensionais, ou seja, é um grafo não direcionado.

### 2. Idea de Como Resolver

#### Como é um grafo:

#### Pensamento

A abordagem para resolver esse problema envolve a análise da estrutura do grafo e a contagem das rotas possíveis.

1. Modelar o grafo como uma árvore e representar as cidades com uma lista de adjacência a partir das arestas fornecidas.
2. Identificar os pares de cidades a duas arestas de distância, percorrendo cada vértice com BFS limitado a profundidade 2.
3. Adicionar arestas entre essas cidades “somewhat near”, formando um grafo aumentado com conexões extras.
4. Criar uma estrutura onde cada estado representa um subconjunto de cidades visitadas e a cidade atual.
5. Inicializar com a cidade 0 marcada como visitada, o que permite as transições para os seus vizinhos no grafo aumentado.
6. Somar os caminhos que terminam com todas as cidades visitadas e voltam à cidade 0.

### Criação de um psudo código 
        
        Função contar_ciclos_(tree_adj):
            n := número de cidades (tamanho de tree_adj)
        
            Criar um grafo aumentado com n listas vazias → graph[n]
        
            // Passo 2: Encontrar pares de cidades "somewhat near"
            Para cada cidade u de 0 até n - 1:
                visited := vetor de tamanho n com todos os valores -1
                fila := fila contendo (u, 0) // cidade atual e distância
                visited[u] := 0
        
                Enquanto fila não estiver vazia:
                    (cidade_atual, dist) := fila.remove()
                    Se dist == 2:
                        // Cidades a duas arestas de distância
                        Se u não está em graph[cidade_atual]:
                            Adicione cidade_atual em graph[u]
                            Adicione u em graph[cidade_atual]
                    Se dist < 2:
                        Para cada vizinho em tree_adj[cidade_atual]:
                            Se visited[vizinho] == -1:
                                visited[vizinho] := dist + 1
                                fila.adiciona((vizinho, dist + 1))
        
            Criar dp[2^n] como lista de dicionários → dp[mask][u] = nº de formas
            Inicializar dp[1 << 0][0] := 1 // Começamos na cidade 0 visitada
        
            Para cada subconjunto de cidades (mask) de 0 até 2^n - 1:
                Para cada cidade atual u de 0 até n - 1:
                    Se dp[mask][u] > 0:
                        Para cada vizinho v de u em graph[u]:
                            Se cidade v ainda não foi visitada no mask:
                                novo_mask := mask | (1 << v)
                                dp[novo_mask][v] += dp[mask][u]
        
            // Passo 7: Contar todos os caminhos que terminam na cidade 0
            total := 0
            full_mask := (1 << n) - 1 // Todas as cidades visitadas
        
            Para cada cidade u de 0 até n - 1:
                Se existe aresta entre u e 0 no graph:
                    total += dp[full_mask][u]
        
            Retorne total


### 4. Criação do Código em Python
```python
#!/bin/python3

import math
import os
import random
import re
import sys
from collections import deque

MOD = 1000000007

def precalcular_fatoriais(n, modulo):
    tam_fatoriais = max(n + 1, 1)  # Define o tamanho necessario para o array de fatoriais
    fatoriais_array = [1] * tam_fatoriais  # Inicializa o array de fatoriais
    for i in range(2, tam_fatoriais):
        fatoriais_array[i] = (fatoriais_array[i-1] * i) % modulo  
    return fatoriais_array  # Retorna o array com os fatoriais

# Funcao que executa uma busca em largura (BFS) simples a partir do no 0
def executar_bfs_teste(n, adjacencias):
    if n == 0:
        return  # Se nao houver cidades, nada e feito
    if 0 < n:
        fila = deque([0])  
        visitados = {0}  
        while fila:
            u = fila.popleft()  
            if 0 <= u < n:  
                if u < len(adjacencias):  
                    for v in adjacencias[u]:  
                        if 0 <= v < n and v not in visitados:  # Se o vizinho nao foi visitado
                            visitados.add(v)  # Marca o vizinho como visitado
                            fila.append(v)  # Adiciona o vizinho a fila para explorar
    pass  


def analisar_no(indice_no, n, adj_original, graus_original, fatoriais_precalculados):
    if not (0 <= indice_no < n):
        return (False, False, 1)  # Se o indice for invalido, retorna falso
    grau = graus_original[indice_no]  # Obtem o grau (numero de vizinhos) do no

    if grau <= 1:
        return (False, True, 1)  

  
    else:
        eh_interno = True
        vizinhos_folha = 0
        vizinhos_internos = 0
        if indice_no < len(adj_original):
            for vizinho in adj_original[indice_no]: 
                if 0 <= vizinho < n:
                    if graus_original[vizinho] == 1: 
                        vizinhos_folha += 1
                    else:  
                        vizinhos_internos += 1
                else:
                     return (eh_interno, False, 1)  # Se o vizinho nao for valido
        else:
             return (False, False, 1)  # Se o indice for invalido

        if vizinhos_internos > 2:
            estrutura_possivel = False  # Se houver mais de dois vizinhos internos, a estrutura e impossivel
            termo_fatorial = 1
        else:
            estrutura_possivel = True  
            if vizinhos_folha >= len(fatoriais_precalculados):  
                estrutura_possivel = False
                termo_fatorial = 1
            else:
                 termo_fatorial = fatoriais_precalculados[vizinhos_folha]

        return (eh_interno, estrutura_possivel, termo_fatorial) 

# Funcao calcula o numero de rotas possiveis na arvore
def calcular_rotas_hamiltonianas(n, adj_original, graus_original, fatoriais_precalculados):
    produto_total = 1
    contagem_nos_internos = 0

    for i in range(n):
        eh_interno, no_possivel, termo = analisar_no(i, n, adj_original, graus_original, fatoriais_precalculados)

        if not no_possivel: 
            return 0

        if eh_interno:
            contagem_nos_internos += 1
            produto_total = (produto_total * termo) % MOD  # Multiplica o resultado pelo fatorial do numero de folhas

    if contagem_nos_internos == 0:
        return 0 if n > 2 else (2 % MOD)  
    elif contagem_nos_internos == 1:
        return produto_total 
    else:
        return (produto_total * 2) % MOD  # duas direcoes para o ciclo

def rotasBytelandia(n, estradas):
    if n == 0:
        return 0  # Se nao houver cidades, nao ha rotas possiveis
    if n == 1:
        return 1  # Se houver apenas uma cidade, ha uma unica rota (a cidade em si)
    if n == 2:
        return 2 % MOD  # Se houver duas cidades, ha duas formas de percorrer (ida e volta)

    adj_original = [[] for _ in range(n)]  # Inicializa o grafo com listas vazias
    graus_original = [0] * n 

    # Criacao das adjacencias com as estradas fornecidas
    for estrada in estradas:
        if len(estrada) == 2:
            u, v = estrada
            if 0 <= u < n and 0 <= v < n:
                adj_original[u].append(v)
                adj_original[v].append(u)
                graus_original[u] += 1
                graus_original[v] += 1

    executar_bfs_teste(n, adj_original)

    fatoriais_precalculados = precalcular_fatoriais(n, MOD)

    resultado = calcular_rotas(n, adj_original, graus_original, fatoriais_precalculados)

    return resultado


if __name__ == '__main__':
    try:
        output_path = os.environ['OUTPUT_PATH']
        fptr = open(output_path, 'w')
    except KeyError:
        fptr = sys.stdout

    t = int(input().strip()) 

    for t_itr in range(t):
        n = int(input().strip()) 

        roads = []

        num_expected_roads = n - 1 if n > 1 else 0
        for _ in range(num_expected_roads):
             try:
                 road_input = list(map(int, input().rstrip().split()))  # Le as estradas
                 if len(road_input) == 2:
                     roads.append(road_input)
                 else:
                     pass
             except ValueError:
                 pass
             except EOFError:
                 break

        result = rotasBytelandia(n, roads)

        fptr.write(str(result) + '\n')

    if fptr != sys.stdout:
        fptr.close()
```

## Resultado Final

<img src="" alt="Descrição" width="800"/>
