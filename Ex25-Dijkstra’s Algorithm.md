# Ex25 Finding the Fastest Route to a Charging Station using Dijkstra’s Algorithm
## AIM:
To design and implement a java program that helps an electric vehicle (EV) find the shortest travel time from its current block to the nearest charging station using Dijkstra’s shortest path algorithm.
## Algorithm
1. Start the program
2. Represent the city map as a weighted graph, where nodes represent blocks and edges represent travel times.
3. Take the current block of the EV as the source node
4. Store the locations of all charging stations
5. Use Dijkstra’s algorithm to compute the shortest distance from the source node to all other nodes in the graph.
6. Compare distances to find the nearest charging station with the minimum travel time
7. Display the shortest path and the total travel time
8. If no charging station is reachable, display an appropriate message
9. Stop the program   

## Program:
```java
/*
Program to find the Fastest Route to a Charging Station using Dijkstra’s Algorithm
Developed by: Abinaya A
RegisterNumber: 212224230004

*/

import java.util.*;

public class EVChargingNavigation {

    static class Pair {
        int node, time;
        Pair(int node, int time) {
            this.node = node;
            this.time = time;
        }
    }

    static int findNearestChargingStation(int n, List<List<Pair>> graph, int source, Set<Integer> stations) {
        int[] dist = new int[n];
        Arrays.fill(dist, Integer.MAX_VALUE);
        dist[source] = 0;

        PriorityQueue<Pair> pq = new PriorityQueue<>(Comparator.comparingInt(a -> a.time));
        pq.offer(new Pair(source, 0));

        while (!pq.isEmpty()) {
            Pair current = pq.poll();
            for (Pair neighbor : graph.get(current.node)) {
                int newDist = dist[current.node] + neighbor.time;
                if (newDist < dist[neighbor.node]) {
                    dist[neighbor.node] = newDist;
                    pq.offer(new Pair(neighbor.node, newDist));
                }
            }
        }

        int minTime = Integer.MAX_VALUE;
        for (int s : stations) {
            if (dist[s] < minTime) minTime = dist[s];
        }
        return minTime == Integer.MAX_VALUE ? -1 : minTime;
    }

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

        int n = sc.nextInt(), m = sc.nextInt();
        List<List<Pair>> graph = new ArrayList<>();
        for (int i = 0; i < n; i++) graph.add(new ArrayList<>());

        for (int i = 0; i < m; i++) {
            int u = sc.nextInt(), v = sc.nextInt(), w = sc.nextInt();
            graph.get(u).add(new Pair(v, w));
            graph.get(v).add(new Pair(u, w)); // Undirected
        }

        int source = sc.nextInt();
        int k = sc.nextInt();
        Set<Integer> stations = new HashSet<>();
        for (int i = 0; i < k; i++) stations.add(sc.nextInt());

        System.out.println(findNearestChargingStation(n, graph, source, stations));
    }
}

```

## Output:
<img width="304" height="310" alt="image" src="https://github.com/user-attachments/assets/e58d31e7-55ff-42fc-8c87-e1b71a6fbd5c" />



## Result:
The program has been successfully implemented and executed.
It uses Dijkstra’s algorithm to determine the shortest travel time from the EV’s current location to the nearest charging station and correctly handles cases where no station is reachable.
