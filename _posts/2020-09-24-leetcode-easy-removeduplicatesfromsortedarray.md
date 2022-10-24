---
title: JAVA-알고리즘 Leetcode(Easy)-Remove Duplicates from Sorted Array
date: 2020-09-24 13:11:00 +0900
categories: [Algorithm,JAVA]
tags: [algorithm,java,알고리즘,자바,leetcode]
---

### Leetcode(Easy) Remove Duplicates from Sorted Array

## Question
Given a sorted array nums, remove the duplicates in-place such that each element appear only once and return the new length.
Do not allocate extra space for another array, you must do this by modifying the input array in-place with O(1) extra memory.

## 대충 해석
주어진 배열에서 중복되는 요소를 제거하세요.

## Example
``` text
#1
Given nums = [1,1,2],
Your function should return length = 2, with the first two elements of nums being 1 and 2 respectively.

It doesn't matter what you leave beyond the returned length.

#2
Given nums = [0,0,1,1,1,2,2,3,3,4],
Your function should return length = 5, with the first five elements of nums being modified to 0, 1, 2, 3, and 4 respectively.

It doesn't matter what values are set beyond the returned length.
```

***

## Solution    
``` java
public class RemoveDuplicatesFromSortedArray {

	public static void main(String[] args) {
		// LeetCode Easy 26
		int[] nums = {0, 0, 1, 1, 3, 4, 6, 6};
		int result = removeDuplicates(nums);
		
		System.out.println(result);
	}
	
	public static int removeDuplicates(int[] nums) {
		int result = 0;
		int len = nums.length;
		
		int cnt = 0;
		for(int i = 0; i < len -1; i ++) {
			
			if(nums[i] != nums[i +1]) {
				cnt += 1;
				nums[cnt] = nums[i +1];
			} 
		}
		result = cnt +1;
		return result;
	}
}
```