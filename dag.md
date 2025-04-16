# Desafio DAG Queries
- Tema: Grafos
- Nível: Difícil
- Linguagem: Python <br> <br>

![image](https://github.com/user-attachments/assets/9339b458-3d5a-4301-8777-c91246b81ecf) <br> <br>

## Enunciado
You are given a Directed Acyclic Graph (DAG) with  vertices n and m  edges. Each vertex v  has an integer, av, associated with it and the initial value of av is 0  for all vertices. You must perform q queries on the DAG, where each query is one of the following types:

- 1 u x: Set av to x for all v such that there is a path in the DAG from u to v .
- 2 u x: Set av to x for all  such that there is a path from u  to v  and av > x.
- 3 u: Print the value of au on a new line.

### Input Format

The first line contains three space-separated integers describing the respective values of n (the number of vertices in the DAG), m (the number of edges in the DAG), and q (the number of queries to perform).
Each of the m subsequent lines contains two space-separated integers describing the respective values of u and v (where 1 ≤ u, v ≤ n, u ≠ v ) denoting a directed edge from vertex u to vertex v in the graph.
Each of the q subsequent lines contains a query in one of the three formats described above.

### Constraints

![image](https://github.com/user-attachments/assets/f4b616ea-c3c4-4e70-9747-774e8cfeec0b) <br> <br>

### Output Format

For each query of type 3 (i.e., 3 u), print the value of au on a new line.

### Sample Input 0

6 5 18 <br> <br>
1 2 <br> <br>
1 3 <br> <br>
3 4 <br> <br>
2 4 <br> <br>
5 6 <br> <br>
1 1 3 <br> <br>
3 1 <br> <br>
3 2 <br> <br>
3 3 <br> <br>
3 4 <br> <br>
1 2 2  <br> <br>
3 1 <br> <br>
3 2 <br> <br>
3 3 <br> <br>
3 4 <br> <br>
2 6 7 <br> <br>
3 5 <br> <br>
3 6 <br> <br>
2 1 3 <br> <br>
3 1 <br> <br>
3 2 <br> <br>
3 3 <br> <br>
3 4 <br> <br>

### Sample Output 0

3 <br> <br>
3 <br> <br>
3 <br> <br>
3 <br> <br>
3 <br> <br>
2 <br> <br>
3 <br> <br>
2 <br> <br>
0 <br> <br>
0 <br> <br>
3 <br> <br>
2 <br> <br>
3 <br> <br>
2 <br> <br>

### Explanation 0

The diagram below depicts the changes to the graph after all type 1 and type 2 queries:<br> <br>

![image](https://github.com/user-attachments/assets/cd8feec7-0e15-4eba-b70f-eb42a0dd60b4)<br> <br>



## Resolução

   
     

## Resultado Final

