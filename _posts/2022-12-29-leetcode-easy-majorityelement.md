---
title: JS-알고리즘 Leetcode(Easy)-Majority Element
date: 2022-12-29 14:31:00 +0900
categories: [Algorithm,Javascript]
tags: [algorithm,알고리즘,javascript,leetcode]
---

## Question
Given an array `nums` of size `n`, return the majority element.    
The majority element is the element that appears more than `⌊n / 2⌋` times. You may assume that the majority element always exists in the array.

## 대충 해석
주어진 n크기의 배열에서 majority element를 찾아 리턴하세요.    
단, majority element는 n/2번 이상 무조건 존재한다고 가정합니다.    

## Example
``` text
#1
Input: nums = [3,2,3]
Output: 3

#2
Input: nums = [2,2,1,1,1,2,2]
Output: 2
```

***

## 고려해야할 부분
배열 안, 어떠한 한 요소는 무조건 절반 이상 존재한다는게 중요하다.

***

## Solution 1
배열을 정렬한 후에 n과 n+1번째 요소를 비교하면서 같을 때마다 count를 1 증가시키다가    
count값이 배열 길이의 절반보다 클때 해당 값을 리턴해준다.    

근데 다 풀고 잠깐 생각해보니 이렇게 하지말고,     
그냥 `array.length/2`번째 인덱스에 위치한 값을 바로 리턴하면 될 것 같다는 생각이 들었다.    
``` javascript
var majorityElement = function(nums) {
    // O(nlogn)
    // 생각해보니 그냥 바로 nums.length/2 에 위치한 element 리턴해주면 될듯?
    if(nums.length < 3) return nums[0];

    nums.sort();
    let cnt = 1;
    for(let i = 0; i < nums.length-1; i ++) {
        
        if((nums[i] ^ nums[i+1]) === 0) {
            cnt ++;
        } else {
            cnt = 1;
        }

        if(cnt > Math.floor(nums.length/2)) {
            return nums[i];
        }
    }
};
```

## Solution 2
첫 번째 풀이로 우선 문제를 풀고 다르게도 풀 수 있을 것 같아서 좀 고민하다, 문제 하단에 있는 follow-up 조건을 그제서야 봤다.    
> Follow-up: Could you solve the problem in linear time and in O(1) space?    
그래서 조금 고민하다가 bit manipulation으로도 풀 수 있을 것 같은데 싶어서 두 번째는 비트 조작으로 풀이.    
``` javascript
var majorityElement2 = function(nums) {
    // bitwise
    // Maybe.. O(32n)?
    if(nums.length < 3) return nums[0];

    let majority = 0;
    for(let i = 0; i < 32; i ++) {
        let cnt = 0;
        for(let j = 0; j < nums.length; j ++) {
            if((nums[j] >> i) & 1 === 1) {
                cnt ++;
            }

            if(cnt > Math.floor(nums.length/2)) {
                console.log(nums[j])
                majority |= (1 << i);
            }
        }
    }

    return majority;
};
```

## Solution 3
Solution 2의 방식으로 푼 후에 아무리 생각해도 for문 하나로도 될 것 같은데.. 하고   
선형시간에 상수공간이라는 follow-up을 곰곰이 보며 고민하다가    
> the element that appears more than `⌊n / 2⌋` times.    
이 부분을 보고 문득 _"그러게, 과반수 투표 알고리즘인데?"_ 라는 생각이 들어 해당 알고리즘을 사용하여 마지막 풀이를 했다.    

* 과반수 투표 알고리즘(majority vote algorithm)    
- 배열 내에 절반 이상 존재하는 원소를 선형시간(linear time), 상수공간(constant space)으로 찾을 수 있는 알고리즘.
- 단, 배열에 절반 이상 존재하는 원소가 무조건 있다는 조건이 필요. 이 조건이 보장되지 않는다면 결과값으로 쓰레기 값이 나올 수도 있음.    
``` javascript
var majorityElement3 = function(nums) {
    // Boyer-Moore, majority vote algorithm
    // O(n)
    if(nums.length < 3) return nums[0];

    let cnt = 0;
    let majority = 0;
    for(let i = 0; i < nums.length; i ++) {
        
        if((nums[i] ^ majority) === 0) {
            cnt ++;
        } else {
            cnt --;
        }

        if(cnt < 0) {
            majority = nums[i];
            cnt ++;
        }
        
    }
    return majority;
};
```