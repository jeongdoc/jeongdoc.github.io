---
title: JS-알고리즘 Leetcode(Easy)-Best Time to Buy and Sell Stock
date: 2022-12-13 16:31:00 +0900
categories: [Algorithm,Javascript]
tags: [algorithm,알고리즘,javascript,leetcode]
---

### Question
You are given an array `prices` where `prices[i]` is the price of a given stock on the `i`th day.    
You want to maximize your profit by choosing a __single__ day to buy one stock and choosing a __different day in the future__ to sell that stock.    
Return _the maximum profit you can achieve from this transaction_. If you cannot achieve any profit, return `0`.

### 대충 해석
주어진 가격표에서 얻을 수 있는 최대 이익을 구하세요.

### Example
``` text
#1
Input: prices = [7,1,5,3,6,4]
Output: 5
Explanation: Buy on day 2 (price = 1) and sell on day 5 (price = 6), profit = 6-1 = 5.
Note that buying on day 2 and selling on day 1 is not allowed because you must buy before you sell.

#2
Input: prices = [7,6,4,3,1]
Output: 0
Explanation: In this case, no transactions are done and the max profit = 0.
```

***

### 고려해야할 부분
유의점으로는 구매가 무조건 먼저 일어나야한다는 부분 정도가 있다.

***

### Solution 1
1. 배열의 크기가 1인 경우는 이익을 낼 수 없으므로 바로 0을 리턴한다.    
2. 최소구매금액을 찾기 위해 0번째 값을 최소구매금액으로 초기화한다.    
3. 최소구매금액과 비교하여 더 작은 값을 최소구매금액으로 설정한다.    
4. 최소구매금액 - prices[i]를 이전 이익과 비교하여 더 작은 값을 찾는다. 구매가 무조건 우선이므로 계산값이 작을 수록 큰 이익이다.    

``` javascript

const compare = (a, b) => {
    return a < b ? a : b;
}

var maxProfit = function(prices) {
    
    prices = [7,1,5,3,6,4];
    if(prices.length < 2) return 0;

    let minBuy = prices[0];
    let profit = 0;
    for(let i = 1; i < prices.length; i++) {

        minBuy = compare(prices[i], minBuy);
        profit = compare(minBuy - prices[i], profit);

    }
    return -profit;
};
```