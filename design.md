## number 1
Performance measure -to reach kisumu using the lowest path cost while following the highway network and avoiding unecessary routes
Environment-Kenya's highway network consisting of towns and roads connecting them
Actuators-Steering,accelerator,brakes,indicators
Sensors-GPS,cameras,road sensor,speed sensor

Defining environment
partially observavble-The trailer doesn't have complete info about it's environment and has to make informed judgements based on the little info
Stochastic-The result oof travelling is't completely predictable due ot traffic,weather ,accidents and road conditions
Sequential-The environment has variety of aspects beyond control of the agents
Dynamic-the environment can change while agent is selecting an action making the agent consult the environment before acting

## number 3

1.Setting up the problem
The trailer needs to get from Nairobi to Kisumu using the highway network we mapped in Question 2. Because we abstracted the map so that each town is a node and each highway between two towns is an edge, the problem is simple to state: pick a sequence of towns that gets the trailer from Nairobi to Kisumu with the least distance driven.
Component	Our definition
Initial state	The trailer is in Nairobi
Actions	Drive from one town to a neighbouring town along the highway
Transition model	After driving from x to y, the trailer is in y
Goal test	The trailer is in Kisumu
Path cost	Total distance in km. Every leg has a positive cost, so adding legs never makes a path cheaper

Since the question tells us to use distance as the cost, we used Uniform-Cost Search (UCS). It always expands whichever town on the frontier has the smallest total distance from Nairobi so far, so it will not settle for a longer route.
2. Distances between towns
Roads run both ways, so each link below can be driven in either direction at the same cost. These are driving distances in km between town centres, rounded to the nearest kilometre.
From	To	Distance (km)
Nairobi	Thika	42
Nairobi	Naivasha	92
Nairobi	Narok	140
Thika	Nyeri	100
Nyeri	Nyahururu	70
Nyahururu	Nakuru	65
Naivasha	Nakuru	63
Nakuru	Kericho	115
Nakuru	Eldoret	155
Kericho	Kisumu	80
Eldoret	Kisumu	130
Narok	Kisii	190
Kisii	Kisumu	115

3. Working through the search
We started at Nairobi with a cost of 0 and kept expanding the cheapest town on the frontier. We only checked whether we had reached Kisumu when it was the cheapest thing left, not the first time we saw it. That matters here, as the table shows.
Step	Node expanded (g)	Successors generated / updated	Frontier after step (ordered by g)
0	-	Initial state: Nairobi	Nairobi 0
1	Nairobi (0)	Thika 42, Naivasha 92, Narok 140	Thika 42, Naivasha 92, Narok 140
2	Thika (42)	Nyeri 142	Naivasha 92, Narok 140, Nyeri 142
3	Naivasha (92)	Nakuru 155	Narok 140, Nyeri 142, Nakuru 155
4	Narok (140)	Kisii 330	Nyeri 142, Nakuru 155, Kisii 330
5	Nyeri (142)	Nyahururu 212	Nakuru 155, Nyahururu 212, Kisii 330
6	Nakuru (155)	Kericho 270, Eldoret 310; Nyahururu via Nakuru = 220 (worse than 212, discarded)	Nyahururu 212, Kericho 270, Eldoret 310, Kisii 330
7	Nyahururu (212)	Nakuru already expanded at 155 (nothing to add)	Kericho 270, Eldoret 310, Kisii 330
8	Kericho (270)	Kisumu 350	Eldoret 310, Kisii 330, Kisumu 350
9	Eldoret (310)	Kisumu via Eldoret = 440 (worse than 350, discarded)	Kisii 330, Kisumu 350
10	Kisii (330)	Kisumu via Kisii = 445 (worse than 350, discarded)	Kisumu 350
11	Kisumu (350)	GOAL TEST PASSED, search stops	-

A few things worth pointing out from the table:
•	Narok (140) was expanded before Nyeri (142) and Nakuru (155) because its cost so far was lower, even though it ends up on a poor route to Kisumu.
•	At step 6 Nyahururu could also be reached through Nakuru at 220 km, but we already had it at 212 km via Nyeri, so we kept the cheaper one.
•	We first saw Kisumu at 350 km in step 8, but we kept going. Eldoret (310) and Kisii (330) were still cheaper than 350, so one of them might have led to a shorter route. Neither did: Kisumu via Eldoret costs 440 km and via Kisii 445 km.

4. Answer
Best route: Nairobi → Naivasha → Nakuru → Kericho → Kisumu
Total distance: 92 + 63 + 115 + 80 = 350 km
For comparison, here are the other routes we could have taken:
Route	Calculation	Total (km)
Nairobi - Naivasha - Nakuru - Kericho - Kisumu	92 + 63 + 115 + 80	350
Nairobi - Naivasha - Nakuru - Eldoret - Kisumu	92 + 63 + 155 + 130	440
Nairobi - Narok - Kisii - Kisumu	140 + 190 + 115	445
Nairobi - Thika - Nyeri - Nyahururu - Nakuru - Kericho - Kisumu	42 + 100 + 70 + 65 + 115 + 80	472
Nairobi - Thika - Nyeri - Nyahururu - Nakuru - Eldoret - Kisumu	42 + 100 + 70 + 65 + 155 + 130	562

The Kericho route is at least 90 km shorter than any alternative. It passes through four road segments, the same number as the Eldoret route, but the Nakuru to Kericho and Kericho to Kisumu legs add up to far less than Nakuru to Eldoret and Eldoret to Kisumu. The Narok and Kisii route has the fewest segments (three), yet it is longer overall because the Narok to Kisii stretch alone is 190 km. So fewer towns does not automatically mean a shorter drive.
5. Why we can trust this answer
UCS finds the best path here for two reasons. First, the graph is small and every leg has a positive distance, so the search is guaranteed to finish. Second, UCS expands towns strictly in order of cost, so by the time Kisumu reached the front of the queue at 350 km, every cheaper partial route had already been explored and none of them reached Kisumu for less. That makes 350 km the minimum on this graph.
One caveat: the distances we used are approximate. If your Google Maps readings differ by a few kilometres, the numbers in the trace will shift slightly, but the Kericho route is ahead by a wide enough margin that the answer should stay the same.
6. Conclusion
Using distance as the path cost, the best route for the trailer from Nairobi to Kisumu goes through Naivasha, Nakuru and Kericho, and covers about 350 km.



