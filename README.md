# Maze-Solving-with-Genetic-Algorithms
This project demonstrates the application of a genetic algorithm to solve a maze. It utilizes the DEAP library for evolutionary computation and matplotlib for visualization. The algorithm evolves a population of potential paths through the maze, gradually improving their fitness until an optimal solution is found.
<ul><li>Utilized the DEAP library for evolutionary computation and explored it to solve a maze solver</li>
<li>Enhanced understanding of genetic algorithms, experimenting with various strategies in maze solving</li>
<li>Employed Matplotlib for visualizing complex routing solutions, facilitating effective data communication and decision-making.</li>
</ul>

## Introduction to Genetic Algorithms

### Objective
Genetic algorithms are a class of optimization algorithms inspired by the process of natural selection in biology. They are designed to find the best solution to complex problems by mimicking the process of evolution.
This algorithms are powerful because they can efficiently search large, complex spaces and find optimal or near-optimal solutions. They are especially useful for problems where traditional optimization methods struggle.
### Content
**What are Genetic Algorithms?**  
Genetic algorithms are a class of optimization algorithms inspired by the process of natural selection in biology. They are designed to find the best solution to complex problems by mimicking the process of evolution.

**Why are Genetic Algorithms Powerful?**  
Genetic algorithms are powerful because they can efficiently search large, complex spaces and find optimal or near-optimal solutions. They are especially useful for problems where traditional optimization methods struggle.

**Simple Analogies**  
Think of genetic algorithms as a way of breeding solutions. Just like how traits are passed from parents to offspring in nature, genetic algorithms combine and mutate solutions to produce better ones over generations.
# Genetic Algorithm Setup
```python
creator.create("FitnessMin", base.Fitness, weights=(-1.0,))
creator.create("Individual", list, fitness=creator.FitnessMin)
toolbox = base.Toolbox()
toolbox.register("attr_direction", random.choice, ['U', 'D', 'L', 'R'])
toolbox.register("individual", tools.initRepeat, creator.Individual, toolbox.attr_direction, n=100)
toolbox.register("population", tools.initRepeat, list, toolbox.individual)
creator.create: Defines fitness and individual classes.
```
toolbox: A container for evolutionary operators.

attr_direction: Randomly selects a direction: Up (U), Down (D), Left (L), or Right (R).

individual: A sequence of 100 moves.

population: A list of individuals (solutions).
# Evaluation Function
```python
def evaluate(individual):
    x, y = start
    for move in individual:
        if move == 'U': y = max(0, y - 1)
        elif move == 'D': y = min(len(maze) - 1, y + 1)
        elif move == 'L': x = max(0, x - 1)
        elif move == 'R': x = min(len(maze[0]) - 1, x + 1)
        
        if (x, y) == end: return (0,)  # Reached the end
        if maze[y][x] == 1: break  # Hit a wall
    
    return (abs(end[0] - x) + abs(end[1] - y),)
```
evaluate: Computes the fitness of an individual by simulating its path through the maze. It returns the Manhattan distance to the end point.
# Custom Mutation Function
```def custom_mutate(individual, indpb=0.2):
    directions = ['U', 'D', 'L', 'R']
    for i in range(len(individual)):
        if random.random() < indpb:
            # Exclude the current direction to ensure mutation changes the gene
            possible_directions = [d for d in directions if d != individual[i]]
            individual[i] = random.choice(possible_directions)
    return individual,
```
Purpose<br>
The custom_mutate function is designed to introduce random changes (mutations) into an individual (a solution) in the population of the genetic algorithm. Mutation is an essential part of genetic algorithms as it helps maintain genetic diversity within the population, allowing the algorithm to explore new areas of the solution space.

<li>Parameters</li><br>
individual: This is the solution that will undergo mutation. In the context of this problem, it is a list of directions representing a path through the maze.
<br>
indpb: This is the probability of mutating each gene (direction) in the individual. In this case, indpb is set to 0.2, meaning there's a 20% chance of each gene being mutated.

<br>Operation<br>
Define Possible Directions: <br>The function starts by defining a list of possible directions (U for Up, D for Down, L for Left, and R for Right).
<br>
Iterate Through Each Gene:<br> It loops through each gene (direction) in the individual.
<br>
Random Mutation Decision: <br>For each gene, the function decides whether to mutate it based on the mutation probability indpb. This decision is made using random.random() < indpb, which generates a random float between 0 and 1 and checks if it is less than indpb.
<br>
Mutation: <br>If the gene is selected for mutation:
<br>
The function creates a list of possible directions excluding the current direction (possible_directions).
<br>
It then randomly selects a new direction from this list and assigns it to the current gene.
<br>
Return the Mutated Individual:<br> The function returns the mutated individual as a tuple.
<br>
Example
Let's consider a simple example to illustrate how custom_mutate works:

Before Mutation
```
individual = ['U', 'L', 'R', 'D', 'L']
```
<ul>
<li>Mutation Process</li>
Assume indpb is 0.2. For each gene, there's a 20% chance it will be mutated.

<li>First gene 'U': No mutation (random value > 0.2).</li>

<li>Second gene 'L': Mutation occurs (random value < 0.2), new direction might be 'U', 'D', or 'R'.</li>

<li>Third gene 'R': No mutation (random value > 0.2).</li>

<li>Fourth gene 'D': Mutation occurs (random value < 0.2), new direction might be 'U', 'L', or 'R'.</li>

<li>Fifth gene 'L': No mutation (random value > 0.2).</li>
</ul>
After Mutation
```
individual = ['U', 'U', 'R', 'R', 'L']  
```
# Example outcome
The custom_mutate function ensures that each gene in the individual has a chance to change, introducing variation and helping the genetic algorithm explore different paths in the maze.
