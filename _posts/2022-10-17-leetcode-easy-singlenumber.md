---
title: TS-알고리즘 Leetcode(Easy)-Single Number
date: 2022-10-17 16:56:00 +0900
categories: [Algorithm,Typescript]
tags: [algorithm,알고리즘,typescript,leetcode]
---

## Question
Given a __non-empty__ array of integers `nums`, every element appears twice except for one. Find that single one.
You must implement a solution with a linear runtime complexity and use only constant extra space.

## 대충 해석
주어진 비어있지 않은 배열 정수 배열에서 쌍이 아닌 정수를 찾으세요.

## Example
``` text
#1
Input: nums = [2,2,1]
Output: 1

#2
Input: nums = [4,1,2,1,2]
Output: 4

#3
Input: nums = [1]
Output: 1
```

***
## 고려해야할 부분
문제를 대충 읽으면 놓치기 쉬운 부분이 있다. 문제에도 명시되어있는 조건인데,
1. Time complexity : Linear - O(n)
2. Space complexity : Constant -  O(1)    

***

## Solution 1 - Map 
- Time complexity : O(n)
- Space complexity : O(n)    

``` javascript
// 136. single number

function SingleNumber(nums: number[]): void {
    // first solution
    // let checkDuple: Map<number, number> = new Map<number, number>();

    // nums.forEach((i, idx) => {
    //     if(checkDuple.get(i)) {
    //         let temp: number = checkDuple.get(nums[i])!;
    //         checkDuple.set(i, temp+1);
    //     } else {
    //         checkDuple.set(i, 1);
    //     }
    // });

    const checkDuple: Map<number, number> = nums.reduce((prev, curr, idx)=> {
        prev.set(curr, (prev.get(curr) || 0) +1);
        return prev;
    }, new Map<number, number>());

    let singleOne: number = 0;
    for(let [key, val] of checkDuple) {
        if(val === 1) {
            singleOne = key;
            break;
        }
    }

    console.log(singleOne);
};

SingleNumber([4,1,2,1,2]);

```

## Solution 2 - XOR
- Time complexity : O(n)
- Space complexity : O(1)    

``` javascript
function SingleNumber2(nums: number[]): void {

    // second solution
    let singleOne: number =0;
    for(let i: number = 0; i < nums.length; i++){
        singleOne^=nums[i];
    }
    
    console.log(singleOne);

};

SingleNumber2([4,1,2,1,2]);
```