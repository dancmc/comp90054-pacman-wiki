#### Link to the branch - https://gitlab.eng.unimelb.edu.au/mneguib/comp90054-pacman-922939/tree/Ashwin's-MCTS

# Design of the approach

Monte Carlo tree search can be used in finding the best possible moves by building a tree by selecting random actions from all possible actions and keeping track of the set of moves that have already been made. This algorithm started off in the motive to implement MCTS but took a turn to implement something similar to an exhaustive search or more likely a random simulation. The algorithm generates an action and the gamestate is evaluated. A finite number of actions are randomly generated and this gamestate is evaluated based on that. For each of the possible actions, the gamestate is evaluated with keeping track of the next few actions and the maximum valued actions is chosen. The action of the enemy is not taken into consideration but a few feature values take this into account by calculating the distance from the nearest ghost. Both the agents in this algorithm are made to be offensive as I was curious on how well only offensive agents would do in this contest. 

# Features chosen for evaluation purposes

The idea for choosing the following feature values was based on baselineTeam.py. Distances to various factors, score of the successor state and the food eaten by the agent  were calculated and weights were assigned accordingly. The features are as follows:

* Distance to nearest ghost 
* Distance to nearest boundary
* Distance to nearest food
* Distance to nearest power capsule
* Successor states score
* Food eaten

Based on these features, the gamestate for an action is evaluated. Each of these features are prioritised based on the weights given to them. However, there seemed to a problem as the agent was looping around as the priorities may have tied up in a few instances. For example, the agent chose not to eat the food as eating the food would reduce the evaluated value in the further steps and the best actions resulted in the agents looping around the same place until a ghost or an enemy pacman approached it. 

![Screenshot_2018-10-14_at_8.39.15_PM](/uploads/5694ab44444c59f0ac88b2a794603d4d/Screenshot_2018-10-14_at_8.39.15_PM.png)

# Weights for the evaluation process

The weights assigned to the feature values were split into four categories. They are as follows:

*  If the enemy agent is not visible (Used mainly at the starting of the game)
*  If the enemy agent is visible and the capsule is not eaten
*  If the enemy agent is visible and the capsule has recently been eaten
*  If the enemy agent is visible and the capsule is about to expire

The weights in each of these categories were assigned based on the priorities for each feature value to get a favourable outcome and was mostly based on calculating the value for a favourable outcome and a few trial and error runs. The weights calculated based on priorities led to a clash in conflict in prioritising each features as the values sometimes may tie. During this, the agents loop around until the values change which happens when the enemy pacman starts eating or when they approach our agents. 

# Analysis of this approach

The weights assigned to the features were not the best and hence did not bring out the best performance of this approach. Prioritising based on weights was the intent but there were a few instances where the values generated may have tied up for different features making the agent indecisive and hence looping with the same actions. With an optimal set of weights, the performance could have been made better. Another problem was that if the enemy's defensive agent was able to take down one of our agents, the other agent was also likely to fall prey to the enemy's defence. Similarly, if our agents were stuck in front of a ghost or were looping, it gives the enemy's offensive agent a chance to eat all the food without any problem as we have no defence in our approach. 

# Improvements that could have been made

*  The conflict within features can be avoided by giving better weights and taking care of the looping if it arises but adding more conditions to our approach. The number of features could have been reduced in order to avoid such conflict and concentrate on a few features to bring out the best performance of our agent. 

*  The looping could have also been avoided if I had kept track of the actions and if there had been a loop or if the actions were repeating after a threshold, a random action could have been generated to avoid the loop.

*  If our agents could not get all the food before the enemy's offensive agent or if the enemy's offensive agent outperforms our agent, we could lose the game easily. Adding a defence agent would eliminate this problem but doesn't support the intent of this approach which is to be all out offensive. 
