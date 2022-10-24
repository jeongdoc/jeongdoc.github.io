---
title: JAVA-알고리즘 Leetcode(Easy)-Two Sum
author: jeongdoc
date: 2020-09-03 12:00:00 +0900
categories: [Algorithm,JAVA]
tags: [algorithm,java,알고리즘,자바,leetcode]
---

## Leetcode(Easy) 1

## Question
Given an array of integers nums and an integer target, return indices of the two numbers such that they add up to target.
You may assume that each input would have exactly one solution, and you may not use the same element twice.
You can return the answer in any order.

## 대충 해석
int 배열과 int 타겟 하나 줌. 배열 요소 두 개의 합이 타겟값과 일치하는 요소찾기. 찾은 후 요소의 인덱스 배열 리턴.

## Example
``` text
#1
Input: nums = [2,7,11,15], target = 9
Output: [0,1]
Output: Because nums[0] + nums[1] == 9, we return [0, 1].

#2
Input: nums = [3,2,4], target = 6
Output: [1,2]
```

***

## solution    
``` java
import java.util.Arrays;
import java.util.Comparator;
import java.util.HashMap;
import java.util.Map;

public class TwoSum {

	public static void main(String[] args) {
		// LeetCode Easy 1
		
		int[] nums = {2, 11, 7, 15};
		int target = 9;
		
		Map<Integer, Integer> map = new HashMap<>();
		int[] temp = new int[] {0, 0};
		for(int i = 0; i < nums.length; i ++) {
			map.put(nums[i], i);
		}
		
		for(int i = 0; i < nums.length; i ++) {
			int result = target - nums[i];
			
			if(map.containsKey(result)) {
				
				int answer = map.get(result);
				if(answer == i) continue;
				
				temp[0] = i;
				temp[1] = answer;
				System.out.println(temp[0] + ", " + temp[1]);
				break;
			}
		}
	}
}
```