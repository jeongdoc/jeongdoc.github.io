---
title: JAVA-알고리즘 Leetcode(Easy)-Valid Parentheses
date: 2020-09-04 17:00:00 +0900
categories: [Algorithm,JAVA]
tags: [algorithm,java,알고리즘,자바,leetcode]
---

### Leetcode(Easy) Valid Parentheses

## Question
Given a string s containing just the characters '(', ')', '{', '}', '[' and ']', determine if the input string is valid.

An input string is valid if:

    1. Open brackets must be closed by the same type of brackets.
    2. Open brackets must be closed in the correct order.

## 대충 해석
괄호가 쌍으로 잘 열고 닫혀있는지 판단해보세요.
한 쪽만 있거나 열고 닫고의 괄호 짝이 맞지 않으면 안 됩니다.

## Example
``` text
#1
Input: s = "()"
Output: true

#2
Input: s = "([)]"
Output: false

#3
Input: s = "(]"
Output: false

#4
Input: s = "{[]}"
Output: true
```

***

## Solution    
``` java
import java.util.Stack;

public class ValidParentheses {

	public static void main(String[] args) {
		// LeetCode Easy 20

		String s ="())";
		int len = s.length();
		boolean flag = isValidParentheses(s, len);
		System.out.println(flag);
		
	}
	
	public static boolean isValidParentheses(String s, int len) {
		boolean flag = false;
		System.out.println("string => " + s);
		if(len % 2 == 0) {
			Stack<Character> stack  = new Stack<>();
			for(int i = 0; i < len; i ++) {
				char c = s.charAt(i);
				if(isLeft(c) > 0) {
					stack.push(c);
				} else {
					if(!stack.empty()) {
						char pop = stack.pop();
						
						if(isLeft(pop) != isRight(c)) {
							return false;
						}
						
					} else {
						return false;
					}
				}
				
			}
			flag = stack.isEmpty();
			
		}
		
		return flag;
	}
	
	public static int isLeft(char c) {
		int isValid = 0;
		
		switch(c) {
			case '(' : isValid = 1; break;
			case '{' : isValid = 2; break;
			case '[' : isValid = 3;	break;
		}
		return isValid;
	}
	
	public static int isRight(char c) {
		int isValid = 0;
		
		switch(c) {
		case ')' : isValid = 1; break;
		case '}' : isValid = 2; break;
		case ']' : isValid = 3;	break;
	}
		
		return isValid;
	}
}
```