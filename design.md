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
1. Setting up the problem
The trailer has to get from Nairobi to Kisumu, using the highway network we mapped in Question 2. In that map, every town is a node and every highway between two towns is an edge. So the problem boils down to this: pick a sequence of towns that gets the trailer from Nairobi to Kisumu with the least distance driven.

The trailer starts in Nairobi, and its only move is to drive from one town to a neighbouring town along the highway. Once it has driven from town x to town y, it is in y. We know we’re done when the trailer is in Kisumu. The cost of a path is the total distance in kilometres, and since every leg has a positive distance, adding more legs can never make a path cheaper.

Because the question says to use distance as the cost, we used Uniform-Cost Search (UCS). UCS always expands whichever town on the frontier has the smallest total distance from Nairobi so far, so it never settles for a longer route when a shorter one is available.

2. Distances between towns
All the roads run both ways, so each link costs the same in either direction. The figures are driving distances between town centres, rounded to the nearest kilometre.

Nairobi connects to three towns: Thika (42 km), Naivasha (92 km) and Narok (140 km). From Thika the road goes on to Nyeri (100 km), and from Nyeri to Nyahururu (70 km). Nyahururu links to Nakuru (65 km), and Nakuru can also be reached directly from Naivasha (63 km). From Nakuru, one road leads to Kericho (115 km) and another to Eldoret (155 km). Kericho and Eldoret both connect to Kisumu, at 80 km and 130 km respectively. Finally, Narok links to Kisii (190 km), and Kisii links to Kisumu (115 km).

3. Working through the search
We began at Nairobi with a cost of 0 and kept expanding the cheapest town on the frontier. One important detail is that we only checked for Kisumu when it became the cheapest thing left, not the first time we spotted it. As you’ll see, that makes a real difference here.

Expanding Nairobi (0) gave us Thika at 42, Naivasha at 92 and Narok at 140. Thika was the cheapest, so it went next, and it brought in Nyeri at 142. At that point the frontier held Naivasha (92), Narok (140) and Nyeri (142). Expanding Naivasha produced Nakuru at 155, which left the frontier as Narok (140), Nyeri (142) and Nakuru (155).

Next up was Narok (140), because its cost so far was lower than the others, even though it turns out to sit on a poor route to Kisumu. Expanding it added Kisii at 330. After that we expanded Nyeri (142), which brought in Nyahururu at 212, so the frontier became Nakuru (155), Nyahururu (212) and Kisii (330).

Then we expanded Nakuru (155), which generated Kericho at 270 and Eldoret at 310. It also offered a second way to reach Nyahururu, at 220 km. We already had Nyahururu at 212 km via Nyeri, so we kept the cheaper one and threw the 220 away. The frontier now read Nyahururu (212), Kericho (270), Eldoret (310) and Kisii (330).

Expanding Nyahururu (212) added nothing, since Nakuru had already been expanded at 155. Kericho (270) came next, and from it we saw Kisumu for the first time, at 350 km. We didn’t stop there, though. Eldoret (310) and Kisii (330) were both still cheaper than 350, so either of them could have led to a shorter route. Expanding Eldoret showed that Kisumu would cost 440 km that way, and expanding Kisii showed 445 km. Both were worse than 350, so we discarded them. That left Kisumu as the cheapest node on the frontier. We expanded it at 350 km, the goal test passed, and the search stopped.

4. Answer
The best route is Nairobi → Naivasha → Nakuru → Kericho → Kisumu, with a total distance of 92 + 63 + 115 + 80 = 350 km.

For comparison, here are the other routes we could have taken. Going through Nakuru and then Eldoret costs 92 + 63 + 155 + 130 = 440 km. The route through Narok and Kisii costs 140 + 190 + 115 = 445 km. The long way round through Thika, Nyeri, Nyahururu and Nakuru, then on to Kericho, costs 42 + 100 + 70 + 65 + 115 + 80 = 472 km. The same northern approach finishing via Eldoret costs 42 + 100 + 70 + 65 + 155 + 130 = 562 km.

The Kericho route beats every alternative by at least 90 km. It has four road segments, the same as the Eldoret route, but the Nakuru to Kericho and Kericho to Kisumu legs add up to far less than Nakuru to Eldoret and Eldoret to Kisumu. The Narok and Kisii route has the fewest segments (three), yet it comes out longer because the Narok to Kisii stretch alone is 190 km. So fewer towns doesn’t automatically mean a shorter drive.

5. Why we can trust this answer
There are two reasons UCS gives us the best path here. First, the graph is small and every leg has a positive distance, so the search is guaranteed to finish. Second, UCS expands towns strictly in order of cost. By the time Kisumu reached the front of the queue at 350 km, every cheaper partial route had already been explored, and none of them reached Kisumu for less. That makes 350 km the minimum on this graph.

One caveat is that the distances we used are approximate. If your Google Maps readings differ by a few kilometres, the numbers in the trace will shift slightly. But the Kericho route is far enough ahead that the answer should stay the same.

6. Conclusion
Using distance as the path cost, the best route for the trailer from Nairobi to Kisumu goes through Naivasha, Nakuru and Kericho, and covers about 350 km.