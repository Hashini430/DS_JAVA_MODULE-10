# Ex24 Shortest Path and Reachability in a Heritage Town using BFS
## AIM:
To design and implement a java program that, given a map of attractions in a heritage town connected by walking paths, recommends:
The shortest number of paths (minimum hops) from a starting attraction to a target attraction.
The number of reachable attractions from the same starting point using Breadth-First Search (BFS)


## Algorithm
1. Start the program.
2. Represent the heritage town map as a graph using a HashMap where each key is an attraction and its value is a list of connected attractions (walking paths).
3. Implement a BFS traversal to:
   - Visit all reachable attractions from a given starting attraction.
   - Count the total number of reachable attractions.
4. Implement another BFS-based shortest path algorithm that:
   - Uses a queue and a distance map.
   - Tracks the shortest number of hops (edges) from the start to the target attraction.
5. In the `main()` method:
   - Define the map (graph).
   - Take a starting and target attraction.
   - Display the reachable attractions and the shortest distance.
6. Stop the program.  

## Program:
```java
/*
Program to determine Shortest Path and Reachability in a Heritage Town using BFS
Developed by: Abinaya A
RegisterNumber: 212224230004

*/

import java.util.*;

public class TouristNavigation {
    
    public static int bfs(List<List<Integer>> graph, int start, int target, int n) {
        boolean[] visited = new boolean[n];
        int[] dist = new int[n];
        Queue<Integer> q = new LinkedList<>();
        q.offer(start);
        visited[start] = true;
        dist[start] = 0;

        while (!q.isEmpty()) {
            int curr = q.poll();
            if (curr == target) return dist[curr];
            for (int neigh : graph.get(curr)) {
                if (!visited[neigh]) {
                    visited[neigh] = true;
                    dist[neigh] = dist[curr] + 1;
                    q.offer(neigh);
                }
            }
        }
        return -1;
    }

    public static void dfs(List<List<Integer>> graph, boolean[] visited, int node) {
        visited[node] = true;
        for (int neighbor : graph.get(node)) {
            if (!visited[neighbor]) {
                dfs(graph, visited, neighbor);
            }
        }
    }

    public static int countReachable(boolean[] visited) {
        int count = 0;
        for (boolean v : visited) if (v) count++;
        return count;
    }

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int n = sc.nextInt(), e = sc.nextInt();
        List<List<Integer>> graph = new ArrayList<>();
        for (int i = 0; i < n; i++) graph.add(new ArrayList<>());

        for (int i = 0; i < e; i++) {
            int u = sc.nextInt(), v = sc.nextInt();
            graph.get(u).add(v);
            graph.get(v).add(u);
        }

        int start = sc.nextInt();
        int target = sc.nextInt();

        int shortest = bfs(graph, start, target, n);
        boolean[] visited = new boolean[n];
        dfs(graph, visited, start);
        int reachable = countReachable(visited);

        System.out.println("Shortest path from start to target: " + shortest);
        System.out.println("Total reachable attractions from start: " + reachable);
    }
}

```

## Output:
<img width="927" height="235" alt="image" src="https://github.com/user-attachments/assets/81ebe51d-83b2-4603-9cf9-edbf6113a8d7" />



## Result:
The program has been successfully implemented and executed.
It correctly computes:
The shortest number of paths (minimum hops) between two attractions.
The total number of reachable attractions from a given starting point using BFS traversal.
