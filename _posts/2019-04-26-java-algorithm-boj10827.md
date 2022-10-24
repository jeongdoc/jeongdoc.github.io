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
import java.util.Arrays;
import java.util.Scanner;

public class BOJ10817 {

	public static void main(String[] args) {
		// boj10817
		Scanner s = new Scanner(System.in);
		int []iar = new int[3];
		for(int j = 0; j < iar.length; j ++) {
			iar[j] = s.nextInt();
		}
		s.close();
		Arrays.sort(iar);
		System.out.println(iar[1]);
	}

}
```