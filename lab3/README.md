# Computational Intelligence - Lab 3

# Collaboration
This work was done in collaboration with: 
* Maria Rosa Scoleri    (s301841)
* Jonathan Damone       (s301514)

# Sources
* [Materials from course's repository](https://github.com/squillero/computational-intelligence/blob/master/2022-23/)

# Methods

# Fixed-Rule Strategy
This is a fixed strategy based on the current Nim state and the possible moves from that state.

It is based on the idea of checking if there is a winning move in the next state after our move, and avoid it (avoids oppenent's win with depth = 1) . 
Moreover, if a winning move is available, it is chosen.

The algorithm, based on all possible moves from the current state, simply does this:
- if there is a winning move, chooses it
- if there is not a winning move but the move puts the opponent in a winning situation, discards it, otherwise chooses the first move possible, even if not optimal (obliged to do a move)

Notice that a situation is considered winning for an opponent if he can win in one move, i.e. if there is one row with at most n elements (where n is k if it is defined, otherwise is the current number of objects in the row)

# Evolved strategy
This strategy is based on a GA. The parameters of the GA are the following:
- NUM_GENS = 100    
- POPULATION_SIZE = 10
- OFFSPRING_SIZE = 20
- TOURNAMENT_SIZE = 2
- USELESS_GENS = 0
- STEADY_STATE_LIMIT = 10

The GA has a population of individuals which are bit strings encoding which rules are taken into account to evaluate if a move is a winning move.
The population evolves either by mutation or by crossover, the individuals for genetic operators are chosen from tournaments and the algorithm stops if no improvements are made after some generations (steady state).

The strategy uses the individual and the current state of the Nim board. It first evaluates all possible moves and calculates a score applying all the rules which compose the individual.
Then, two possible outcomes are possible:
- there is at least "perfect move", i.e. score = 0
- there is not a perfect move

In the first case, it returns the first perfect move, while in the second case it returns the first available move since there is not a preference.

The set of rules the individual can be composed of are:
- sum
- min
- max
- mean
- stdev

N.B. my_xor rule, i.e. nim-sum, was excluded to avoid converging to the optimal solution which we already know and also to avoid bias.

The score above mentioned is calculated as a linear combination of these function (multiplied by the corresponding factor in the individual's genome: 0 if inactive, 1 if active) applied to the state of the board after a move.

# Results
Out of 100 random matches (random size and random k), played both as first player and as second player to avoid bias, the above mentioned strategies produced these results:

- Fixed strategy win rate was 4.5 % (9.0/200)
- Evolved strategy win rate was 5.5 % (11.0/200)

The evolved strategy with the above parameters found, as best individual, the one using the rule min and stdev. So it seems to prefer moves which clear the rows and minimize the standard deviation between rows.