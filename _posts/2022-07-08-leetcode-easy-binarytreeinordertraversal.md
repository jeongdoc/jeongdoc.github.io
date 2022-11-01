---
title: JS-알고리즘 Leetcode(Easy)-Binary Tree Inorder Traversal
date: 2022-07-08 17:58:00 +0900
categories: [Algorithm,Javascript]
tags: [algorithm,알고리즘,javascript,leetcode]
---

## Question
Given the root of a binary tree, return the inorder traversal of its nodes' values.

## 대충 해석
주어진 이진트리를 중위순회한 값을 반환하세요.

## Example
``` text
#1
Input: root = [1,null,2,3]
Output: [1,3,2]

#2
Input: root = []
Output: []

#3
Input: root = [1]
Output: [1]
```

***

## Solution    
``` javascript
// 94. Binary Tree Inorder Traversal
/**
 * Definition for a binary tree node.
 * function TreeNode(val, left, right) {
 *     this.val = (val===undefined ? 0 : val)
 *     this.left = (left===undefined ? null : left)
 *     this.right = (right===undefined ? null : right)
 * }
 */
/**
 * @param {TreeNode} root
 * @return {number[]}
 */
 var inorderTraversal = function(root) {
    
    let result = [];
    const helper = function(nodes) {
        
        if(!nodes) {
            return result;
        }
        
        helper(nodes.left);
        result.push(nodes.val);
        helper(nodes.right);
        
        return nodes;
    }
    
    helper(root);
    
    return result;
};
```