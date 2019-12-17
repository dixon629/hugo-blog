+++
title = "Leetcode - Single Number"
date = 2019-12-16T18:47:24+08:00
draft = true
tags = ["Leetcode"]
categories = ["Leetcode"]
+++
##### Given a non-empty array of integers, every element appears twice except for one. Find that single one.

#### Note:

Your algorithm should have a linear runtime complexity. Could you implement it without using extra memory?

#### Example 1:

Input: [2,2,1]
Output: 1

#### Example 2:

Input: [4,1,2,1,2]
Output: 4

### Solution:

#### Solution 1: 排序，然后比较

```java
class Solution {
    public int singleNumber(int[] nums) {
        if(nums.length  == 1)
            return nums[0];
        
        Arrays.sort(nums);
          
        for(int i=0; i< nums.length; i++){
            if(i ==0){
                if( nums[i] != nums[i+1]){
                    return nums[i];
                }
            }else if(i == nums.length-1){
                if( nums[i-1] != nums[i]){
                    return nums[i];
                }
            }else{
                if(nums[i-1] != nums[i] && nums[i] != nums[i+1]){
                    return nums[i];
                }
            }
        }
        return nums[0];
    }
}
```

#### Solution 2: 异或

```java
class Solution {
    public int singleNumber(int[] nums) {
        if(nums.length  == 1)
            return nums[0];
        
        int single = nums[0];
        for(int i = 1; i<nums.length;i++){
            single = single ^ nums[i];
        }
        
        return single;

    }
}
```

### Conclusion:

##### 理解异或运算的特别，以及题目只有一个单一数字的解释：
1. 如果我们对 0 和二进制位做 XOR 运算，得到的仍然是这个二进制位
a^0=a
2. 如果我们对相同的二进制位做 XOR 运算，返回的结果是 0
a^a=0
3. XOR 满足交换律和结合律
a^b^a =(a^a)^b=0^b=b
