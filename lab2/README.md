# Computational Intelligence - Lab 1

# Collaboration
This work was done in collaboration with: 
* Maria Rosa Scoleri    (s301841)
* Jonathan Damone       (s301514)

# Sources
* [Materials from course's repository](https://github.com/squillero/computational-intelligence/blob/master/2022-23/)

# Methods

## Evolutionary Algorithm
Many factors were evaluated empirically running grid searches and varying factors. A small example of grid search used is provided in the code itself, varying the size of the population and the size of the offspring. In this analysis, it has been chosen population_size = 2*N and offspring_size = 1.625*population_size
Moreover, it has been noticed that starting from an empty solution allows to reach overall better local minima before finding a steady state.
An individual is a list of 1s and 0s, indicating if the i-th list of the problem is taken or not.
The chosen fitness is based on a tuple:
-   First element is how many different elements from the chosen lists are covered (goal is N, otherwise a solution is not found), higher is better
-   Second element is how many elements there are in total in the chosen lists (goal is to minimize this value), lower is better
Thus, given a problem with N defined, the starting population is made of the same individuals which are lists of 0s only (i.e. no lists chosen from the problem).
In every generation, an offspring is generated and for each child either two situations can occur:
-   A random mutation, with probability 0.3 occurs, inverting if a list of a solution is taken or not
-   Two parents are selected (each one out of two random ones from the population, choosing the best based on its fitness) and the generated a child through crossover (random cut unites two slics of each parent)
The algorithm ends on two conditions:
-   it loops NUM_GENS times
-   a steady state is detected, i.e. the best individual of the population did not improve in the last 50 generations

A graph displays the trend of the fitness (two curves since made of two elements) for each execution.

# Results
N = 5\
Found a solution with 5 elements\
N = 10\
Found a solution with 11 elements\
N = 20\
Found a solution with 28 elements\
N = 100\
Found a solution with 232 elements\
N = 500\
Found a solution with 1475 elements\
N = 1000\
Found a solution with 3391 elements