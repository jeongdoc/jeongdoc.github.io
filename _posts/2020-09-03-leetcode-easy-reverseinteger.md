---
title: JAVA-알고리즘 Leetcode(Easy)-Reverse Integer
author: jeongdoc
date: 2020-09-03 12:15:00 +0900
categories: [Algorithm,JAVA]
tags: [algorithm,java,알고리즘,자바,leetcode]
---

## Question
Given a 32-bit signed integer, reverse digits of an integer.

## 대충 해석
주어빈 32비트 int를 뒤집어보세요

## Example
``` text
#1
Input:123
Output: 321

#2
Input: -123
Output: -321

#3
Input: 120
Output: 21
```

***

## Solution    
``` java
public class ReverseInteger {

	public static void main(String[] args) {
		// LeetCode Easy 7
		int input = 1534236469;
		long rev = 0;
		
		while(input != 0) {
			long temp =(rev * 10) + (input % 10);
			
			rev = (temp - (input % 10)) / 10 != rev ? 0 : temp;
			
			input /= 10;
		}
		
		System.out.println(rev);
	}
}
```