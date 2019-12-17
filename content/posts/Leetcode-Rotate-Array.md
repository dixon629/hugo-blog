+++
title = "Leetcode - Rotate Array"
date = 2019-11-20T18:24:06+08:00
draft = true
tags = ["Leetcode"]
categories = ["Leetcode"]
+++

#####  Given an array, rotate the array to the right by k steps, where k is non-negative.

#### Example 1:

>Input: [1,2,3,4,5,6,7] and k = 3
>Output: [5,6,7,1,2,3,4]
>Explanation:
>rotate 1 steps to the right: [7,1,2,3,4,5,6]
>rotate 2 steps to the right: [6,7,1,2,3,4,5]
>rotate 3 steps to the right: [5,6,7,1,2,3,4]

#### Example 2:

>Input: [-1,-100,3,99] and k = 2
>Output: [3,99,-1,-100]
>Explanation: 
>rotate 1 steps to the right: [99,-1,-100,3]
>rotate 2 steps to the right: [3,99,-1,-100]


**Note:**

Try to come up as many solutions as you can, there are at least 3 different ways to solve this problem.
Could you do it in-place with O(1) extra space?

### Solution:

```java
class Solution {
    public void rotate(int[] nums, int k) {
        if(nums == null || nums.length <2)
            return;
        
        int len = nums.length;
        k = k % len;
        int count = 0;
        int temp;
        int preValue;
        
        for(int start = 0; count < len; start ++){
            int current = start;
            // Store the repaced previous value for next roatation
            preValue = nums[current];
            do{
                int next = (current + k) % len;
                temp = nums[next];
                nums[next] = preValue;
                preValue = temp;
                current = next;

                count++;
            }while( current != start);
             
        }  
    }
}
```

### Conclusion:

1. 使用环状替换
2. 发转法 Reverse

分析右移后数据结构，发现可以通过三次反转达到移动效果。

>原始数组                  : 1 2 3 4 5 6 7
>反转所有数字后             : 7 6 5 4 3 2 1
>反转前 k 个数字后          : 5 6 7 4 3 2 1
>反转后 n-k 个数字后        : 5 6 7 1 2 3 4 --> 结果