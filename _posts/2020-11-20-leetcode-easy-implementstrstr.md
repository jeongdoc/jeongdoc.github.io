---
title: JAVA-알고리즘 Leetcode(Easy)-ImplementStrStr
date: 2020-11-20 09:58:00 +0900
categories: [Algorithm,JAVA]
tags: [algorithm,java,알고리즘,자바,leetcode]
---

### Leetcode(Easy) ImplementStrStr

## Question
Return the index of the first occurrence of needle in haystack, or -1 if needle is not part of haystack.

## 대충 해석
haystac에 needle이 일부분으로 존재하면 index를 반환하고 없으면 -1 반환하세요

## Example
``` text
#1
Input: haystack = "hello", needle = "ll"
Output: 2

#2
Input: haystack = "aaaaa", needle = "bba"
Output: -1

#3
Input: haystack = "", needle = ""
Output: 0
```

***

## Solution    
``` java
public class ImplementStrStr {
	
	public int str(String haystack, String needle) {
		// LeetCode Easy 28
		char[] hay = haystack.toCharArray();
		char[] ned = needle.toCharArray();
		
		int hayLen = hay.length;
		int nedLen = ned.length;
		
		if(nedLen < 1) return 0;
		if(hayLen < nedLen) return -1;
		
		int nedIdx = 0;
		
		for(int hayIdx = 0; hayIdx < hayLen;) {
			if(ned[nedIdx] == hay[hayIdx]) {
				nedIdx ++;
			} else {
				hayIdx = hayIdx - nedIdx;
				nedIdx = 0;
			}
			hayIdx ++;
			
			if(nedIdx == nedLen) return hayIdx - nedIdx;
		}
	
		return -1;
		
		// 걍 indexOf 써서 풀었을 때 
		// haystack.indexOf(needle);
	}
}
```