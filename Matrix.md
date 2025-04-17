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

#### Pensamento

1. Se uma estrada conecta duas cidades que possuem máquinas, ela é considerada crítica e precisa ser cortada.
2. Ordenar as estradas em ordem crescente de tempo de destruição.
3. Cortas as estradas que possuem menor custo.


### Criação do psudo código


        def minTime(roads, machines):
            # Inicializar a variável para o número de cidades
            n = max_city_id(roads, machines)
        
            # Criar o array de pais e inicializar a presença de máquinas nas cidades
            parent = list(range(n))
            machine_in_component = [False] * n
            for machine in machines:
                machine_in_component[machine] = True
        
            # Função para encontrar o líder de um conjunto (Union-Find)
            def find_set(i):
                if parent[i] == i:
                    return i
                parent[i] = find_set(parent[i])
                return parent[i]
        
            # Função para unir dois conjuntos
            def unite_sets(i, j):
                root_i = find_set(i)
                root_j = find_set(j)
                if root_i != root_j:
                    parent[root_j] = root_i
                    machine_in_component[root_i] = machine_in_component[root_i] or machine_in_component[root_j]
                    return True
                return False
        
            # Processar as estradas e ordenar
            processed_edges = []
            for u, v, time in roads:
                processed_edges.append((time, u, v))
            processed_edges.sort(key=lambda x: x[0], reverse=True)
        
            # Inicializar o tempo total de destruição
            min_total_time = 0
        
            # Processar as estradas e calcular o tempo total
            for time, u, v in processed_edges:
                root_u = find_set(u)
                root_v = find_set(v)
        
                if root_u != root_v:
                    if machine_in_component[root_u] and machine_in_component[root_v]:
                        min_total_time += time
                    else:
                        unite_sets(u, v)
        
            return min_total_time


### 4. Criação do Código em Python

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
          def minTime(roads, machines):
              max_node_id = -1
          
              # Verifica se ha estradas ou maquinas
              if not roads and not machines:
                  n = 0  # Nenhuma estrada nem maquina
              else:
                  # Encontra o maior id de no nas estradas
                  for u, v, _ in roads:
                      max_node_id = max(max_node_id, u, v)
                  # Encontra o maior id de no nas maquinas
                  for machine_node in machines:
                      max_node_id = max(max_node_id, machine_node)
                  # O numero de nos sera o maior id encontrado mais 1
                  n = max_node_id + 1
          
              # Se nao ha nos (nenhuma estrada ou maquina), retorna 0
              if n == 0:
                  return 0
              # Se ha apenas um no, tambem nao ha tempo
              if n == 1:
                  return 0
          
              # Inicializa o vetor de pais (para o algoritmo de uniao de conjuntos disjuntos)
              parent = list(range(n))
              # Vetor que indica se um no tem maquina ou nao
              machine_in_component = [False] * n
              # Marca os nos que tem maquinas
              for machine_node in machines:
                  if 0 <= machine_node < n:
                      machine_in_component[machine_node] = True
          
              # Funcao para encontrar o pai de um conjunto, com compressao de caminho
              def find_set(i):
                  if parent[i] == i:
                      return i
                  parent[i] = find_set(parent[i])
                  return parent[i]
          
              # Funcao para unir dois conjuntos, atualizando o vetor de pais
              def unite_sets(i, j):
                  root_i = find_set(i)
                  root_j = find_set(j)
                  # Se os conjuntos forem diferentes, unimos eles
                  if root_i != root_j:
                      parent[root_j] = root_i
                      machine_in_component[root_i] = machine_in_component[root_i] or machine_in_component[root_j]
                      return True
                  return False
          
              # Processa as estradas, armazenando as estradas validas
              processed_edges = []
              for u, v, time in roads:
                  if 0 <= u < n and 0 <= v < n:
                      processed_edges.append((time, u, v))
          
              # Ordena as estradas pelo tempo, em ordem decrescente
              processed_edges.sort(key=lambda x: x[0], reverse=True)
          
              # Variavel para acumular o tempo total minimo
              min_total_time = 0
          
              # Processa as estradas para conectar os componentes
              for time, u, v in processed_edges:
                  root_u = find_set(u)
                  root_v = find_set(v)
          
                  # Se os dois nos nao pertencem ao mesmo conjunto, tentamos unir eles
                  if root_u != root_v:
                      # Se ambos os conjuntos possuem maquinas, adicionamos o tempo
                      if machine_in_component[root_u] and machine_in_component[root_v]:
                          min_total_time += time
                      else:
                          # Caso contrario, unimos os conjuntos
                          unite_sets(u, v)
          
              # Retorna o tempo minimo total para conectar as maquinas
              return min_total_time
          
          # Funcao principal que recebe a entrada e chama a funcao minTime
          if __name__ == '__main__':
              output_path = os.environ.get('OUTPUT_PATH')  # Verifica se ha caminho para a saida
              if output_path:
                  fptr = open(output_path, 'w')  # Abre o arquivo de saida
              else:
                  fptr = sys.stdout  # Caso nao haja caminho, usa a saida padrao
          
              try:
                  # Leitura da entrada
                  first_multiple_input = input().rstrip().split()
                  n_nodes = int(first_multiple_input[0])  # Numero de nos
                  k_machines = int(first_multiple_input[1])  # Numero de maquinas
          
                  input_roads = []
                  num_edges_to_read = n_nodes - 1  # O numero de arestas eh n-1 (em uma arvore)
                  for _ in range(num_edges_to_read):
                      road_data = list(map(int, input().rstrip().split()))
                      if len(road_data) == 3:
                          input_roads.append(road_data)
          
                  input_machines = []
                  for _ in range(k_machines):
                      machines_item = int(input().strip())
                      input_machines.append(machines_item)
          
                  # Chama a funcao para calcular o tempo minimo
                  result = minTime(input_roads, input_machines)
          
                  # Escreve o resultado na saida
                  fptr.write(str(result) + '\n')
          
              except Exception as e:
                  pass  # Em caso de erro, nao faz nada
              finally:
                  if output_path and fptr != sys.stdout:
                      fptr.close()  # Fecha o arquivo de saida, se estiver sendo usado
