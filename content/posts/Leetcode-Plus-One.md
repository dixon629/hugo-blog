+++
title = "Leetcode - Plus One"
date = 2019-12-10T18:41:56+08:00
draft = true
tags = ["Leetcode"]
categories = ["Leetcode"]
+++

# Leetcode - Plus One

##### Given a non-empty array of digits representing a non-negative integer, plus one to the integer.
##### The digits are stored such that the most significant digit is at the head of the list, and each element in the array contain a single digit.
##### You may assume the integer does not contain any leading zero, except the number 0 itself.

#### Example 1:

>Input: [1,2,3]
>Output: [1,2,4]
>Explanation: The array represents the integer 123.

#### Example 2:

>Input: [4,3,2,1]
>Output: [4,3,2,2]
>Explanation: The array represents the integer 4321.

### Solution:

#### Solution 1: recursion

```java
class Solution {
    public int[] plusOne(int[] digits) {
      int units = digits[digits.length-1]+1;
      if(units < 10){
         digits[digits.length-1] = units;
         return digits;
      }else{  
         digits[digits.length-1] = units-10;
          // The previous digit plus one
         if(digits.length ==1){
             int[] results = new int[digits.length+1];
             results[0] =1;
             results[1] = digits[0];
             return results; 
         }else{
             int[] pre = new  int[digits.length-1];
             for(int i=0;i<pre.length;i++){
                 pre[i] = digits[i];
             }
            int[] next= plusOne(pre);
            int[] results = new int[next.length+1];
            for(int i=0;i<results.length;i++){
                if(i!=results.length-1){
                    results[i] = next[i];
                }else{
                    results[i] = digits[digits.length-1];
                }
            }
            return results;
         }
      }
    }
}
```

#### Solution 2: Offcial solution  
```java
class Solution {
    public int[] plusOne(int[] digits) {
        for (int i = digits.length - 1; i >= 0; i--) {
            digits[i]++;
            digits[i] = digits[i] % 10;
            if (digits[i] != 0) return digits;
        }
        digits = new int[digits.length + 1];
        digits[0] = 1;
        return digits;
    }
}

```


### Conclusion:

1. 递归移位+1