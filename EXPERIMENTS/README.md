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
