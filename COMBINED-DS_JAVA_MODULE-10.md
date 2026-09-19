# DS Java Module 10 – Merged Exercises

This document combines Exercises 21–25 from **DS_JAVA_MODULE-10**.

---

## Ex21 – Count the Nodes in the Left Subtree of a Binary Tree

### Aim
Construct a binary tree from level-order input and count the nodes in the left subtree of the root node.

### Algorithm
1. Read the number of nodes and their level-order values.
2. Construct the binary tree using a queue.
3. Recursively count the nodes in the root's left subtree.
4. Display the count.

### Program

```java
import java.util.LinkedList;
import java.util.Queue;
import java.util.Scanner;

public class Main {
    static class Node {
        int data;
        Node left;
        Node right;

        Node(int data) {
            this.data = data;
        }
    }

    static Node buildTree(int[] values) {
        if (values.length == 0) {
            return null;
        }

        Node root = new Node(values[0]);
        Queue<Node> queue = new LinkedList<>();
        queue.add(root);
        int index = 1;

        while (!queue.isEmpty() && index < values.length) {
            Node current = queue.poll();
            current.left = new Node(values[index++]);
            queue.add(current.left);

            if (index < values.length) {
                current.right = new Node(values[index++]);
                queue.add(current.right);
            }
        }

        return root;
    }

    static int countNodes(Node root) {
        if (root == null) {
            return 0;
        }
        return 1 + countNodes(root.left) + countNodes(root.right);
    }

    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int n = scanner.nextInt();
        int[] values = new int[n];

        for (int i = 0; i < n; i++) {
            values[i] = scanner.nextInt();
        }

        Node root = buildTree(values);
        System.out.println(root == null ? 0 : countNodes(root.left));
        scanner.close();
    }
}
```

### Result
The program constructs the binary tree and correctly counts the nodes in the left subtree of the root.

---

## Ex22 – Search for a Book ID in a Binary Search Tree

### Aim
Construct a Binary Search Tree using Book IDs and determine whether queried Book IDs exist in the tree.

### Algorithm
1. Read the Book IDs and insert them into a BST.
2. For each query, compare the value with the current node.
3. Search the left subtree for smaller values and the right subtree for larger values.
4. Display `Found` or `Not Found`.

### Program

```java
import java.util.Scanner;

public class BookIDSearch {
    static class Node {
        int data;
        Node left;
        Node right;

        Node(int data) {
            this.data = data;
        }
    }

    static Node insert(Node root, int key) {
        if (root == null) {
            return new Node(key);
        }

        if (key < root.data) {
            root.left = insert(root.left, key);
        } else {
            root.right = insert(root.right, key);
        }
        return root;
    }

    static boolean search(Node root, int key) {
        if (root == null) {
            return false;
        }
        if (root.data == key) {
            return true;
        }
        return key < root.data
                ? search(root.left, key)
                : search(root.right, key);
    }

    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int n = scanner.nextInt();
        Node root = null;

        for (int i = 0; i < n; i++) {
            root = insert(root, scanner.nextInt());
        }

        int queries = scanner.nextInt();
        while (queries-- > 0) {
            int key = scanner.nextInt();
            System.out.println(search(root, key) ? "Found" : "Not Found");
        }

        scanner.close();
    }
}
```

### Result
The program successfully constructs a BST and searches for the requested Book IDs.

---

## Ex23 – Breadth-First Search Traversal of a City Junction Map

### Aim
Perform BFS traversal on a graph representing city junctions and list all reachable locations from a source junction.

### Algorithm
1. Read the number of vertices and edges.
2. Store the graph using an adjacency list.
3. Add each undirected edge to the adjacency list.
4. Place the source vertex in a queue.
5. Visit each vertex and enqueue its unvisited neighbors.
6. Print the BFS traversal order.

### Program

```java
import java.util.ArrayList;
import java.util.LinkedList;
import java.util.List;
import java.util.Queue;
import java.util.Scanner;

public class EmergencyRouteBFS {
    static void addEdge(List<List<Integer>> graph, int first, int second) {
        graph.get(first).add(second);
        graph.get(second).add(first);
    }

    static void bfs(List<List<Integer>> graph, int source, boolean[] visited) {
        Queue<Integer> queue = new LinkedList<>();
        queue.offer(source);
        visited[source] = true;

        while (!queue.isEmpty()) {
            int current = queue.poll();
            System.out.print(current + " ");

            for (int neighbor : graph.get(current)) {
                if (!visited[neighbor]) {
                    visited[neighbor] = true;
                    queue.offer(neighbor);
                }
            }
        }
    }

    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int vertices = scanner.nextInt();
        int edges = scanner.nextInt();
        List<List<Integer>> graph = new ArrayList<>();

        for (int i = 0; i < vertices; i++) {
            graph.add(new ArrayList<>());
        }

        for (int i = 0; i < edges; i++) {
            addEdge(graph, scanner.nextInt(), scanner.nextInt());
        }

        int source = scanner.nextInt();
        bfs(graph, source, new boolean[vertices]);
        scanner.close();
    }
}
```

### Result
The program performs BFS traversal and lists all locations reachable from the source junction.

---

## Ex24 – Shortest Path and Reachability Using BFS

### Aim
Find the shortest path in number of hops between two attractions and count all attractions reachable from a starting point.

### Algorithm
1. Represent the map as an undirected graph.
2. Use BFS to calculate the shortest distance from the start to the target.
3. Use DFS to mark every reachable vertex.
4. Count the marked vertices.
5. Display the shortest path and reachable count.

### Program

```java
import java.util.ArrayList;
import java.util.LinkedList;
import java.util.List;
import java.util.Queue;
import java.util.Scanner;

public class TouristNavigation {
    static int bfs(List<List<Integer>> graph, int start, int target) {
        boolean[] visited = new boolean[graph.size()];
        int[] distance = new int[graph.size()];
        Queue<Integer> queue = new LinkedList<>();

        queue.offer(start);
        visited[start] = true;

        while (!queue.isEmpty()) {
            int current = queue.poll();
            if (current == target) {
                return distance[current];
            }

            for (int neighbor : graph.get(current)) {
                if (!visited[neighbor]) {
                    visited[neighbor] = true;
                    distance[neighbor] = distance[current] + 1;
                    queue.offer(neighbor);
                }
            }
        }

        return -1;
    }

    static void dfs(List<List<Integer>> graph, boolean[] visited, int node) {
        visited[node] = true;
        for (int neighbor : graph.get(node)) {
            if (!visited[neighbor]) {
                dfs(graph, visited, neighbor);
            }
        }
    }

    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int vertices = scanner.nextInt();
        int edges = scanner.nextInt();
        List<List<Integer>> graph = new ArrayList<>();

        for (int i = 0; i < vertices; i++) {
            graph.add(new ArrayList<>());
        }

        for (int i = 0; i < edges; i++) {
            int first = scanner.nextInt();
            int second = scanner.nextInt();
            graph.get(first).add(second);
            graph.get(second).add(first);
        }

        int start = scanner.nextInt();
        int target = scanner.nextInt();
        int shortest = bfs(graph, start, target);
        boolean[] visited = new boolean[vertices];
        dfs(graph, visited, start);

        int reachable = 0;
        for (boolean isVisited : visited) {
            if (isVisited) {
                reachable++;
            }
        }

        System.out.println("Shortest path from start to target: " + shortest);
        System.out.println("Total reachable attractions from start: " + reachable);
        scanner.close();
    }
}
```

### Result
The program correctly computes the minimum number of hops and the total number of reachable attractions.

---

## Ex25 – Find the Fastest Route to a Charging Station Using Dijkstra's Algorithm

### Aim
Find the shortest travel time from an electric vehicle's current location to the nearest charging station in a weighted graph.

### Algorithm
1. Represent the city map as a weighted graph.
2. Set the EV's current block as the source vertex.
3. Use Dijkstra's algorithm to compute the shortest distances.
4. Compare distances to all charging stations.
5. Display the minimum travel time, or `-1` if no station is reachable.

### Program

```java
import java.util.ArrayList;
import java.util.Arrays;
import java.util.Comparator;
import java.util.HashSet;
import java.util.List;
import java.util.PriorityQueue;
import java.util.Scanner;
import java.util.Set;

public class EVChargingNavigation {
    static class Pair {
        int node;
        int time;

        Pair(int node, int time) {
            this.node = node;
            this.time = time;
        }
    }

    static int findNearestChargingStation(
            int vertices,
            List<List<Pair>> graph,
            int source,
            Set<Integer> stations) {
        int[] distance = new int[vertices];
        Arrays.fill(distance, Integer.MAX_VALUE);
        distance[source] = 0;

        PriorityQueue<Pair> queue = new PriorityQueue<>(
                Comparator.comparingInt(pair -> pair.time));
        queue.offer(new Pair(source, 0));

        while (!queue.isEmpty()) {
            Pair current = queue.poll();
            if (current.time != distance[current.node]) {
                continue;
            }

            for (Pair neighbor : graph.get(current.node)) {
                int newDistance = current.time + neighbor.time;
                if (newDistance < distance[neighbor.node]) {
                    distance[neighbor.node] = newDistance;
                    queue.offer(new Pair(neighbor.node, newDistance));
                }
            }
        }

        int minimumTime = Integer.MAX_VALUE;
        for (int station : stations) {
            minimumTime = Math.min(minimumTime, distance[station]);
        }

        return minimumTime == Integer.MAX_VALUE ? -1 : minimumTime;
    }

    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int vertices = scanner.nextInt();
        int edges = scanner.nextInt();
        List<List<Pair>> graph = new ArrayList<>();

        for (int i = 0; i < vertices; i++) {
            graph.add(new ArrayList<>());
        }

        for (int i = 0; i < edges; i++) {
            int first = scanner.nextInt();
            int second = scanner.nextInt();
            int time = scanner.nextInt();
            graph.get(first).add(new Pair(second, time));
            graph.get(second).add(new Pair(first, time));
        }

        int source = scanner.nextInt();
        int stationCount = scanner.nextInt();
        Set<Integer> stations = new HashSet<>();

        for (int i = 0; i < stationCount; i++) {
            stations.add(scanner.nextInt());
        }

        System.out.println(findNearestChargingStation(
                vertices, graph, source, stations));
        scanner.close();
    }
}
```

### Result
The program uses Dijkstra's algorithm to determine the shortest travel time to the nearest reachable charging station.

---

## Summary

| Exercise | Topic |
|---|---|
| Ex21 | Left subtree node count |
| Ex22 | Binary Search Tree search |
| Ex23 | Breadth-First Search traversal |
| Ex24 | Shortest path and reachability |
| Ex25 | Dijkstra's shortest path algorithm |
