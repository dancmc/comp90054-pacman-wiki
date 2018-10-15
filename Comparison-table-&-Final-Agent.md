<h1>Table

Win/Tie rate of the 3 candidate agents against each other and baselineTeam

|          | Baseline | Exhaustive | UCT | Approx Q |
| -------- | -------- | -------- | -------- | -------- |
| Exhaustive   | 100   | -   | 0   | 100   |
| UCT  | 100   | 100   | -   | 80   |
| Approx Q   | 100   | 0   | 20   | -   |


<h1>Final Agent

As stated in the UCT/Q learning pages, the UCT agent performed best in the head to head trials, as expected. Approx Q would likely have prevailed given enough time to train and also tweak the rewards/features.

We were originally going to submit our UCT agent based on the results above. However, the assignment was extended at the last minute to accommodate a re-run of the last pre-competition, and we used that to test our Q learning agent with pre-learned weights (with learning switched off).

Surprisingly, it ranked above the top staff team, which both exhaustive simulation and UCT had been unable to do recently. Because of the last minute nature of the extension, we assumed that most of the agents would have been the final agents, and we therefore decided to leave the Q learning agent as our final tournament agent on the basis of empirical performance in the pre-competitions (although the weights could most likely be optimised further).

n.b. Technically only the offence agent is a Q learning agent as we did not have time to code defence. The defence agent is an exhaustive simulation agent.