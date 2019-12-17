+++
title = "Leetcode - Best Time to Buy and Sell Stock II"
date = 2019-12-17T18:54:12+08:00
draft = true
tags = ["Leetcode"]
categories = ["Leetcode"]
+++
##### Say you have an array for which the ith element is the price of a given stock on day i.
##### 
##### Design an algorithm to find the maximum profit. You may complete as many transactions as you like (i.e., buy one and sell one share of the stock multiple times).

**Note**: You may not engage in multiple transactions at the same time (i.e., you must sell the stock before you buy again).

#### Example 1:

>Input: [7,1,5,3,6,4]
>Output: 7
>Explanation: Buy on day 2 (price = 1) and sell on day 3 (price = 5), profit = 5-1 = 4.
>             Then buy on day 4 (price = 3) and sell on day 5 (price = 6), profit = 6-3 = 3.
#### Example 2:

>Input: [1,2,3,4,5]
>Output: 4
>Explanation: Buy on day 1 (price = 1) and sell on day 5 (price = 5), profit = 5-1 = 4.
>             Note that you cannot buy on day 1, buy on day 2 and sell them later, as you are
>             engaging multiple transactions at the same time. You must sell before buying again.
#### Example 3:

>Input: [7,6,4,3,1]
>Output: 0
>Explanation: In this case, no transaction is done, i.e. max profit = 0.

### Solution:

```java
class Solution {
    public int maxProfit(int[] prices) {
        if(prices == null || prices.length < 2)
            return 0;
        
        int profit = 0;
        int len = prices.length;
        for(int i=0; i< len; i++){
            if(i< len-1 &&  prices[i+1] > prices[i]){
                profit  += prices[i+1] - prices[i];
            }
        }
        return profit;
    }
}
```

### Conclusion:

1. Greedy algorithm, breakdown problem to each day, work out the best result for each day
2. For each day, there're three conditions buy, sell, noting to do
3. For each day, it can make mutliple transcations, can buy first and sell or sell first and then buy
4. Dynamic programming algorith
5. Peak and vally algorithm