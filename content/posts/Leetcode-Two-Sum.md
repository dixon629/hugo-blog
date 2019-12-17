+++
title = "Leetcode - Two Sum"
date = 2019-12-04T18:36:02+08:00
draft = true
tags = ["Leetcode"]
categories = ["Leetcode"]
+++

##### Given an array of integers, return indices of the two numbers such that they add up to a specific target.
##### You may assume that each input would have exactly one solution, and you may not use the same element twice.

#### Example:

>Given nums = [2, 7, 11, 15], target = 9,
>
>Because nums[0] + nums[1] = 2 + 7 = 9,
>return [0, 1].


### Solution:

#### Solution 1: 暴力法  时间 O(n^2) 空间 0(1)

```java
class Solution {
    public int[] twoSum(int[] nums, int target) {
        if(nums == null)
            return null;
        if(nums.length <2)
            return new int[0];
        
        for(int i=0; i<nums.length;i++){
            for(int j=i+1; j<nums.length;j++){
                if(nums[i] + nums[j] == target){
                    return new int[]{i,j};
                }
            }
        }
        
        return new int[0];
    }
}
```

#### Solution 2: 哈希表 时间O(n),空间 O(n)，性能更好

```java
class Solution {
    public int[] twoSum(int[] nums, int target) {
        if(nums == null)
            return null;
        
        Map<Integer,Integer> map = new HashMap();
        for(int i=0;i<nums.length;i++){
            int complement = target - nums[i];
            if(map.containsKey(complement)){
                return new int[]{map.get(complement),i};
            }else{
                map.put(nums[i],i);
            }
        }
        throw new IllegalArgumentException("No two sum solution");

    }
}
```

### Conclusion:
1. 哈希表取特定数的时间复杂度仅为O(1)
2. 哈希表法虽然有额外的空间开销，当时当n越大时其消耗时间大幅小于暴力法