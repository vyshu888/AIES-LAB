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
![output](https://github.com/vyshu888/AIES-LAB/blob/53a71e7e217ee837014bba6a56f64821c671e161/Screenshot%202026-07-07%20114031.png)
## EXPERIMENT -7 A* ALGORITHM
```
from queue import PriorityQueue

class State(object):
    def __init__(self, value, parent, start=0, goal=0):
        self.children = []
        self.parent = parent
        self.value = value
        self.dist = 0

        if parent:
            self.start = parent.start
            self.goal = parent.goal
            self.path = parent.path[:]
            self.path.append(value)
        else:
            self.path = [value]
            self.start = start
            self.goal = goal

    def GetDistance(self):
        pass

    def CreateChildren(self):
        pass


class State_String(State):

    def __init__(self, value, parent, start=0, goal=0):
        super().__init__(value, parent, start, goal)
        self.dist = self.GetDistance()

    def GetDistance(self):
        if self.value == self.goal:
            return 0

        dist = 0
        for i in range(len(self.goal)):
            letter = self.goal[i]
            dist += abs(i - self.value.index(letter))
        return dist

    def CreateChildren(self):
        if not self.children:
            for i in range(len(self.goal) - 1):
                val = self.value
                val = val[:i] + val[i+1] + val[i] + val[i+2:]
                child = State_String(val, self)
                self.children.append(child)


class A_Star_Solver:

    def __init__(self, start, goal):
        self.path = []
        self.visitedQueue = []
        self.priorityQueue = PriorityQueue()
        self.start = start
        self.goal = goal

    def Solve(self):
        startState = State_String(self.start, None, self.start, self.goal)

        count = 0
        self.priorityQueue.put((0, count, startState))

        while not self.path and not self.priorityQueue.empty():

            closestChild = self.priorityQueue.get()[2]

            closestChild.CreateChildren()

            self.visitedQueue.append(closestChild.value)

            for child in closestChild.children:

                if child.value not in self.visitedQueue:

                    count += 1

                    if child.dist == 0:
                        self.path = child.path
                        break

                    self.priorityQueue.put((child.dist, count, child))

        if not self.path:
            print("Goal is not possible!", self.goal)

        return self.path


if __name__ == "__main__":

    start1 = "vyshu"
    goal1 = "ushvy"

    print("Starting...")

    a = A_Star_Solver(start1, goal1)

    a.Solve()

    for i in range(len(a.path)):
        print("{} ) {}".format(i, a.path[i]))
```
![output](https://github.com/vyshu888/AIES-LAB/blob/faaf9f74cc8d08f4725b96ab573a9d9137f24e05/Screenshot%202026-07-14%20110639.png)
## EXPERIMENT -8 (HILL CLIMBING ALGORITHM)
```
import random
def objective_function(solution):
    return sum(solution)

def generate_neighbor(current_solution):
    neighbor = current_solution[:]
    index = random.randint(0, len(neighbor) - 1)
    neighbor[index] = 1 - neighbor[index]
    return neighbor

def hill_climbing():
    current_solution = [random.randint(0, 1) for _ in range(10)]
    current_fitness = objective_function(current_solution)
    while True:
        neighbor = generate_neighbor(current_solution)
        neighbor_fitness = objective_function(neighbor)
        if neighbor_fitness >= current_fitness:
            current_solution = neighbor
            current_fitness = neighbor_fitness
        else:
            break

    return current_solution, current_fitness
best_solution, best_fitness = hill_climbing()

print("Best Solution:", best_solution)
print("Best Fitness:", best_fitness)
def simple_hill_climbing(numbers):
    current_index = 0

    while True:
        if current_index + 1 < len(numbers):
            if numbers[current_index] < numbers[current_index + 1]:
                current_index += 1
            else:
                return numbers[current_index]
        else:
            return numbers[current_index]
numbers = [1, 3, 7, 12, 9, 5]

max_number = simple_hill_climbing(numbers)

print("The maximum number in the list is:", max_number)
```
![output]()
