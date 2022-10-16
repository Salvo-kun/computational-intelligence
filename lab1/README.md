# Computational Intelligence - Lab 1

# Collaboration
This work was done in collaboration with: 
* Maria Rosa Scoleri    (s301841)
* Jonathan Damone       (s301514)

# Sources
* [Materials from course's repository](https://github.com/squillero/computational-intelligence/blob/master/2022-23/)

# Methods

## Generative Exact solution
Starting from an empty set, it adds to the frontier every element of the possible starting sets which is not a subset of the set it has to be added to and assigns a cost to them.
The cost of a new state (or node) is evaluated as how many elements are duplicated when adding a new set to an existing one.
The frontier is a priority queue whose priority function is the sum of the cost accumulated by the state to be extracted.

## Generative Heuristic solution
This solution is similar to the generative exact solution except for the priority function which is an heuristic estimated on the missing number of elements of a given state to reach the solution.

## Enhanced Greedy solution
The solution is based on a greedy approach. We select, at each iteration the subset which has the highest percentage of new elements, with respect to the solution set constructed at that moment, and which is not a subset of the current solution set.

# Results

## Generative Exact solution
N = 5 \
Found a solution with 5 elements; visited 32 nodes\
N = 10\
Found a solution with 10 elements; visited 583 nodes\
N = 20\
Found a solution with 23 elements; visited 2864 nodes\
N = 100, 500, 1000 \
Solution not found in a reasonable execution time

## Generative Heuristic solution
N = 5\
Found a solution with 5 elements; visited 4 nodes\
N = 10\
Found a solution with 10 elements; visited 4 nodes\
N = 20\
Found a solution with 28 elements; visited 5 nodes\
N = 100\
Found a solution with 192 elements; visited 6 nodes\
N = 500\
Found a solution with 1304 elements; visited 8 nodes\
N = 1000\
Found a solution with 2893 elements; visited 9 nodes

## Enhanced Greedy solution
N = 5\
Found a solution with 5 elements; visited 5 states\
N = 10\
Found a solution with 12 elements; visited 5 states\
N = 20\
Found a solution with 30 elements; visited 6 states\
N = 100\
Found a solution with 171 elements; visited 8 states\
N = 500\
Found a solution with 1256 elements; visited 12 states\
N = 1000\
Found a solution with 2913 elements; visited 13 states