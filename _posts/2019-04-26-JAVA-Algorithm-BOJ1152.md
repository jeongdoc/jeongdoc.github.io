---
title: \[JAVA-알고리즘\] BOJ1152
author: cotes
date: 2019-04-26 17:00:00 +0900
categories: [Algorithm, JAVA]
tags: [algorithm, java, 알고리즘, 자바]
pin: true
---

### 백준 1152

``` java
package bojEX;

import java.util.Scanner;
import java.util.StringTokenizer;

public class BOJ1152 {

	public static void main(String[] args) {
		// boj1152
		String sentence = new Scanner(System.in).nextLine().trim();
		StringTokenizer st = new StringTokenizer(sentence, " ");
		System.out.println(st.countTokens());

	}

}
```