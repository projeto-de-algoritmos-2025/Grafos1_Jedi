# Desafio Diameter Minimization
- Tema: Grafos
- Nível: Expert
- Linguagem: Python <br> <br>

![image](https://github.com/user-attachments/assets/b70a4be5-5e34-4026-b697-fc35554bacc9)

## Enunciado
We define the diameter of a strongly-connected oriented graph, G=(V,E) , as the minimum integer d  such that for each u,v E G there is a path from u to v  of length ≤d (recall that a path's length is its number of edges).

Given two integers, n and m, build a strongly-connected oriented graph with n vertices where each vertex has outdegree m and the graph's diameter is as small as possible (see the Scoring section below for more detail). Then print the graph according to the Output Format specified below.

Here's a sample strongly-connected oriented graph with 3 nodes, whose outdegree is 2 and diameter is 1.
<br> <br>
![image](https://github.com/user-attachments/assets/e942cece-1d52-43f4-9cd8-09a633805996)
<br> <br>

Note: Cycles and multiple edges between vertices are allowed.

### Input Format

Two space-separated integers describing the respective values of n (the number of vertices) and m (the outdegree of each vertex).

### Constraints<br> <br>
![image](https://github.com/user-attachments/assets/fb80c5ee-50f7-485a-8f80-b1d9be54df24)
<br> <br>
Scoring

We denote the diameter of your graph as d and the diameter of the graph in the author's solution as s. Your score for each test case (as a real number from 0 to 1 ) is:<br> <br>

![image](https://github.com/user-attachments/assets/afd281f9-3c5c-48ae-8881-abacf6feadd6) <br> <br>

### Output Format

First, print an integer denoting the diameter of your graph on a new line.
Next, print  lines where each line  () contains  space-separated integers in the inclusive range from  to  describing the endpoints for each of vertex 's outbound edges.

### Sample Input 0

5 2
### Sample Output 0

2
1 4
2 0
3 1
4 2
0 3
## Código a ser completado

![image](https://github.com/user-attachments/assets/734d5051-c469-4504-8301-6366905e2208)


## Resolução
```python
    def eprint(*args, **kwargs):
        print(*args, file=sys.stderr, **kwargs)
    
    t0 = time()
    
    #N, D = 787, 3
    N, D=[int(i) for i in input().strip().split(' ')]
    
    RN=range(N)
    RD=range(D)
    
    MINDIAM=1
    NN=D
    while NN < N: NN *=D; MINDIAM+=1
    eprint("MINDIAM=",MINDIAM)
    
    out=[[] for n in RN]
    
    def diam(n0):
        dis=[9999]*N
        q = deque()
        for o in out[n0]:
            dis[o]=1
            q.append(o)
        while len(q):
            n = q.popleft()
            dd=dis[n]+1
            for o in out[n]:
                if dis[o] <= dd: continue
                dis[o]=dd
                q.append(o)
        r=max(dis)
        #eprint("dis[%d]=" % n0, dis)
        #eprint("diam[%d]="%n0,r, " mind=",md)
        return r
    
    def diamall():
        lo=N
        hi=0
        for n in RN:
            d=diam(n)
            lo=min(lo, d)
            hi=max(hi, d)
        eprint("DIAMALL:",lo,hi)
        return hi
    
    o=1
    for n in RN:
        for d in RD:
            out[n].append(o)
            o+=1
            if o==N: o=0
    
    ans=diamall()
    eprint("t=", time()-t0)
    print(ans)
    
    for n in RN:
        print(*out[n])
        #if n>10: eprint("BREAK"); break
```


## Resultado

![image](https://github.com/user-attachments/assets/28579660-ae6b-4e5a-850b-a7111a0b8569)   
     
![image](https://github.com/user-attachments/assets/2525e7ba-6364-428d-893c-6de811ef18fc)


## Histórico de versão
| Versão | Data       | Descrição | Autora | Revisora |
|--------|------------|-----------------------|--------|----------|
| 1.0    | 15/04/2025 | Criação do documento | Mylena | Larissa |
| 1.1    | 17/04/2025 | Formatação e adição do histórico de versão | Mylena | Larissa |


