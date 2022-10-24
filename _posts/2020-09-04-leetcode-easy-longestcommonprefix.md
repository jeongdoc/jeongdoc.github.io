---
title: JAVA-알고리즘 Leetcode(Easy)-Longest Common Prefix
date: 2020-09-04 14:00:00 +0900
categories: [Algorithm,JAVA]
tags: [algorithm,java,알고리즘,자바,leetcode]
---

## Question
Write a function to find the longest common prefix string amongst an array of strings.

If there is no common prefix, return an empty string "".

## 대충 해석
문자열 배열 요소들의 공통된 접두어를 찾아보세요. 공통 접두어가 없다면 ""을 리턴하세요.

## Example
``` text
#1
Input: ["flower","flow","flight"]
Output: "fl"

#2
Input: ["dog","racecar","car"]
Output: ""
Explanation: There is no common prefix among the input strings.
```

***

## Solution    
``` java
public class LongestCommonPrefix {

	public static void main(String[] args) {
		// LeetCode Easy 14

		String[] s = {"apple", "application", "aplogise" , "api"};
		
		int len = s.length;
		
		String result = longestCommonPrefix(s, len);
		System.out.println(result);
		
	}
	
	public static String longestCommonPrefix(String[] s, int len) {
		String prefix = null;
		
		if(len == 0) {
			prefix = "";
		} else {
			prefix = s[0];
			for(int i = 0; i < len; i ++) {
				while(s[i].indexOf(prefix) != 0) {
					prefix = prefix.substring(0, prefix.length() -1);
					
					if(prefix.length() < 1) prefix = "";
				}
			}
		}
		
		return prefix;
	}

}
```