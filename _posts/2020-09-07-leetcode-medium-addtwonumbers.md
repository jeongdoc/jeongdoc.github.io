---
title: JAVA-알고리즘 Leetcode(Medium)-add Two Numbers
date: 2020-09-07 19:23:00 +0900
categories: [Algorithm,JAVA]
tags: [algorithm,알고리즘,java,leetcode]
---

## Question
You are given two non-empty linked lists representing two non-negative integers. The digits are stored in reverse order, and each of their nodes contains a single digit.   Add the two numbers and return the sum as a linked list.

You may assume the two numbers do not contain any leading zero, except the number 0 itself.

## 대충 해석
음이 아닌 정수를 가지고 있는 두 LinkedList가 주어지며 이들의 합을 리턴하세요.
각 자리의 수가 역순으로 되어있으며 각 노드는 1의 자리 수입니다.

## Example
``` text
#1
Input: l1 = [2,4,3], l2 = [5,6,4]
Output: [7,0,8]
Explanation: 342 + 465 = 807.

#2
Input: l1 = [0], l2 = [0]
Output: [0]

#3
Input: l1 = [9,9,9,9,9,9,9], l2 = [9,9,9,9]
Output: [8,9,9,9,0,0,0,1]
```

***

## 고려해야할 부분
l1과 l2의 길이가 다를 수 있다는 점을 고려, null체크만 유의한다면 수월하게 풀 수 있다.    
Iteration으로 문제에 접근하였는데 Leetcode의 다른 사람들 풀이도 참고해보니 Recursion으로도 접근할 수 있었다.    
Recursion을 선호하지 않아서 미처 고려하지 못했던 접근법이었다.    
***

## Solution 1 
``` java
// 2. add Two Numbers
public class AddTwoNumbers {
	
	int val;
	AddTwoNumbers next;
	AddTwoNumbers() {}
	AddTwoNumbers(int val) {this.val = val;}
	AddTwoNumbers(int val, AddTwoNumbers next) {this.val = val; this.next = next;}

	public static void main(String[] args) {
		// LeetCode Medieum
		AddTwoNumbers l1 = null;
		AddTwoNumbers list1 = new AddTwoNumbers(9);
		l1 = list1;
		
		AddTwoNumbers l2 = null;
		AddTwoNumbers list2 = new AddTwoNumbers(1);
		l2 = list2;
		list2.next = new AddTwoNumbers(9);
		list2 = list2.next;
		list2.next = new AddTwoNumbers(9);
		list2 = list2.next;
		list2.next = new AddTwoNumbers(9);
		
		
		AddTwoNumbers result = addTwoNumbers(l1, l2);
		
		while(result != null) {
			System.out.println(result.val);
			result = result.next;
		}

	}
	
	public static AddTwoNumbers addTwoNumbers(AddTwoNumbers l1, AddTwoNumbers l2) {
		
		// 1 10 100 1000 10000
		AddTwoNumbers answerNode = new AddTwoNumbers(0);
		AddTwoNumbers temp = answerNode;
		
		int nodeVal = 0;
		int sum = 0;
		
		while(l1 != null || l2 != null) {
			if(l1 != null) {
				sum += l1.val;
				l1 = l1.next;
			}
			if(l2 != null) {
				sum += l2.val;
				l2 = l2.next;
			}
			
			nodeVal = sum %10;
			
			temp.next = new AddTwoNumbers(nodeVal);
			temp = temp.next;
			
			sum /= 10;
			
		}
		if(sum != 0) temp.next = new AddTwoNumbers(sum);
		
		return answerNode.next;
	}

}
```