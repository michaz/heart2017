When building a traffic model from location data, one can either:

- Build a model of travel behavior from location data, then build a traffic model from that.
- Build a traffic model directly, then impute behavior.

---

Given location data, one can
- Determine home/work locations
- Determine modes of transport
- Determine other trips
- Using additional data
    - census
    - land use
    - time use survey

The result will be something resembling a travel survey. This can then be simulated in the usual way.

---


Given location data, one can also

- Look at one particular day
- Connect the location data points of each user and construct moving agent
- Concurrently simulate moving agents
- Agent tries to produce behavior consistent with the evidence
    - What the agent can do:
        - Activities (with location, end time)
        - Travel (with mode of transport)
- Agent decides if its movement is feasible: Can it be at its witnessed locations in time?

---


Consider sparse location data:

- Not all activity locations will be witnessed, so trips will be missing.
- Uncertainty about start and end times of witnessed activities.
- Little to say about modes of transport.
- Ambiguity about whether location data point is at an activity location or on a trip.

An agent trying to produce behavior consistent with the data would:

- Do activities at witnessed locations
- Connect them with trips
- Pick departure times anywhere between witnessed instant and the latest required departure time to reach the next location in time
- Will get negative feedback when that doesn’t work. Try other departure times.


---


Such an agent will likely, compared to its real counterpart
- Do fewer activities
- Travel less
- Have some activities at locations where, in reality, there was none. (But which the individual still visited while passing through.)

---

### Synthetic CDRs by simulation

A plug-in for MATSim, our agent-oriented transport model.

Input:
Full transport scenario (simulated individuals executing all-day travel plans).
		synthetic ground truth
Cell coverage, which partitions the simulated geographic area into mobile phone cells.
Mobile phone usage model.
Each agent can decide in every time step whether or not to place a call
Allows investigating non-homogeneous calling behavior
Output:
Synthetic CDR data set.
Any aggregate data which can be taken from the scenario. In particular: link counts.

We consider this the available data for demand modeling. We can now study in isolation the question of how much information is lost for the simulation model if we replace travel diaries by CDRs.


---


### Create a demand model from CDRs

Convert CDR trajectories back to travel diaries:

- Create one individual for each observed caller id
- Convert each call into an activity
- Fuse calls in the same zone with no call in another zone in between
- Choose location randomly within zone
- Choose activity end time arbitrarily so that the location of the next call is reached
- Possibly insert additional locations for which there is no evidence by calls

Simulate the resulting plans. The output of this step is of the same form as the base case. The two scenarios can now be compared to assess the approximation quality.


---

Calibrate against traffic counts to

* reduce temporal uncertainty due to sparseness
* reduce bias in daily travelled distance due to expansion heuristic

Assignment of trajectory which fits the observed trace is incorporated into the agents’ behavior.

note:

Utility function is modified so that joint behavior of agents is directed towards fitting traffic counts.

---

Consider dense location data:

- Activities will be more clearly delineated
- Many data points will recognizably be from trips
- will say something about speeds.

An agent using the same strategy as in the sparse case will, compared to its real counterpart

- Do much more activities, some of them very short
- Exhibit less reaction to feedback from the transport system: Will not change its route, even if another is faster

---


### Contribution

Problem of fusing CDRs and link counts reformulated as calibrating agent behavior within a well-studied simulation framework.

Software framework to develop and evaluate methods to reconstruct agent-based scenarios from CDRs.
