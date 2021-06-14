---
layout: post
title: [Algorithms] Binary tree - Level order traversal II
description: [Algorithms] Binary tree - Level order traversal II
keywords: algorithms, leetcode
---

### Problem

https://leetcode.com/problems/binary-tree-level-order-traversal-ii/

### DFS

With `DFS` approach, we will recursively traverse the Binary Tree. For each node we traversal, we check if the List<Integer> of its level is already present or not. 
- If not, create a new one and push the node value into.
- If yes, just push the node value

After traversed all nodes in the tree, we just need to `reverse` the `List<List<Integer>>` we have by `Collections.reverse` method.

```java
class Solution {
    public List<List<Integer>> levelOrderBottom(TreeNode root) {
        List<List<Integer>> ret = new ArrayList<>();

        dfs(ret, 0, root);

        Collections.reverse(ret);

        return ret;
    }

    public void dfs(List<List<Integer>> ret, int level, TreeNode root) {
        if (root == null)
            return;

        if (ret.size() == level) {
            List<Integer> newList = new ArrayList<>();
            newList.add(root.val);
            ret.add(level, newList);
        } else {
            List<Integer> existingList = ret.get(level);
            existingList.add(root.val);
            ret.set(level, existingList);
        }

        dfs(ret, level + 1, root.left);
        dfs(ret, level + 1, root.right);
    }
}
```

#### Submission detail

> Your runtime breaks 100% java submssion.

### BFS

In this approach, we will use a `Stack` to save nodes in the current level, for each level, we just pop them all and push the node value into a an array.

```java
class Solution {
    public List<List<Integer>> levelOrderBottom(TreeNode root) {
        List<List<Integer>> ret = new ArrayList<>();

        bfs(ret, root);

        Collections.reverse(ret);

        return ret;
    }

    public void bfs(List<List<Integer>> ret, TreeNode root) {
        if (root == null) return;

        Stack<List<TreeNode>> nodes = new Stack<>();
        List<TreeNode> initial = new ArrayList<>();
        initial.add(root);
        nodes.push(initial);

        while (!nodes.isEmpty()) {
            List<TreeNode> traversing = nodes.pop();
            List<TreeNode> next = new ArrayList<>();
            List<Integer> arr = new ArrayList<>();

            for (TreeNode node : traversing) {
                arr.add(node.val);
                if (node.left != null) {
                    next.add(node.left);
                }
                if (node.right != null) {
                    next.add(node.right);
                }
            }
            ret.add(arr);
            if (next.isEmpty()) break;
            nodes.push(next);
        }
    }
}
```

#### Submission detail

> Runtime: 1 ms, faster than 85.55% of Java online submissions for Binary Tree Level Order Traversal II.

### Sketch

