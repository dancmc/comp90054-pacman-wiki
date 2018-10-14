Prepared by Mustafa Neguib 9229393
==================================

Value Iteration Wiki
====================

Branch for the approach where a subset of the map is calculated:

Approach 1:
<https://gitlab.eng.unimelb.edu.au/mneguib/comp90054-pacman-922939/tree/valueIterationNew>

Branch for the approach where the whole map is being calculated:

Approach 2:
<https://gitlab.eng.unimelb.edu.au/mneguib/comp90054-pacman-922939/tree/valueIteration>

We had selected Value Iteration as one of the approaches because it mainly
seemed logical to do so. What we mean by logical is that at every turn we can
calculate the V(s) values of the cells(locations) based on the state of the map.
We assigned negative rewards to the actions and assigned positive V(s) values
to the target locations such as food locations, etc…

However, once we started to plan out the implementation, we had doubts that
performing the value iterations on the whole map at every turn would require
quite a bit of time, but unfortunately, there were a number of restrictions in
place which further limited what we could and could not do.

Restrictions that further limited what we could do
--------------------------------------------------

Firstly, we were restricted by a time limit as well at every turn. We had a
limit of 1 second by the time which we had to return an action, and if we exceed
this time will receive a warning. Eventually receiving 3 such warnings or the
decision to which action to take takes more than 3 seconds results in forfeiture
or the game.

Next, since the state changes after every turn, we can not train before-hand and
use learned V(s) for the states. In addition, in the contest a random map will
also be played, which is not known to us, so as a result training will not work,
therefore all the calculations have to be performed at run-time.

Design of the approaches
------------------------

In order to implement value iteration, we looked at two designs and tried to
implement them. Both had their respective problems and challenges which
prevented us from coming to a complete solution and eventually had to abandon
value iteration altogether.

The first approach that we looked at and implemented consisted of a number of
different approaches. We will explain the main idea through an illustration.

For planning and visualization purposes we have used the following map. The map
is divided into two halves and the half line is the half line shown in
column 15 by dark green and dark brown cells. The brown cells, marked with “T”
are walls, while the light green marked with “F” are empty spaces where the
agent can move into. Walls block of access to different areas, while they guide
the agent into other areas depending on where they are placed.

The right half is the home side of the blue team, while the left half is the
home side of the red team. The agents of the blue team on the right half will
play as ghosts, and as soon as they cross the half line at column 15 they turn
into Pacman. The same applies to the red team. When the agents of the red team
cross the half line, they turn into Pacman and when they return to their half
they turn back into ghosts.

![map5](/uploads/57e0c0c966616d7adb03f26c9aa7068c/map5.png)

Figure 1: Map used for planning and visualization

In the first approach let us assume that we are the blue team, and our agents
start from locations (30,14) and (30,13) respectively. The agents play as
offensive, and defensive, where the aim of the offensive agent is to conduct
raids into the other team’s half and try to eat as many dots (shown as grey
cells in the left half of the map) it can and to return to its own half and
deposit them, gaining points for its team. The aim of the defensive agent is to
prevent the offensive agent (Pacman) of the other team to raid and eat the dots
in the opposing team’s half. In the above map, the grey cells are the dots that
our Pacman agent will have to eat during one of its raids.

The following is an actual map that is shown when the game is played. Since the
actual map is only available while the game is actually being played, the map
shown earlier is quite useful in helping us visualize what is happening and what
we want to implement.

![Screenshot__118_](/uploads/249e32d764772b9b322d089a54b950f2/Screenshot__118_.png)
Figure 2: Actual map in play

Approach 1: Calculating the Value Iteration on a subset of the map at every turn
--------------------------------------------------------------------------------

Our agents are located at (30,13) and (30,14). In this strategy, the offensive
agent is to be first moved towards the half line. Then as soon as it crosses the
half-line it turns into a Pacman, and then the closest dot is searched for and
is from where the value iteration calculations are to be performed. Essentially
the closest dot is given a positive V(s) value while the rest of the locations
between the agent’s location and the dot’s location are given the V(s) value of
0. The calculations are repeated from 0 to n-1 where n is the number of
locations between the dot and the agent’s location. This allows us to assign a
value for every location and then return an action all within the time limit.

The darkened area shows the columns whole locations for whom we want to
calculate the V(s) values. If you notice that we also have included the column
to the right of the agent. This has been done so that in the future if an agent
needs to go back if it needs to, can do so easily.

![map1](/uploads/3be9e999c966fa309c883cc67b9ac31d/map1.png)

As the agent moves forward, the shaded area will also move forward along with
the agent.
![map2](/uploads/c2137441a5ae416d95ff8ad8ed64ed36/map2.png)

The above map shows the shaded area moved one column to the left. The advantage
of this technique is that we do not perform a lot of calculations, which keeps
the execution time quite low and also returns the action much quicker.

However, there are also problems with this approach. Firstly, since we are only
looking at a small area, we do not find the optimal path which might be
available in the map, but not in the shaded area. Secondly, once the agent
crosses the half line and becomes a Pacman it has to search for the closest
food. Suppose the Pacman is at location (9,6) and the target dot is at (9,11).
Assume that the dots near the location (9,6) have already been eaten by the
Pacman. Now as we can see, in order to each (9,11) from (9,6) we can not find
any such path because the path (location cells) that fall on the path are not

Included in the shaded area, hence the food item cannot be reached using this
strategy and the Pacman will start to get stuck and oscillate within two
locations repeatedly and will have to use other techniques to help move the
pacman into some other direction. One way to solve this is to increase the size
of the shaded area, but then we will have to perform that many calculations
which will further cause problems which we do not want in the first place. To
handle this situation, what we did was to randomly select a food item other than
the currently selected food item and to try and move the agent away from the
original food item hoping that it can find a path to another dot. However, we
then found out that the Pacman does stop oscillating, but then falls back into
the oscillation as it looks for the closest dot and finds the original offending
dot again and gets stuck in the oscillation again. Another problem that we encountered
was that since the closest dot is searched for at every turn and we are using Manhattan Distance,
so there is a possibility that we may not find a dot that is actually close to our agent and
instead go after one which is far away.

![map3](/uploads/5c1a75e60b6ba2e4123befbe9010bd7d/map3.png)

The problem with such an approach is that due to the way the shaded are moves,
we encounter many situations where we have to manually implement fixes to
circumvent and to make the agent take some other action. We believe that this is
not the way how value iteration should be work and this requires too much
tinkering to just make the agent go towards the food and eat it. There are many
other situations that we had not implemented yet which would have further made
things more challenging and problematic and also be the cause of unforeseen
errors.

Approach 2: Calculating Value Iteration on the whole map at every turn
----------------------------------------------------------------------

This approach considers the whole of the map. Now the dots past the half line in
the opponent’s half are set as having positive V(s) while the rest of the
location cells are given a value of 0.

![map4](/uploads/3ff60e6501c4745ee030fb0e12ad3e1e/map4.png)

The next decision that we have to make is how to decide which location cells
will be given to the algorithm to calculate the V(s) values.

Initially, we had implemented a column-wise approach. Suppose the target dot from
where we want to start the calculations from is at (5,1) as shown in the
following map are highlighted in red.

![map5](/uploads/acd7fdf3c34c57d46ca8e1be083c99ee/map5.png)

The algorithm works backwards from the target location column by column and goes
till the whole of the map has been covered. This also has problems and it turns
out that even though the values are calculated and populated, they are sorted in
increasing size from top of the column to the bottom of the column. Once these
values have been calculated we then look for the appropriate action to take, and
this works by moving to the cell with a larger value than the current location
cell’s value. The problem with this is that once the agent moves down to (30,1)
and then moves to (28,1) it can not move upwards towards (28,5) because the
values are in an increasing order from top to bottom and not from bottom to top
as the agent needs. At this point, the agent cannot use plain value iteration to
figure out which action it has to take to reach to the next location on the
path.

Armed with the knowledge of this problem, we then decided to instead get the
legal neighbours, or the actual neighbors form the target state to the agent’s
location, and this was to be done by using Breadth First Search. This would
explore the whole of the map and return the paths where the agent can go and
this would also help us in calculating the V(s) values. This worked, and we had
retrieved a list of all accessible locations from each of the opponent’s dot
which was a target and had a positive V(s) value.

The problem lay when we were to actually calculate the V(s) values for all of
the locations in the lists. In this particular map, we have 248 location cells
where the agent can go, so as a result in order to have all of the location
cells to have V(s) values (though not optimal) we will have to run the Value
Iteration algorithm 248 times for each of the enemy dots(targets) (23 in total).
This brings the number of calculations needed to 5704. If we did not have a time
limit at every turn, then we could have used this approach, but unfortunately
due to this restriction, the time it takes the calculations to complete exceeds
this limit by many factors (give time taken here).

Final Results
-------------

Due to the problems discussed above we decided to stop work Value Iteration as
it would not work.

Possible improvements that can be made
--------------------------------------

If we have enough time, we believe that we can make improvements to Approach 2.

Firstly, we can use the strategy used in Approach 1 of only considering the
location cells on the right side of the half line if the agent is still a ghost
and to coax the agent to reach the half first before considering the dots on the
opponent’s side. This should reduce the time required to calculate the V(s) for
the location cells, but at the expense of getting an optimal path since we are
not considering the whole of map any more.

If Approach 2 works with the changes mentioned above, we believe that we do not
need Approach 1 anymore, mostly because it is extremely restricted in terms of
the location cells that it takes into account.
