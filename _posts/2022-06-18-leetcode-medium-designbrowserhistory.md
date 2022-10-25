---
title: JAVA-알고리즘 Leetcode(Medium)-Design Browser History
date: 2022-06-18 11:57:00 +0900
categories: [Algorithm,JAVA]
tags: [algorithm,알고리즘,java,leetcode]
---

## Question
You have a browser of one tab where you start on the homepage and you can visit another url, get back in the history number of steps or move forward in the history number of steps.

Implement the BrowserHistory class:

    * BrowserHistory(string homepage) Initializes the object with the homepage of the browser.
    * void visit(string url) Visits url from the current page. It clears up all the forward history.
    * string back(int steps) Move steps back in history. If you can only return x steps in the history and steps > x, 
      you will return only x steps. Return the current url after moving back in history at most steps.
    * string forward(int steps) Move steps forward in history. If you can only forward x steps in the history and steps > x, 
      you will forward only x steps. Return the current url after forwarding in history at most steps.

## 대충 해석
주어진 비어있지 않은 배열 정수 배열에서 쌍이 아닌 정수를 찾으세요.

## Example
``` text
#1
Input:
["BrowserHistory","visit","visit","visit","back","back","forward","visit","forward","back","back"]
[["leetcode.com"],["google.com"],["facebook.com"],["youtube.com"],[1],[1],[1],["linkedin.com"],[2],[2],[7]]
Output:
[null,null,null,null,"facebook.com","google.com","facebook.com",null,"linkedin.com","google.com","leetcode.com"]

Explanation:
BrowserHistory browserHistory = new BrowserHistory("leetcode.com");
browserHistory.visit("google.com");       // You are in "leetcode.com". Visit "google.com"
browserHistory.visit("facebook.com");     // You are in "google.com". Visit "facebook.com"
browserHistory.visit("youtube.com");      // You are in "facebook.com". Visit "youtube.com"
browserHistory.back(1);                   // You are in "youtube.com", move back to "facebook.com" return "facebook.com"
browserHistory.back(1);                   // You are in "facebook.com", move back to "google.com" return "google.com"
browserHistory.forward(1);                // You are in "google.com", move forward to "facebook.com" return "facebook.com"
browserHistory.visit("linkedin.com");     // You are in "facebook.com". Visit "linkedin.com"
browserHistory.forward(2);                // You are in "linkedin.com", you cannot move forward any steps.
browserHistory.back(2);                   // You are in "linkedin.com", move back two steps to "facebook.com" then to "google.com". return "google.com"
browserHistory.back(7);                   // You are in "google.com", you can move back only one step to "leetcode.com". return "leetcode.com"
```

***

## Solution 1 
``` java
// 1472. Design Browser History
class BrowserHistory {

    HistoryNode curr;
    HistoryNode last;

    public static class HistoryNode {
        HistoryNode left;
        HistoryNode right;
        String url;
        public HistoryNode(String homepage) {
            this.url = homepage;
        }
    }
    public BrowserHistory(String homepage) {
        last = new HistoryNode(homepage);
        curr = last;
    }
    
    public void visit(String url) {
        HistoryNode vs = new HistoryNode(url);
        curr.right = vs;
        vs.left = curr;
        curr = vs;
    }
    
    public String back(int steps) {
        while(curr.left != null && steps > 0) {
            steps --;
            curr = curr.left;
        }
        
        return curr.url;
    }
    
    public String forward(int steps) {
        while(curr.right != null && steps > 0) {
            steps --;
            curr = curr.right;
        }
        
        return curr.url;
    }

    public static void main(String args[]) {
        BrowserHistory browserHistory = new BrowserHistory("leetcode.com");
        browserHistory.visit("google.com");       // You are in "leetcode.com". Visit "google.com"
        browserHistory.visit("facebook.com");     // You are in "google.com". Visit "facebook.com"
        browserHistory.visit("youtube.com");      // You are in "facebook.com". Visit "youtube.com"
        System.out.println(browserHistory.back(1));
        System.out.println(browserHistory.back(1));                   // You are in "facebook.com", move back to "google.com" return "google.com"
        System.out.println(browserHistory.forward(1));                // You are in "google.com", move forward to "facebook.com" return "facebook.com"
        browserHistory.visit("linkedin.com");     // You are in "facebook.com". Visit "linkedin.com"
        System.out.println(browserHistory.forward(2));                // You are in "linkedin.com", you cannot move forward any steps.
        System.out.println(browserHistory.back(2));                   // You are in "linkedin.com", move back two steps to "facebook.com" then to "google.com". return "google.com"
        System.out.println(browserHistory.back(7));                   // You are in "google.com", you can move 
    }
}
```