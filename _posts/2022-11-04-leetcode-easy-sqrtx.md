---
title: JAVA-알고리즘 Leetcode(Easy)-Sqrt(X)
date: 2022-11-04 16:30:00 +0900
categories: [Algorithm,JAVA]
tags: [algorithm,java,알고리즘,자바,leetcode]
---

## Question
Given a non-negative integer x, return the square root of `x` rounded down to the nearest integer. The returned integer should be non-negative as well. You must not use any built-in exponent function or operator.    

* For example, do not use `pow(x, 0.5)` in c++ or `x ** 0.5` in python.


## 대충 해석
내장 함수를 사용하지 않고, 주어진 음이 아닌 정수 x의 제곱근을 리턴하세요. 가장 가까운 정수로 버림합니다.

## Example
``` text
#1
Input: x = 4
Output: 2
Explanation: The square root of 4 is 2, so we return 2.

#2
Input: x = 8
Output: 2
Explanation: The square root of 8 is 2.82842..., and since we round it down to the nearest integer, 2 is returned.
```

***

## 고려해야할 부분
Example을 보면 아 소숫점 버림하라는 거구나 대충 감을 잡을 수 있긴 하다.     
주의할 점은 `round down`은 버림의 의미라는 것.     
int를 제곱해도 long 범위를 벗어나지 않음.     
x가 2 이상일 경우 제곱근의 버림이 x/2보다 작을 수 밖에 없다.    

대개 이런 류의 문제는 Binary search를 이용하긴 한다.

## Solution    
``` java
public class SqrtX {
	
	public int sqrt(int x) {
		// leetCode Easy 69
		if(x < 2) return x;
		
		long low = 1;
		long high = x;
		
		while(low < high) {
			long middle = 0;
			middle = low + (high - low) /2;
			
			if(middle * middle > x) {
				high = middle;
			} else {
				low = middle + 1;
			}
		}
		
		int result = (int)low - 1;
		
		return result;
	}

}
```

뉴턴법을 적용한 풀이가 있다. 참고하면 도움이 많이 될 것 같다.     
배움엔 끝이 없지...    
[Leetcode 다른 풀이 - Newton's method](https://leetcode.com/problems/sqrtx/discuss/1862093/Java-0ms-Newton's-method-commented)
{: .prompt-tip }