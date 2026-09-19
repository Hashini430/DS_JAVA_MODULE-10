
# Ex21 Count the Number of Nodes in the Left Subtree of a Binary Tree
## AIM:
To design and implement a java program that constructs a binary tree from given level order input and counts the number of nodes present in the left subtree of the root node

## Algorithm
1. Start the program
2. Define a class Node to represent each node in the binary tree
3. Create a function insertLevelOrder(arr, root, i, n) to construct the binary tree from a given level order list.
4. Define a function countNodes(root) that returns the total number of nodes in a given subtree.
5.  Read or define the level order input array representing the binary tree.
6.  Construct the binary tree using the insertLevelOrder() function.
7.  Identify the left child of the root and call countNodes() on it.
8.  Display the number of nodes in the left subtree.
9.   Stop the program.

## Program:
```java
/*
Program to constructs a binary tree from given level order input and counts the number of nodes 
Developed by: Abinaya A
RegisterNumber: 212224230004

*/

import java.util.*;

class Node {
    int data;
    Node left, right;
    Node(int data) {
        this.data = data;
        this.left = this.right = null;
    }
}

public class Main {
    static Node buildTree(int[] arr) {
        if (arr.length == 0) return null;
        Node root = new Node(arr[0]);
        Queue<Node> q = new LinkedList<>();
        q.add(root);
        int i = 1;

        while (!q.isEmpty() && i < arr.length) {
            Node current = q.poll();
            if (i < arr.length) {
                current.left = new Node(arr[i++]);
                q.add(current.left);
            }
            if (i < arr.length) {
                current.right = new Node(arr[i++]);
                q.add(current.right);
            }
        }

        return root;
    }

    static int countNodes(Node root) {
        if (root == null) return 0;
        return 1 + countNodes(root.left) + countNodes(root.right);
    }

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int n = sc.nextInt();
        int[] arr = new int[n];
        for (int i = 0; i < n; i++) arr[i] = sc.nextInt();
        Node root = buildTree(arr);

        if (root.left == null) {
            System.out.println(0);
        } else {
            System.out.println(countNodes(root.left));
        }
    }
}

```

## Output:
<img width="321" height="115" alt="image" src="https://github.com/user-attachments/assets/3d88487b-6602-4003-883c-9e1c2e5aafc6" />

## Result:
The program has been successfully implemented and executed.
It correctly constructs the binary tree from level order input and counts the number of nodes in the left subtree of the root node.
