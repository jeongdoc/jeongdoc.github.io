---
title: JAVA-알고리즘 Leetcode(Easy)-Single Number
date: 2022-10-17 16:56:00 +0900
categories: [Algorithm,Javascript]
tags: [algorithm,알고리즘,javascript,leetcode]
---

### Leetcode(Easy) Single Number

## Question
Given a __non-empty__ array of integers `nums`, every element appears twice except for one. Find that single one.
You must implement a solution with a linear runtime complexity and use only constant extra space.

## 대충 해석
주어진 비어있지 않은 배열 정수 배열에서 쌍이 아닌 정수를 찾으세요.

## Example
``` text
#1
Input: root = [1,null,2,3]
Output: [1,3,2]

#2
Input: root = []
Output: []

#3
Input: root = [1]
Output: [1]
```

***

## Solution 1 - Map
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