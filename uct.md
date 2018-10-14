<h1>UCT Wiki
Link to branch :

<h1>Basis for MCTS/UCT

The UCT approach is based on MCTS, which runs multiple simulations to determine the action with the highest average reward. Every round starts at the root node, then a policy (UCB) is used to select an action leading to s’. If s’ is a leaf node, it is expanded and then a simulation is run from it, with the reward backpropagated. If s’ is terminal, a reward is directly calculated and backpropagated. If s’ is not a leaf, then UCB is applied to find another s’.

Simulations were run to a depth of 4, then a reward was calculated for the end state and backpropagated. For example, if the leaf node was at depth 3, a simulation was run from it for 1 more step. We also defined terminal nodes as nodes with a depth of 4. The advantage of UCT is that it tries to balance exploration and exploitation, and will return the arm with the best potential reward, as compared with the exhaustive simulation technique which would return the average reward of all the paths from that arm. 

<h1>MCTS vs Exhaustive Simulation
For example, say going West (and 3 more steps) ended up with a reward of 5 for 20% of the possible paths, and 1 for the other 80%, and East resulted in a reward of 3 for all possible paths. The exhaustive simulation would return an expected value of 1.8 for West and 3 for East, and therefore choose East. Intuitively however, we would want to pick West because there were some paths leading to a higher reward. Theoretically, UCB would explore the reward 5 paths more often than the 1 reward paths due to its exploitation aspect, and therefore return a higher expected value for West, possibly choosing West. This would therefore give it an edge over the simple exhaustive simulation strategy.

The other advantage of UCT/MCTS over exhaustive simulation is that it is an online strategy and is interruptible. We can therefore run it up to the max computational budget of 1 second, or cut it short to run faster games, and it should be able to return a reasonable guess at the best action. Exhaustive simulation on the other hand generates the path tree through BFS, and it therefore cannot be interrupted, or the results would at least be very difficult to interpret.

<h3>Evaluation of terminal states
We handcrafted rewards to evaluate terminal states depending on the mode the agent was in - offence, return food (for offence), or defence. These evaluations were also shared with the exhaustive simulation approach. For offence, we used the remaining food, distance to nearest food, increase in score, and distance to nearest enemy as the evaluation metrics. For defence, we used the distance to nearest enemy and a bonus for eating a pacman, while giving it a bonus for lingering near the middle of the map if there were no visible enemies. For return, we used the distance to nearest border home square and the increase in score as incentive, and it would naturally avoid dying to prevent the distance from increasing.


<h3>Conclusion and Future Work
Indeed, in our head to head matches between agents on randomly generated maps, UCT beat out approx Q learning and exhaustive simulation most of the time. The main issue with UCT, as with exhaustive simulation, is that the rewards for terminal states are handcrafted and mostly arbitrary, making it difficult to determine the best values (if they exist). We would want to experiment with different methods of calculating the value of terminal states and also vary the exploration parameter in the future.