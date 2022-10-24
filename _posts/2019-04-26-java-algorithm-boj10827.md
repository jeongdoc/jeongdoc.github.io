---
title: JAVA-알고리즘 BOJ10827
author: jeongdoc
date: 2019-04-26 17:13:00 +0900
categories: [Algorithm, JAVA]
tags: [algorithm, java, 알고리즘, 자바]
---

### 백준 10827    
## solution    
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