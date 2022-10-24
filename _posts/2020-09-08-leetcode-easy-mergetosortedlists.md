---
title: JAVA-알고리즘 Leetcode(Easy)-Merge Two Sorted Lists
date: 2020-09-08 11:00:00 +0900
categories: [Algorithm,JAVA]
tags: [algorithm,java,알고리즘,자바,leetcode]
---

### Leetcode(Easy) Merge Two Sorted Lists

## Question
Merge two sorted linked lists and return it as a new sorted list. 
The new list should be made by splicing together the nodes of the first two lists.

## 대충 해석
주어진 두 개의 리스트를 하나의 새로운 리스트로 만들어보세요.

## Example
``` text
Input: 1->2->4, 1->3->4
Output: 1->1->2->3->4->4
```

***

## Solution    
``` java
public class MergeTwoSortedLists {
	
	int val;
	MergeTwoSortedLists next;
	MergeTwoSortedLists() {}
	MergeTwoSortedLists(int val) { this.val = val; }
	MergeTwoSortedLists(int val, MergeTwoSortedLists next) { this.val = val; this.next = next; }

	public static void main(String[] args) {
		// LeetCode Easy 21
		MergeTwoSortedLists l1 = null;
		MergeTwoSortedLists list1 = new MergeTwoSortedLists(1);		
		l1 = list1;
		list1.next = new MergeTwoSortedLists(3);
		list1 = list1.next;
		list1.next = new MergeTwoSortedLists(4);
		
		
		MergeTwoSortedLists l2 = null;
		MergeTwoSortedLists list2 = new MergeTwoSortedLists(2);;
		l2 = list2;
		list2.next = new MergeTwoSortedLists(6);
		list2 = list2.next;
		list2.next = new MergeTwoSortedLists(7);
		
		System.out.println(listNode(l1, l2));
	}
	
	public static MergeTwoSortedLists listNode(MergeTwoSortedLists l1, MergeTwoSortedLists l2) {
		MergeTwoSortedLists result = new MergeTwoSortedLists(0);
		MergeTwoSortedLists temp = result;
		
		
		while(l1 != null && l2 != null) {
			if(l1.val < l2.val) {
				temp.next = l1;
				l1 = l1.next;
			} else {
				temp.next = l2;
				l2 = l2.next;
			}
			
			temp = temp.next;
		}
		
		temp.next = l1 != null ? l1 : l2;
		
		return result.next;
	}
}
```