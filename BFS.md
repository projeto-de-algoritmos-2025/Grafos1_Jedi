# Desafio Breadth First Search: Shortest Reach
- Tema: Grafos
- Nível: Médio
- Linguagem: Python <br> <br>
![image](https://github.com/user-attachments/assets/8a269aa2-8be3-4c00-baa2-86b4e1ba056b)

## Enunciado
Consider an undirected graph where each edge weighs 6 units. Each of the nodes is labeled consecutively from 1 to n. 
You will be given a number of queries. For each query, you will be given a list of edges describing an undirected graph. 
After you create a representation of the graph, you must determine and report the shortest distance to each of the other nodes from a given starting position using the breadth-first search algorithm (BFS). 
Return an array of distances from the start node in node number order. If a node is unreachable, return -1 for that node.

Example
The following graph is based on the listed inputs:<br><br>
![image](https://github.com/user-attachments/assets/a108d599-cca7-4f7e-9120-53dbdd203efc)<br>

n = 5 // number of nodes
m = 3 // number of edges
edges = [1,2],[1,3],[3,4]
s = 1 // starting node

All distances are from the start node 1. Outputs are calculated for distances to nodes 2 through 5 : [6,6,12,-1]. Each edge is 6 units, and the unreachable node 5 has the required return distance of -1.

Function Description

Complete the bfs function in the editor below. If a node is unreachable, its distance is -1 .

bfs has the following parameter(s):
- int n: the number of nodes
- int m: the number of edges
- int edges[m][2]: start and end nodes for edges
- int s: the node to start traversals from
Returns
int[n-1]: the distances to nodes in increasing node number order, not including the start node (-1 if a node is not reachable)

Input Format

The first line contains an integer q, the number of queries. Each of the following q sets of lines has the following format:

- The first line contains two space-separated integers n and m, the number of nodes and edges in the graph.
- Each line  of the  subsequent lines contains two space-separated integers,  and , that describe an edge between nodes  and .
- The last line contains a single integer, , the node number to start from.

Constraints<br><br>
![image](https://github.com/user-attachments/assets/b0f20695-ac90-48cb-affc-50ba377e9c1c)

## Código a ser completado

![image](https://github.com/user-attachments/assets/8f96f132-e4fb-4d53-b330-0a2731488ae5)

## Resolução

    def bfs(n, m, edges, s):
    graph = [[] for _ in range(n + 1)]
    
    for edge in edges:
        u, v = edge
        graph[u].append(v)
        graph[v].append(u)
        
    distances = [-1] * (n + 1)
    distances[s] = 0
    queue = deque([s])
    while queue:
        node = queue.popleft()        
        
        for neighbor in graph[node]:
            if distances[neighbor] == -1:  
                distances[neighbor] = distances[node] + 6  
                queue.append(neighbor)
    
    return [distances[i] for i in range(1, n + 1) if i != s]
    
     

## Resultado Final
![image](https://github.com/user-attachments/assets/d4da2552-9439-490a-85b4-8e1cc5de18ff)
![image](https://github.com/user-attachments/assets/b1dee97e-3f89-436d-a74d-036ef5647685)



