---
title: JAVA-알고리즘 Leetcode(Easy)-Remove Element
date: 2020-09-24 16:11:00 +0900
categories: [Algorithm,JAVA]
tags: [algorithm,java,알고리즘,자바,leetcode]
---

## Question
Given an array nums and a value val, remove all instances of that value [in-place](https://en.wikipedia.org/wiki/In-place_algorithm) and return the new length.
Do not allocate extra space for another array, you must do this by modifying the input array [in-place](https://en.wikipedia.org/wiki/In-place_algorithm) with O(1) extra memory.
The order of elements can be changed. It doesn't matter what you leave beyond the new length.

## 대충 해석
배열과 값을 각각 하나씩 줌. 주어진 배열에서 해당 값을 제외한 배열을 리턴하세요.

## Example
``` text
#1
Given nums = [3,2,2,3], val = 3,
Your function should return length = 2, with the first two elements of nums being 2.

It doesn't matter what you leave beyond the returned length.

#2
Given nums = [0,1,2,2,3,0,4,2], val = 2,
Your function should return length = 5, with the first five elements of nums containing 0, 1, 3, 0, and 4. Note that the order of those five elements can be arbitrary.

It doesn't matter what values are set beyond the returned length.
```

***

## Solution    
``` java
public class RemoveElement {

	public int removeElement(int[] nums, int val) {
		// LeetCode Easy 27
		int idx = 0;
		
		if(nums.length < 1) return idx;
		
		for(int i = 0; i < nums.length; i ++) {			
			int temp = nums[i];
			System.out.println(temp);
			if(temp != val) {
				nums[idx] = temp;
				idx ++;
			} else {
				continue;
			}
			
		}
		return idx;
	}
}
```