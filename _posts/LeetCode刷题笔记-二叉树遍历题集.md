---
layout: post
title: "LeetCode 解题笔记"
subtitle: ''
author: "Liuyun"
header-style: text
tags:
  - Vim
---
LeetCode刷题笔记

二叉树遍历：

前序遍历：https://leetcode-cn.com/problems/binary-tree-preorder-traversal/

​        递归：

```java
class Solution {
    List<Integer> result = new ArrayList<>();
    public List<Integer> preorderTraversal(TreeNode root) {
        if(root == null){
            return result;
        }
        result.add(root.val);
        preorderTraversal(root.left);
        preorderTraversal(root.right);
        return result;
   }
}
```
​       迭代（深度优先遍历)：

```java
class Solution {
    public List<Integer> preorderTraversal(TreeNode root) {
        List<Integer> result = new ArrayList<>();
        if(root == null){
            return result;
        }
        LinkedList<TreeNode> stack = new LinkedList<>();
        stack.addFirst(root);
        while(!stack.isEmpty()){
            TreeNode cur = stack.pollFirst();
            result.add(cur.val);
            if(cur.right != null){
                stack.addFirst(cur.right);
            }
            if(cur.left != null){
                stack.addFirst(cur.left);
            }
        }
        return result;
    }
}
```

​     莫里斯遍历：

```java
class Solution {
    public List<Integer> preorderTraversal(TreeNode root) {
        List<Integer> result = new LinkedList<>();
        while(root != null){
            if(root.left == null){
                result.add(root.val);
                root = root.right;
            }
            else{
                TreeNode pre = root.left;
                while(pre.right != null && pre.right != root){
                    pre = pre.right;
                }
                if(pre.right == null){
                    result.add(root.val);
                    pre.right = root;
                    root = root.left;
                }
                else{
                    root = root.right;
                    pre.right = null;
                }
            }
        }
        return result;
    }
}
```

拓展-N叉树的前序遍历：https://leetcode-cn.com/problems/n-ary-tree-preorder-traversal/submissions/

```java
/*
// Definition for a Node.
class Node {
    public int val;
    public List<Node> children;

    public Node() {}

    public Node(int _val,List<Node> _children) {
        val = _val;
        children = _children;
    }
};
*/
class Solution {
    public List<Integer> preorder(Node root) {
        List<Integer> result = new LinkedList<>();
        if(root == null){
            return result;
        }
        LinkedList<Node> stack = new LinkedList<>();
        stack.addFirst(root);
        while(!stack.isEmpty()){
            Node cur = stack.pollFirst();
            result.add(cur.val);
            if(cur.children != null){
                //list.get(index) 或者下面注释中的Iterator
                for(int i = (cur.children.size() -1);i >= 0;i--){
                    stack.addFirst(cur.children.get(i));
                }
                // ListIterator<Node> iterator = cur.children.listIterator(cur.children.size());
                // while(iterator.hasPrevious()){
                //     stack.addFirst(iterator.previous());
                // }
            }
        }
        return result;
    }
}
```

中序遍历：https://leetcode-cn.com/problems/binary-tree-inorder-traversal/submissions/

​      递归：

```java
class Solution {
    List<Integer> result = new ArrayList<>();
    public List<Integer> preorderTraversal(TreeNode root) {
        if(root == null){
            return result;
        }
        preorderTraversal(root.left);
        result.add(root.val);
        preorderTraversal(root.right);
        return result;
   }
}
```

​    迭代：

```java
class Solution {
    public List<Integer> inorderTraversal(TreeNode root) {
        List<Integer> result = new LinkedList<>();
        if(root == null){
            return result;
        }
        LinkedList<TreeNode> queue = new LinkedList<>();
        while(root != null || !queue.isEmpty()){
            while(root != null){
                queue.addFirst(root);
                root = root.left;
            }
            root = queue.pollFirst();
            result.add(root.val);
            root = root.right;
        }
        return result;
    }
}
```

后序遍历：https://leetcode-cn.com/problems/binary-tree-postorder-traversal/

​     递归：

```java
class Solution {
    List<Integer> result = new ArrayList<>();
    public List<Integer> preorderTraversal(TreeNode root) {
        if(root == null){
            return result;
        }
        result.add(root.val);
        preorderTraversal(root.left);
        preorderTraversal(root.right);
        result.add(root.val);
        return result;
   }
}
```

​     迭代：

```java
class Solution {
    public List<Integer> postorderTraversal(TreeNode root) {
        LinkedList<TreeNode> stack = new LinkedList<>();
        LinkedList<Integer> output = new LinkedList<>();
        if(root == null){
            return output;
        }
        stack.addFirst(root);
        while(!stack.isEmpty()){
            TreeNode cur = stack.pollFirst();
            output.addFirst(cur.val);
            if(cur.left != null){
                stack.addFirst(cur.left);
            }
            if(cur.right != null){
                stack.addFirst(cur.right);
            }
        }
        return output;
    }
}
```

