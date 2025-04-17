# Matrix

## Dados do Desafio:

- **Link do Desafio**: [Graph Theory: Matrix](https://www.hackerrank.com/challenges/matrix/problem)
- **Tema:** Graph Teory
- **Nível:** Difícil
- **Língaguem:** Python

## Passo a Passo da Resolução:

### 1. Compreensão do Prolema:

O problema considera que se tem um conjunto de cidades conectadas por estradas **bidimensionais**. Assim, o objetivo é cortar a mínima quantidade de estradas para que os nós que contêm máquinas não estejam mais conectados entre si.

**Entradas:**

- A primeira linha contém dois inteiros n e m.

- n -> Número de cidades
- m -> Número de máquinas.

- Seguem (n-1) linhas, cada uma representando uma estrada conectando duas cidades.
- Após isso, são fornecidos m números os quais representam as cidades que possuem as máquinas.

#### Exemplo de entrada: 

- N = 5 → 5 cidades no total.
- M = 3 → 3 das cidades têm máquinas.
- 2 1 8 → A primeira estrada conecta a cidade 2 à cidade 1, com o tempo de destruição sendo 8.
- 1 0 5 → A segunda estrada conecta a cidade 1 à cidade 0, com o tempo de destruição sendo 5.
- 2 4 5 → A terceira estrada conecta a cidade 2 à cidade 4, com o tempo de destruição sendo 5.
- 1 3 4 → A quarta estrada conecta a cidade 1 à cidade 3, com o tempo de destruição sendo 4.
- 2 → A cidade 2 tem máquinas.
- 4 → A cidade 4 tem máquinas.
- 0 → A cidade 0 tem máquinas.

### 2. Idea de Como Resolver

#### Como é um grafo:

No grafo, as cidades estão conectadas por estradas, que são as arrestas. Além disso, é uma árvore, pois há uma única estrada entre cada par de cidades.

- O objetivo é desconectar as máquinas, o que implica em cortar as estradas de forma eficiente.

### DSU

Para conseguir resolver a questão, teve-se que estudar um pouco sobre [DSU](https://cp-algorithms.com/data_structures/disjoint_set_union.html).

- **DSU (Disjoint Set Union / Union-Find)** é uma estrutura de dados que permite duas operações rápidas: descobrir a qual grupo um elemento pertence (find) e unir dois grupos (union).
- Os grupos no DSU representam conjuntos de cidades conectadas pelas estradas que não foram cortadas.
-  Para cada estrada entre dois nós (cidade) vai se usar o **find** para verificar se eles já pertencem ao mesmo componente (grafo conectado). Se não estão unidos, então tem que verificar se ambos os grafos contêm máquinas. Caso contenham, a estrada é considerada perigosa, pois permite que duas máquinas se conectem, então ela é "destruída".
- Se pelo menos um dos lados não tem máquina, os grafos são conectados(unidos) com union.


#### Pensamento

1. Se uma estrada conecta duas cidades que possuem máquinas, ela é considerada crítica e precisa ser cortada.
2. Ordenar as estradas em ordem crescente de tempo de destruição.
3. Cortas as estradas que possuem menor custo.


### Criação do psudo código
        
        Função minTime(estradas, maquinas):
        
            - O grafo é não direcionado: cada estrada liga duas cidades em ambas as direções.
        
            - Identificar o número total de cidades (máximo ID de cidade + 1)
        
            - Inicializar estrutura de Union-Find (DSU) com:
                - pai: vetor onde cada cidade é seu próprio líder (grafo)
                - componente_tem_maquina: indica se um grafo conectado contém uma máquina
        
            Para cada cidade com máquina:
                Marcar componente_tem_maquina[cidade] como verdadeiro
        
            Definir função encontrar_conjunto(i):
                - Esta função encontra o líder (ou raiz) do grafo conexa da cidade i
        
            Definir função unir_conjuntos(i, j):
                - Une os grafos conectados das cidades i e j
                - Só realiza a união se ainda não estiverem conectadas
                - Se ao unir, ambos os lados tinham máquinas, essa união não é permitida (não é feita)
        
            - Criar lista de estradas com formato (tempo, cidade1, cidade2)
            - Ordenar as estradas por tempo de destruição, do maior para o menor
        
            - Essa ordenação garante que estradas mais custosas (evitar destruir) sejam analisadas primeiro
        
            Inicializar tempo_total_minimo = 0
        
            Para cada estrada (tempo, u, v) na lista ordenada:
                raiz_u = encontrar_grafo(u)
                raiz_v = encontrar_grafo(v)
        
                - Se as cidades u e v estão em grafos diferentes:
                    - Se ambas as componentes já têm máquinas:
                        - A estrada precisa ser destruída, então adiciona o tempo ao total
                    - Senão:
                        - Pode manter essa estrada no grafo (manter conectividade)
                        - Unir os dois conjuntos no Union-Find (conectar as cidades no grafo)
        
            Retornar tempo_total_minimo



### 4. Criação do Código em Python

```python
import math
import os
import random
import re
import sys

# Tenta aumentar o limite de recursao para permitir chamadas de funcao mais profundas
try:
    sys.setrecursionlimit(20000)
except Exception as e:
    pass

# Funcao para calcular o tempo minimo para conectar as maquinas usando as estradas
def minTime(estradas, maquinas):
    maior_id_no = -1

    # Verifica se ha estradas e maquinas
    if not estradas and not maquinas:
        numero_de_nos = 0  # Nenhuma estrada nem maquina
    else:
        # Encontra o maior id de no nas estradas para identificar a quatidade.
        for cidade1, cidade2, _ in estradas:
            maior_id_no = max(maior_id_no, cidade1, cidade2)

        # Considera tambem os nos com maquinas
        for no_com_maquina in maquinas:
            maior_id_no = max(maior_id_no, no_com_maquina)

        numero_de_nos = maior_id_no + 1  # Numero total de nos no grafo

    # Casos triviais: nenhum no ou apenas um no
    if numero_de_nos == 0 or numero_de_nos == 1:
        return 0

    # Inicializa o vetor de pais
    pai = list(range(numero_de_nos))

    # Vetor que indica se um no tem maquina
    maquina_no_componente = [False] * numero_de_nos

    # Marca os nos que contem maquinas
    for no_com_maquina in maquinas:
        if 0 <= no_com_maquina < numero_de_nos:
            maquina_no_componente[no_com_maquina] = True

    # Funcao para encontrar o representante do conjunto com compressao de caminho, baseado na recurssao do DFS
    def encontrar_pai(no):
        if pai[no] == no:
            return no
        pai[no] = encontrar_pai(pai[no])
        return pai[no]

    # Funcao para unir dois conjuntos disjuntos
    def unir_conjuntos(no1, no2):
        raiz1 = encontrar_pai(no1)
        raiz2 = encontrar_pai(no2)
        # Se os conjuntos sao diferentes, unimos
        if raiz1 != raiz2:
            pai[raiz2] = raiz1
            maquina_no_componente[raiz1] = maquina_no_componente[raiz1] or maquina_no_componente[raiz2]
            return True
        return False

    # Processa as estradas validas
    estradas_processadas = []
    for cidade1, cidade2, tempo in estradas:
        if 0 <= cidade1 < numero_de_nos and 0 <= cidade2 < numero_de_nos:
            estradas_processadas.append((tempo, cidade1, cidade2))

    # Ordena as estradas por tempo em ordem decrescente
    estradas_processadas.sort(key=lambda x: x[0], reverse=True)

    # Variavel que acumula o tempo total minimo
    tempo_total_minimo = 0

    # Processa as estradas para evitar que maquinas se conectem
    for tempo, cidade1, cidade2 in estradas_processadas:
        raiz1 = encontrar_pai(cidade1)
        raiz2 = encontrar_pai(cidade2)

        # Se ainda nao estao no mesmo conjunto
        if raiz1 != raiz2:
            # Se ambos os conjuntos contem maquinas, destruir essa estrada
            if maquina_no_componente[raiz1] and maquina_no_componente[raiz2]:
                tempo_total_minimo += tempo
            else:
                # Caso contrario, unimos os conjuntos
                unir_conjuntos(cidade1, cidade2)

    # Retorna o tempo total minimo para evitar que maquinas se conectem
    return tempo_total_minimo

if __name__ == '__main__':
    output_path = os.environ.get('OUTPUT_PATH')
    if output_path:
        fptr = open(output_path, 'w')
    else:
        fptr = sys.stdout

    try:
        first_multiple_input = input().rstrip().split()
        n_nodes = int(first_multiple_input[0])
        k_machines = int(first_multiple_input[1])

        input_roads = []

        num_edges_to_read = n_nodes - 1
        for _ in range(num_edges_to_read):
            road_data = list(map(int, input().rstrip().split()))
            if len(road_data) == 3:
                input_roads.append(road_data)

        input_machines = []
        for _ in range(k_machines):
            machines_item = int(input().strip())
            input_machines.append(machines_item)

        result = minTime(input_roads, input_machines)

        fptr.write(str(result) + '\n')

        fptr.close()

    except Exception as e:
        pass

```

## Resolução
