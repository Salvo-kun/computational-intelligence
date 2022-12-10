# Computational Intelligence - Lab 3

# Collaboration
This work was done in collaboration with: 
* Maria Rosa Scoleri    (s301841)
* Jonathan Damone       (s301514)

# Sources
* [Materials from course's repository](https://github.com/squillero/computational-intelligence/blob/master/2022-23/)
* [Alpha-beta pruning on Wikipedia](https://en.wikipedia.org/wiki/Alpha%E2%80%93beta_pruning)

# Methods

# Task 3.1: Fixed-Rule Strategy
This is a fixed strategy based on the current Nim state and the possible moves from that state.

It is based on the idea of checking if there is a winning move in the next state after our move, and avoid it (avoids oppenent's win with depth = 1) . 
Moreover, if a winning move is available, it is chosen.

The algorithm, based on all possible moves from the current state, simply does this:
- if there is a winning move, chooses it
- if there is not a winning move but the move puts the opponent in a winning situation, discards it, otherwise chooses the first move possible, even if not optimal (obliged to do a move)

Notice that a situation is considered winning for an opponent if he can win in one move, i.e. if there is one row with at most n elements (where n is k if it is defined, otherwise is the current number of objects in the row)

# Task 3.2: Evolved strategy

## General information
This strategy is based on a GA. The parameters of the GA are the following:
- NUM_GENS = 100    
- POPULATION_SIZE = 10
- OFFSPRING_SIZE = 20
- TOURNAMENT_SIZE = 2
- STEADY_STATE_LIMIT = 5
- GENOME_LENGTH = 11

The GA has a population of individuals which are real values in [0, 1] encoding which rules are taken into account to evaluate the score of a move.
The rule found is applied to the values of the rows (two by two until a result is found) of the state of the nim board after a move and outputs an integer value: the lower the score, the better the move (i.e. the move with minimum score is chosen).

The population evolves either by mutation or by crossover, the individuals for genetic operators are chosen from tournaments and the algorithm stops if no improvements are made after some generations (steady state).

The strategy uses the individual and the current state of the Nim board. It first evaluates all possible moves and calculates a score applying all the rules which compose the individual.
Then, it chooses the move with the lowest score.

## Individual encoding
The individual is encoded as a rule according to the following specifications:
- an individual genome is a concatenation of 1 or more basic expressions between two operands (a and b)
- a basic expression is made by a op b, so it is represented by three components, i.e. genes (a, op, b)
- two expressions can be concatenated by an op, i.e. one extra gene

Thus, GENOME_LENGTH must be of the form 4*n + 3 with n > 0, since it must be made at least by a basic expression (3 genes) and then n concatenated basic expressions (3 genes + 1 gene for concatenation).
Moreover, each gene is evaluated like this:
- for a and b, if their gene is < 0.5, their value is kept unchanged, otherwise they are preceded by a bitwise not
- op will be an AND operator if its gene is < 0.5, otherwise it will be an OR operator.

This way, an individual encodes a boolean function with two inputs (a, b) and combines them using biwise not, or, and operators.

Notice that if genome length > 3 (i.e. 7, 11, 15...) there is the probability it will converge to the nim-sum solution, altough this becomes less probable when the length increases, having instead the highest probability when GENOME_LENGTH = 7. 
Here, GENOME_LENGTH = 11 was used and the solution presented did not converge to the nim-sum one. 

Xor operator was not added to the set of possible ones in order to reduce the probability of converging to nim-sum solution.

## Fitness function
The fitness of an individual is calculated as the percentage of won matches against the optimal rule and the random rule.

# Task 3.3: MinMax strategy
MinMax strategy follows minmax approach, hence the name. Starting from a a Nim state, it finds the best move by evaluating all possible moves (minimizing the child moves for the player, while maximizing the child moves for its opponent). There are two versions of minmax:
- without pruning
- with bounded alpha-beta pruning and caching

The first version simply checks all the possible moves and will not converge in a reasonable amount of time as soon as the nim size grows.

The second version is able to perform pruning based on the alpha-beta pruning strategy and it also exploits a bound (passed externally) to eventually stop at some depth (not optimal but required in order to always get a move in a reasonable amount of time). Caching is used to avoid revisiting already visited situations and its size is bounded to 1M entries.

# Task 3.4: Reinforcement Learning strategy
This approach is based on a training phase on a specific Nim game. Here, a table of the explored states is used to keep track about a state and its associated reward. This lookup table is used to decide which move has to be chosen (the one associated with the state which has the ighest reward).

In the training phase, there is an adjustable balance between exploitation and exploration, bases on a factor (randomness) which decide if a given move has to be chosen based on the best reward or by chance. Here, the algorithm plays against a random strategy, considering winning move from the RL's turn as positively rewarded and winning moves in the random's turn as negatively rewarded.

Notice that, since there are too many states for a nim-size > 5, it is preferred not to initialize Q with all states but to add them each time they are visited.
For our tests we used this configuration:
- training_epochs = 500
- randomness = 0.2
- learning_rate = 0.3
- max_depth = 20

# Results
Out of 88 random matches (all combinations of size and k, with size going from 2 to 9), played both as first player and as second player to avoid bias, the above mentioned strategies produced these results:

- Fixed strategy win rate against Optimal strategy was 13.64 % (12.0/88)
- Fixed strategy win rate against Random strategy was 78.41 % (69.0/88)
- Evolved strategy win rate against Optimal strategy was 20.45 % (18.0/88)
- Evolved strategy win rate against Random strategy was 79.55 % (70.0/88)
- MinMax strategy win rate against Optimal strategy was 15.91 % (14.0/88)
- MinMax strategy win rate against Random strategy was 77.27 % (68.0/88)
- RL strategy win rate against Optimal strategy was 12.5 % (11.0/88)
- RL strategy win rate against Random strategy was 51.14 % (45.0/88)

The evolved strategy (trained with the parameters specified in the previous section) is based on the best individual found in one of the runs, which represents the following rule: (a & !b) & (!a | b) | (a | b) (genome: [0, 0, 1, 0, 1, 1, 0, 1, 0, 1, 0]).
We got good performances from the bounded-depth minmax with alpha-beta pruning, where the depth was bounded to 5, against both optimal and random strategies. 
This is potentially the best model because it can converge to the optimal solution, if the depth has a larger bound, however it increases time and memory consumption quickly.
The RL strategy obtains accetptable performances against the optimal strategy, however it performs worse than the other strategies against the random strategy.
