+++
title = "Leetcode - Contains Duplicate"
date = 2019-11-17T18:08:32+08:00
draft = true
tags = ["Leetcode"]
categories = ["Leetcode"]
+++
##### Given an array of integers, find if the array contains any duplicates.
##### Your function should return true if any value appears at least twice in the array, and it should return false if every element is distinct.

#### Example 1:

>Input: [1,2,3,1]
>Output: true

#### Example 2:

>Input: [1,2,3,4]
>Output: false

#### Example 3:

>Input: [1,1,1,3,3,4,3,2,4,2]
>Output: true

### Solution:

#### Solution 1:

```java
class Solution {
    public boolean containsDuplicate(int[] nums) {
        if(nums == null || nums.length <2)
            return false;
        
        for(int i=0; i< nums.length -1;i++){
            for(int j=i+1; j< nums.length ;j++){
                if(nums[i] == nums[j])
                    return true;
            }
        }
        return false;
    }
}
```

#### Solution 2:

```java
class Solution {
    public boolean containsDuplicate(int[] nums) {
        if(nums == null || nums.length <2)
            return false;
        
        Map<Integer,Integer> map = new HashMap();
        for(int i=0; i< nums.length;i++){
           int value = nums[i];
           if(map.get(value) == null){
               map.put(value,1);
           }else{
               return true;
           }
        }
        return false;
    }
}
```

#### Solution 3 （The most time efficient）:

```java
class Solution {
      public boolean containsDuplicate(int[] nums) {
         Arrays.sort(nums);
         for (int i = 0; i < nums.length - 1; ++i) {
            if (nums[i] == nums[i + 1]) return true;
         }
         return false;
     }
}
```


### Conclusion:
1.Can sort array by the embedded java library first ，it's more efficient to work out after sorting
2.Only need to traverse array once by using HashMap or HashSet