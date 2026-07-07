## EXPERIMENT 2
```
from itertools import permutations

def calculate_distance(route, distances):
    total_distance = 0
    for i in range(len(route)-1):
        total_distance += distances[route[i]][route[i+1]]
    total_distance += distances[route[-1]][route[0]]
    return total_distance

def brute_force_tsp(distances):
    n = len(distances)
    cities = list(range(1, n))
    shortest_route = None
    min_distance = float('inf')

    for perm in permutations(cities):
        current_route = [0] + list(perm)

        current_distance = calculate_distance(current_route, distances)
        if current_distance < min_distance:
            min_distance = current_distance
            shortest_route = current_route

    shortest_route.append(0)
    return shortest_route, min_distance
    distances = [
    [0, 10, 15, 20],
    [10, 0, 35, 25],
    [15, 35, 0, 30],
    [20, 25, 30, 0]
]
route, distance = brute_force_tsp(distances)

print("Shortest Route:", route)
print("Minimum Distance:", distance)
```
### output:
![output](https://github.com/vyshu888/AIES-LAB/blob/561d1314cce65870c912f43798226323aa8d68b6/Screenshot%202026-06-30%20114914.png)

##  EXPERIMENT -5 PUZZLE
```
from collections import deque
def bfs(start_state):
    target = [1,2,3,4,5,6,7,8,0]
    dq = deque([start_state])
    visited = {tuple(start_state): None }
    while dq:
        state = dq.popleft()
        if state == target:
            path = []
            while state:
                path.append(state)
                state = visited[tuple(state)]
            return path[::-1]
        zero = state.index(0)
        row,col = divmod(zero,3)
        for move in (-3,3,-1,1):
            new_pos = zero + move
            if 0 <= new_pos < 9:
                new_row,new_col = divmod(new_pos,3)
                if abs(row - new_row)+ abs(col - new_col) == 1:
                    neighbor = state[:]
                    neighbor[zero],neighbor[new_pos] = (
                        neighbor[new_pos],
                        neighbor[zero],
                    )
                    if tuple(neighbor) not in visited:
                        visited[tuple(neighbor)] = state
                        dq.append(neighbor)
    return None
def printSolution(path):
    for state in path:
        print(
            "\n".join(
                " ".join(map(str,state[i:i+3]))
                for i in range(0,9,3)
            ),
            end = "\n----\n",
            )
startstate = [1,3,0,6,8,4,7,5,2]
solution = bfs(startstate)
if solution :
    printSolution(solution)
    print(f"solved in {len(solution) -1} moves.")
else:
    print("No solution found")
```
## output:
![output](https://github.com/vyshu888/AIES-LAB/blob/a883c6d65ccb8d02ad16b95137bcc50e3f66a3ef/Screenshot%202026-07-07%20112842.png)

## EXPERIMENT -6 Towerofhanoi
```
def TowerOfHanoi(n,source,destination,auxiliary):
    if n == 1:
        print("Move disk 1 from source", source,"to destination", destination)
        return 
    TowerOfHanoi(n-1,source,auxiliary,destination)
    print("Move disk",n,"from source",source,"to destination",destination)
    TowerOfHanoi(n-1,auxiliary,destination,source)
n = 4
TowerOfHanoi(n,'A','B','C')
```
![output]()
