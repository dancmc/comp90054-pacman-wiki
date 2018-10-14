An approximate Q learning agent decides on an action by extracting an abstract representation of a state denoted by a number of features, and multiplying each feature by a weight corresponding to that action/feature combination. These weights are updated through playing many games and observing the consequences of taking an action given a particular state. 

In a game like Freeway, there would be 4 weight vectors, for example [Up1, Up2….Upn] representing weights for features 1 to n for the action Up. This is because each action is distinct - the goal state is in an Up direction, the cars come from Left and Right, and Down is towards the start. However, in Pacman, the actions do not have intrinsic differences like this, and we can simplify this to a single weight vector applicable to any action. However, we extract features from the states resulting from each action instead. That is, to determine Q(east, s) in Freeway we would take f(s) * w(east), however in Pacman we would take f(s’) * w(generic_action), where s-east->s’.

We went through the project at http://ai.berkeley.edu/reinforcement.html, and our code structure is based on that, which is also the same algorithm as taught in lectures. For a given Q(a,s), the features are based on s’ and are as follows :
- number of ghosts 1 step away or less : will die if the ghost’s next move is towards us
- number of ghosts 2 steps away
- number of scared ghosts 1 step away or less
- binary function of whether food was eaten
- 1 - (distance to nearest food scaled between 0 and 1) : this term is higher the closer we get to food
- 1 - (distance to nearest capsule scaled between 0 and 1) : this term is higher the closer we get to a capsule, but only activated if a ghost is nearby
- free bias term of 1.0

We also designed shaped rewards for the update function :
- bonus for decrease in remaining food
- bonus for increase in score
- bonus for decrease in distance to nearest food
- penalty for dying
- bonus for eating a scared ghost

The features were designed to try and compare future states based on benefit vs risk. We initialised weights at 1 or -1 depending on whether the feature was beneficial, then trained the agent using an epsilon greedy exploration strategy (epsilon of 0.1) and an alpha of 0.2 (determined mostly by trial and error). After we ran out of training time, alpha and epsilon were fixed at 0 and the last updated weights used as fixed weights without further update.

The agent behaved remarkably well even at the beginning with simple weights, and was able to reliably beat the baseline agent within 1 or 2 games. However, it did not perform as well against our other agents, and did not learn to reliably beat either UCT or MCTS. This was possibly because we were unable to play enough training games, but also likely because alpha and the shaped rewards were not optimised.

Future directions would include playing far more games against a variety of agents, as well as experimenting with many other values of alpha and shaped rewards. The difficulty in this is that games must be played sequentially in order to update the weights, and finding optimal alpha and shaped rewards through grid search or similar would take an inordinate amount of time. Deep Q learning might be an alternative, but was beyond the scope of this project.