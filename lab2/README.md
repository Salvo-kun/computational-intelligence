# Computational Intelligence - Lab 2

# Collaboration
This work was done in collaboration with: 
* Maria Rosa Scoleri    (s301841)
* Jonathan Damone       (s301514)

# Sources
* [Materials from course's repository](https://github.com/squillero/computational-intelligence/blob/master/2022-23/)

# Methods

## Genetic Algorithm
Many factors were evaluated empirically running grid searches and varying factors. A small example of grid search used is provided in the code itself, varying the size of the population, the size of the offspring and the size of the tournament. In this analysis, for the sake of showing the best results detected, it has been chosen to keep the best parameters for each N. 

Moreover, it has been noticed that starting from an empty solution allows to reach overall better local minima before finding a steady state. 

An individual is a list of 1s and 0s, indicating if the i-th list of the problem is taken or not. 

The chosen fitness is based on a tuple: 
*   First element is how many different elements from the chosen lists are covered (goal is N, otherwise a solution is not found), higher is better 
*   Second element is how many elements there are in total in the chosen lists (goal is to minimize this value), lower is better 

Thus, given a problem with N defined, the starting population is made of the same individuals which are lists of 0s only (i.e. no lists chosen from the problem). 

In every generation, an offspring is generated and for each child either two situations can occur: 
*   A random mutation, with probability 0.3 occurs, inverting if a list of a solution is taken or not 
*   Two parents are selected (each one out of two random ones from the population, choosing the best based on its fitness) and the generated a child through crossover (random cut unites two slics of each parent)  

The algorithm ends on two conditions: 
*   it loops NUM_GENS times
*   a steady state is detected, i.e. the best individual of the population did not improve in the last 50 generations 

A graph displays the trend of the fitness (two curves since made of two elements) for each execution.

# Results
Solution found for N=5: w=5 (bloat=0%) after 13 generations. Tournament size: 2. Population size: 7. Offspring size: 13. Evaluated fitness 176 times.

Solution found for N=10: w=10 (bloat=0%) after 15 generations. Tournament size: 5. Population size: 18. Offspring size: 28. Evaluated fitness 438 times.

Solution found for N=20: w=24 (bloat=20%) after 21 generations. Tournament size: 2. Population size: 22. Offspring size: 40. Evaluated fitness 862 times.

Solution found for N=100: w=193 (bloat=93%) after 16 generations. Tournament size: 14. Population size: 200. Offspring size: 400. Evaluated fitness 6600 times.

Solution found for N=500: w=1388 (bloat=177%) after 18 generations. Tournament size: 11. Population size: 663. Offspring size: 1177. Evaluated fitness 21849 times.

Solution found for N=1000: w=3266 (bloat=226%) after 26 generations. Tournament size: 8. Population size: 1326. Offspring size: 1757. Evaluated fitness 47008 times.
