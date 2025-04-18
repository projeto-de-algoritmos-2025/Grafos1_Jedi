# Desafio Find the Path

- Tema: Grafos
- Nível: Difícil
- Linguagem: Python <br> <br>

![image](https://github.com/user-attachments/assets/373e92bd-bcb4-4ae9-9d2f-9e042ff539ff)
 <br> <br>

## Enunciado
![image](https://github.com/user-attachments/assets/fc82b812-a17a-4c11-986c-f150d598f06e)

![image](https://github.com/user-attachments/assets/d7cdd5c1-9eda-4b03-b799-e963bf6fe673)


## Explanation

![image](https://github.com/user-attachments/assets/1c7bd808-5478-45e6-9827-104345ab0c61)
![image](https://github.com/user-attachments/assets/bba11bae-7742-4705-8142-f506e9e42731)
![image](https://github.com/user-attachments/assets/c4182019-4031-4051-bbc6-4f6a11632d1b)

## Código a ser implementado
![image](https://github.com/user-attachments/assets/0256053e-4a2b-488b-b029-d80f6eb6ad38)
![image](https://github.com/user-attachments/assets/723e42e8-c0c6-4a46-a01b-d0422dfe80aa)


## Resolução
```python
    import sys
    from heapq import *
    
    num_rows = 0
    num_cols = 0
    num_queries = 0
    table = []
    query_dict = {}
    query_results = []
    shortest_paths = []
    visited = []
    
    
    def read_input():
        global num_rows, num_cols, num_queries, table, query_dict, query_results, shortest_paths, visited
        
        num_rows, num_cols = map(int, input().split())
        for i in range(num_rows):
            row = []
            entry_list = input().split()
            for j in range(num_cols):
                row.append(int(entry_list[j]))
            table.append(row)
    
        shortest_paths = [ [ sys.maxsize for j in range(num_cols) ] for i in range(num_rows) ]
        visited = [ [ False for j in range(num_cols) ] for i in range(num_rows) ]  
        
        num_queries = int(input())
        for i in range(num_queries):
            query_results.append(sys.maxsize)
            query = list(map(int, input().split()))
            if query[1] > query[3]:
                query = (query[2], query[3], query[0], query[1])
            else:
                query = (query[0], query[1], query[2], query[3])
            
            if query not in query_dict:
                query_dict[query] = []
            query_dict[query].append(i)
        
        
    def is_valid_square(row, col, left_bound, right_bound):
        return row >= 0 and row < num_rows and col >= left_bound and col <= right_bound
    
        
    def process_single_square(start_row, start_col, left_bound, right_bound):
        global shortest_paths, visited
        
        for i in range(num_rows):
            for j in range(left_bound, right_bound + 1):
                shortest_paths[i][j] = sys.maxsize
                visited[i][j] = False  
        
        bfs_heap = []
        shortest_paths[start_row][start_col] = table[start_row][start_col]
        heappush(bfs_heap, (table[start_row][start_col], start_row, start_col))
        
        while bfs_heap:
            weight, row, col = heappop(bfs_heap)
            if visited[row][col]:
                continue         
                
            visited[row][col] = True
            
            if is_valid_square(row - 1, col, left_bound, right_bound) and not visited[row - 1][col] :
                if weight + table[row - 1][col] < shortest_paths[row - 1][col]:
                    shortest_paths[row - 1][col] = weight + table[row - 1][col]
                    heappush(bfs_heap, (weight + table[row - 1][col], row - 1, col))
            if is_valid_square(row + 1, col, left_bound, right_bound) and not visited[row + 1][col]:
                if weight + table[row + 1][col] < shortest_paths[row + 1][col]:
                    shortest_paths[row + 1][col] = weight + table[row + 1][col]
                    heappush(bfs_heap, (weight + table[row + 1][col], row + 1, col))
            if is_valid_square(row, col - 1, left_bound, right_bound) and not visited[row][col - 1]:
                if weight + table[row][col - 1] < shortest_paths[row][col - 1]:
                    shortest_paths[row][col - 1] = weight + table[row][col - 1]
                    heappush(bfs_heap, (weight + table[row][col - 1], row, col - 1))               
            if is_valid_square(row, col + 1, left_bound, right_bound) and not visited[row][col + 1]:
                if weight + table[row][col + 1] < shortest_paths[row][col + 1]:
                    shortest_paths[row][col + 1] = weight + table[row][col + 1]
                    heappush(bfs_heap, (weight + table[row][col + 1], row, col + 1))
                            
    
    def process_only_one_column(start_row, col_index):
        shortest_paths = [ sys.maxsize for i in range(num_rows) ]
        shortest_paths[start_row] = table[start_row][col_index]
        
        weight = table[start_row][col_index]
        for i in reversed(range(0, start_row)):
            weight = weight + table[i][col_index]
            shortest_paths[i] = weight
        
        weight = table[start_row][col_index]
        for i in range(start_row + 1, num_rows):
            weight = weight + table[i][col_index]
            shortest_paths[i] = weight
            
        return shortest_paths
        
        
    def process_queries_by_column(left_col, right_col, whole_queries):
        global query_results
            
        if not whole_queries:
            return
    
        if left_col  >= right_col:
            for query in whole_queries:
                query_indexes = whole_queries[query]
                paths = process_only_one_column(query[0], left_col)
                new_min = min(query_results[query_indexes[0]], paths[query[2]])
                for q_index in query_indexes:
                    query_results[q_index] = new_min
            return
        
        paths = []
        middle_col = (left_col + right_col) // 2
        for i in range(num_rows):
            process_single_square(i, middle_col, left_col, right_col)
            for query in whole_queries:
                query_indexes = whole_queries[query]
                tmp_w = shortest_paths[query[0]][query[1]] + shortest_paths[query[2]][query[3]] - table[i][middle_col]
                if tmp_w < query_results[query_indexes[0]]:
                    for q_index in query_indexes:
                        query_results[q_index] = tmp_w
        
        queries_on_left = {}
        queries_on_right = {}
        for query in whole_queries:
            query_indexes = whole_queries[query]
            if query[3] < middle_col:
                queries_on_left[query] = query_indexes
            if query[1] > middle_col:
                queries_on_right[query] = query_indexes
                                
        process_queries_by_column(left_col, middle_col - 1, queries_on_left)
        process_queries_by_column(middle_col + 1, right_col, queries_on_right)
            
            
    def print_query_results():
        for q_result in query_results:
            print(q_result)
        
        
    if __name__ == '__main__':
        read_input()
        process_queries_by_column(0, num_cols - 1, query_dict)
        print_query_results()
```

 ## Resultado 
![image](https://github.com/user-attachments/assets/2d90c846-e9e3-49a1-af77-9230be89c7a0)
![image](https://github.com/user-attachments/assets/597a3aef-18fa-47fd-b2a9-7e911c8309f4)

## Histórico de versão
| Versão | Data       | Descrição | Autora | Revisora |
|--------|------------|-----------------------|--------|----------|
| 1.0    | 15/04/2025 | Criação do documento | Mylena | Larissa |
| 1.1    | 17/04/2025 | Mudança da questão, nome do arquivo e adição da resolução  | Mylena | Larissa |
| 1.2    | 17/04/2025 | Formatação e adição do histórico de versão | Mylena | Larissa |

 

   
     

## Resultado Final

