---
title: JS-알고리즘 Leetcode(Easy)-Excel Sheet Column Title
date: 2022-11-14 12:43:00 +0900
categories: [Algorithm,Javascript]
tags: [algorithm,알고리즘,javascript,leetcode]
---

### Question
Given an integer `columnNumber`, return its corresponding column title as it appears in an Excel sheet.

### 대충 해석
정수인 colunmNumber에 따라 해당하는 엑셀 시트의 컬럼 타이틀을 리턴하세요.

### Example
``` text
#Sample
A -> 1
B -> 2
C -> 3
...
Z -> 26
AA -> 27
AB -> 28 
...

#1
Input: columnNumber = 1
Output: "A"

#2
Input: columnNumber = 28
Output: "AB"

#3
Input: columnNumber = 701
Output: "ZY"
```

***

### 고려해야할 부분
제약조건이 정말 중요하다. columnNumber의 범위를  `1 <= columnNumber <= 231 - 1` 로 제한하는데 0이 아닌 1부터 주어진다는 점에 유의해야한다.    
사실은 `base-10` 표기를 이용한 `base-26`이였던 것..! 정도로 이해하면 좋다.    
보통 문제를 풀 때 시간을 정해두고 푸는데 급하게 푼 나머지 영 개판이라 이후 풀이를 고치며 정리한 생각을 아래에 작성했다.

예시를 보면 A~Z까지 26개에 1~26의 숫자를 할당하고 있다. 이를 예시로 주어진 것보다 조금 더 그려보자.    

|:---:|:---:|:---:|:---:|
| |A|B|...|
|0|1|2|...|
|A|27|53|...|
|B|28|54|...|
|C|29|55|...|
|D|30|56|...|
|E|31|57|...|
|...|...|...|...|
|Y|51|77|...|
|Z|52|78|...|

적당히 그려보고 나면 규칙을 바로 찾을 수 있을 것이다.    

|:---:|:---:|
|BA|2 * 26 __+ 1__|
|BB|2 * 26 __+ 2__|
|...|...|
|BY|2 * 26 __+ 25__|
|BZ|2 * 26 __+ 26__|

이를 참고해서 AAZ를 식으로 표현해보자면 아래와 같이 대강 식을 세울 수 있다.    
`n = (A * 26^2) + (A * 26^1) + (Z * 26^0) __=>__ n = (1 * 26^2) + (1 * 26^1) + (26 * 26^0)`    
처음 풀이에선 이 식을 그대로 사용하고 나머지가 0일 경우를 따로 처리했었는데..(라고 쓰고 바보라고 읽음)    
고민을 하다보니 어차피 나머지 이용할거 굳이 그렇게 할 필요가 없다는 걸 깨달았다.    
(ZZZ＝Z×26^2＋Z×26^1＋Z＝26×26^2＋26×26^1＋26^0 같은 경우 위처럼하면 좀 귀찮음)    
1~26을 그대로 사용하는 것이 아니라 0~25로 이동시킨 후 나머지를 이용하면 보다 간단하게 처리할 수 있다.    

맨 처음 풀이가 Solution 1이며 그 후 정리한 풀이가 Solution 2이다.    

***

### Solution 1

``` javascript
var convertToTitle = function(columnNumber) {
    // 1회차 풀이
    columnNumber = 52;

    let remain = 0;
    const charcode = [];

    while(columnNumber > 0) {

        remain = columnNumber % 26;

        if(remain === 0) {

            let checkCode = columnNumber + 64;
            if(checkCode > 97)  {
                checkCode = (columnNumber / 26) + 63;
                charcode.push(90);
            }
            charcode.unshift(checkCode);

            break;
        }

        charcode.unshift(remain+64);
        columnNumber = (columnNumber - remain) / 26;

    }

    console.log(String.fromCharCode(...charcode));

};
```

### Solution 2

``` javascript
var convertToTitle2 = function(columnNumber) {
    // 1회차 풀이 코드 정리
    columnNumber = 52;

    let remain = 0;
    const charcode = [];

    while(columnNumber > 0) {

        remain = (columnNumber -= 1) % 26;
        charcode.unshift(remain + 65);
        columnNumber = (columnNumber - remain) / 26;

    }

    console.log(String.fromCharCode(...charcode));

};
```