<h1>Link to files


- Base Agent : https://gitlab.eng.unimelb.edu.au/mneguib/comp90054-pacman-922939/blob/daniel/pacman-contest/agentBase.py
- Search class : https://gitlab.eng.unimelb.edu.au/mneguib/comp90054-pacman-922939/blob/daniel/pacman-contest/agentSearch.py
- Exhaustive Simulation agent : https://gitlab.eng.unimelb.edu.au/mneguib/comp90054-pacman-922939/blob/daniel/pacman-contest/agentExhaustive.py

<h1>Exhaustive Simulation

This was very similar to simulations as carried out in MCTS. The thought was that instead of sampling paths as in MCTS, we could exhaustively search all paths of length 4 from the current point, evaluate the terminal state, and backpropagate the result. As with MCTS, this would give values & simulation numbers for each direct child from the root node/current state, enabling us to choose the next action. The evaluation metric for the value of the terminal state is also discussed on the UCT page. This approach was the first one we tried that worked well (before MCTS and UCT), and in the preliminary competition with about half the teams participating actually outranked the staff top team. 

<h1>Issues with Exhaustive Simulation

However, we later realised the flaw as compared with UCT (discussed on the UCT page), where promising branches were diluted by directly averaging with less promising branches from the same parent branch. In head to head testing, this approach consistently beat the baseline agent and the q learning agent, but mostly lost to the UCT agent (as expected).

![Screen_Shot_2018-10-15_at_12.56.54_am](/uploads/1ad483b1c9f731d151ccd8b397c563d2/Screen_Shot_2018-10-15_at_12.56.54_am.png)

The image above shows that repeated simulations favouring a higher reward depth 2 state (bold lines on the left) can actually influence the value of a depth 1 state, and therefore change the choice of arm. The MCTS root node would therefore pick the left arm, while the exhaustive search would pick the right one. (Whether this is necessarily the right behaviour all the time is debatable - perhaps choosing a less variable option might sometimes be better).

<h1>Future Work

Because it shares the same evaluation functions as UCT, the same possible improvements apply.