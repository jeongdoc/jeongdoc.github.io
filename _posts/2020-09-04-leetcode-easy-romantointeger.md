---
title: JAVA-알고리즘 Leetcode(Easy)-Roman to Integer
date: 2020-09-04 13:00:00 +0900
categories: [Algorithm,JAVA]
tags: [algorithm,java,알고리즘,자바,leetcode]
---

### Leetcode(Easy) Roman to Integer

## Question
Roman numerals are represented by seven different symbols: I, V, X, L, C, D and M.

Symbol - value

I - 1

V - 5

X - 10

L - 50

C - 100

D - 500

M - 1000

For example, two is written as II in Roman numeral, just two one's added together. Twelve is written as, XII, which is simply X + II. The number twenty seven is written as XXVII, which is XX + V + II.

Roman numerals are usually written largest to smallest from left to right. However, the numeral for four is not IIII. Instead, the number four is written as IV. Because the one is before the five we subtract it making four. The same principle applies to the number nine, which is written as IX. There are six instances where subtraction is used:

    I can be placed before V (5) and X (10) to make 4 and 9. 
    X can be placed before L (50) and C (100) to make 40 and 90. 
    C can be placed before D (500) and M (1000) to make 400 and 900.

Given a roman numeral, convert it to an integer. Input is guaranteed to be within the range from 1 to 3999.

## 대충 해석
문자열로 주는 로마숫자를 int 숫자로 바꿔보세요.

## Example
``` text
#1
Input: "III"
Output: 3

#2
Input: "IV"
Output: 4

#3
Input: "MCMXCIV"
Output: 1994
Explanation: M = 1000, CM = 900, XC = 90 and IV = 4.
```

***

## Solution    
``` java

public class RomanToInteger {

	public static void main(String[] args) {
		// LeetCode Easy 13
		
		String s = "MCMXCIV";
		int result = romanToInteger(s);
		System.out.println(result);
		
	}
	
	public static int romanToInteger(String s) {
		int result = 0;
		int len = s.length();
		
		for(int i = 0; i < len -1; i ++) {
			int now = switchRoman(s.charAt(i));
			int next = switchRoman(s.charAt(i +1));
			
			result = now < next ? result - now : result + now;
		}
		
		result += switchRoman(s.charAt(len -1));
		
		return result;
	}
	
	public static int switchRoman(char c) {
		int result = 0;
		
		switch(c) {
			case 'I' : result = 1; break;
			case 'V' : result = 5; break;
			case 'X' : result = 10; break;
			case 'L' : result = 50; break;
			case 'C' : result = 100; break;
			case 'D' : result = 500; break;
			case 'M' : result = 1000; break;
		}
		
		return result;
	}
}
```