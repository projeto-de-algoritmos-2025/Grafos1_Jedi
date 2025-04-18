# Desafio: Journey to the Moon

## Dados do Desafio:

- **Link do Desafio**: [Graph Theory: Journey to the Moon](https://www.hackerrank.com/challenges/journey-to-the-moon/problem)
- **Tema:** Graph Teory
- **Nível:** Médio
- **Língaguem:** Python

**Figura 1:** Dados do desafio.

<img src="https://raw.githubusercontent.com/projeto-de-algoritmos-2025/Grafos1_Jedi/refs/heads/main/Imagens/PA1_DadosJtM.png" alt="Descrição" width="400"/>


## Enunciado

**Figura 2**: Enuciado do desafio.

<img src="https://raw.githubusercontent.com/projeto-de-algoritmos-2025/Grafos1_Jedi/refs/heads/main/Imagens/PA1_EnunciadoJtM.png" alt="Descrição" width="1000"/>

## Passo a Passo da Resolução:


### 1. Compreensão do Prolema:

**Descrição:** O problema dá astronautas e pares de astronautas do mesmo país. O objetivo é calcular quantos pares de astronautas de países diferentes pode-se escolher.

**Entradas:** 
A primeira linha contém dois números inteiros:

  - n → O número de astronautas que  estão na seleção
  - p → O número de paras ou conjuntos de astronautas do mesmo país.

#### Exemplo de entrada: 

- n = 5 → Tem 5 Astronautas.

- P = 3 → Três países diferentes, cada um com um par./

- País 1: [0,1] → O país 1 tem os astronautas 0 e 1;

- País 2: [2,3] → O país 2 tem os astronautas  2 e 3;

- País 3; [0,4] → O país 3 tem os astronautas 0 e 4; 

**Importante:** Uma observação aqui, como o país 1 e o 3 têm astronautas em comum, ou seja, o 0, então quer dizer que, na verdade, eles são o mesmo país.

### 2. Idea de Como Resolver

#### Como é um grafo:

Cada astronauta é um vétice e cada par é na verdade um aresta que liga os nós. Por exemplo, 0–1 e 0-4.

#### Pensamento

1. Encontrar quais astronautas estão no mesmo país, ou seja, achar os vértices conectados.
2. Contar os pares de astronautas que não pertencem ao mesmo país. (árvores diferentes,)
3. Calcular o total de pares de acordo com a fórmula de combinação: n * (n -1) / 2
4. Subtrair os pares dentro do mesmo país para cada árvore. → pares_do_grupo = g * (g - 1) / 2
5. Total: combinação – pares por cada grupo. Exemplo: pares_do_grupo = g * (g - 1) / 2

**Para encontrar as diferentes árvores**: Usar Busca em Largura (BFS) -> Vai retornar uma floresta

- Os astronautas vão ser uma lista de adjacência.


### 3. Criação de um psudo código com base no slide 23 adaptado:
        
        1. Criar uma lista de adjacência Adj[n]         // n é o número total de astronautas
        2. Para cada par (a, b) na entrada:
        3.     Adicionar b à lista de adjacência de a   
        4.     Adicionar a à lista de adjacência de b    // Pois o grafo é não direcionado
        
        5. Criar um vetor visitado[n] e inicializar todos como falso   // Controlar quem já foi visitado
        6. Criar uma lista chamada tamanhos             // Guardar os tamanhos de cada grupo (componente)
        
        7. Para cada astronauta s de 0 até n - 1:
        8.     Se visitado[s] for falso:                
        9.         Inicializar tamanho ← 0              // Inicializar  novo grupo
        10.        (enqueue)Enfileirar s na fila S               
        11.        Marcar visitado[s] como verdadeiro  
        
        12.        (While) Enquanto a fila S não estiver vazia: 
        13.            (dequeue) Remover o primeiro elemento da fila e armazenar em u
        14.            Incrementar tamanho em 1         // Contar quantos vértices há nesse grupo
        
        15.            Para cada vizinho v em Adj[u]:   
        16.                Se visitado[v] for falso:
        17.                     (enqueue) Enfileirar v na fila S   // Explorar esse novo vértice
        18.                    Marcar visitado[v] como verdadeiro
        
        19.        Adicionar o valor de tamanho à lista tamanhos  // Salvar o tamanho do grupo
        
        20. Inicializar total ← 0                      // Vai guardar a soma de todos os pares possíveis
        21. Inicializar soma ← 0                       // Vai acumular os tamanhos de grupos anteriores
        22. Para cada t em tamanhos:
        23.     Incrementar total com t * soma         // Calcular pares entre o grupo atual e os anteriores
        24.     Incrementar soma com t                 // Atualizar soma para o próximo grupo
        
        25. Retornar total                            

### 4. Criação do Código em Python
                
```python
import math
import os
import random
import re
import sys
from collections import defaultdict, deque


def journeyToMoon(n, astronaut):
    # Construir o grafo com os astronautas do mesmo pais
    graph = defaultdict(list)
    for a, b in astronaut:
        graph[a].append(b)
        graph[b].append(a)

    visited = [False] * n

    # Busca em largura (BFS) para encontrar grupos conectados
    def bfs(start):
        queue = deque([start])
        visited[start] = True
        count = 1
        while queue:
            node = queue.popleft()
            for neighbor in graph[node]:
                if not visited[neighbor]:
                    visited[neighbor] = True
                    queue.append(neighbor)
                    count += 1
        return count

    # Encontrar os tamanhos dos grupos (componentes)
    component_sizes = []
    for i in range(n):
        if not visited[i]:
            size = bfs(i)
            component_sizes.append(size)

    # Calcular o numero total de pares validos (de paises diferentes)
    total_pairs = 0
    total_astronauts = 0
    for size in component_sizes:
        total_pairs += size * (n - size - total_astronauts)
        total_astronauts += size

    return total_pairs

if __name__ == '__main__':
    fptr = open(os.environ['OUTPUT_PATH'], 'w')

    first_multiple_input = input().rstrip().split()

    n = int(first_multiple_input[0])
    p = int(first_multiple_input[1])

    astronaut = []

    for _ in range(p):
        astronaut.append(list(map(int, input().rstrip().split())))

    result = journeyToMoon(n, astronaut)

    fptr.write(str(result) + '\n')

    fptr.close()
```


## Resultado Final

**Figura 3:** Mostrar que o código foi aceito.

<img src="https://raw.githubusercontent.com/projeto-de-algoritmos-2025/Grafos1_Jedi/refs/heads/main/Imagens/PA1_ResultadoJtM.png" alt="Descrição" width="800"/>


## Submissões

**Figura 4:** Mostrar as submissões enquanto tenatava resolver.

<img src="https://raw.githubusercontent.com/projeto-de-algoritmos-2025/Grafos1_Jedi/refs/heads/main/Imagens/PA1_SubmissoesJtM.png" alt="Descrição" width="800"/>

## Histórico de versão

| Versão | Data | Descrição | Autora | Revisora |
| ------ | ---- | --------- | ------ | -------- |
|1.0 |15/04/2025|Criação do Documento|Larissa Stéfane | Mylena Angélica |
|1.0 |15/04/2025| Adição do Código em Python |Larissa Stéfane | Mylena Angélica | 
|1.0 |16/04/2025| Adição das capturas de tela |Larissa Stéfane | Mylena Angélica | 
|1.0 |17/04/2025| Adição das submissões |Larissa Stéfane | Mylena Angélica | 
