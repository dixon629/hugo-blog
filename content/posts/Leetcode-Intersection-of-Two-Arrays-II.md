+++
title = "Leetcode - Intersection of Two Arrays II"
date = 2019-12-12T18:44:45+08:00
draft = true
tags = ["Leetcode"]
categories = ["Leetcode"]
+++

##### Given two arrays, write a function to compute their intersection.

#### Example 1:

>Input: nums1 = [1,2,2,1], nums2 = [2,2]
>Output: [2,2]

#### Example 2:

>Input: nums1 = [4,9,5], nums2 = [9,4,9,8,4]
>Output: [4,9]

#### Note:

Each element in the result should appear as many times as it shows in both arrays.
The result can be in any order.

#### Follow up:

What if the given array is already sorted? How would you optimize your algorithm?
What if nums1's size is small compared to nums2's size? Which algorithm is better?
What if elements of nums2 are stored on disk, and the memory is limited such that you cannot load all elements into the memory at once?

### Solution:
#### Solution 1: Hash 
```java
class Solution {
    public int[] intersect(int[] nums1, int[] nums2) {
        if(nums1 == null || nums2 == null)
          return null;

        if( nums1.length <1 || nums2.length <1){
          return new int[0];
        }
        
        Map<Integer,Integer> map1 = new HashMap<Integer,Integer>();
        for(int i=0;i<nums1.length;i++){
            if(map1.get(nums1[i]) == null){
                map1.put(nums1[i],1);
            }else{
                map1.put(nums1[i],map1.get(nums1[i])+1);
            }
        }
        
        List<Integer> list = new ArrayList<Integer>();
        
       for(int i=0;i<nums2.length;i++){
           int num = nums2[i];
           if(map1.get(num)!=null && map1.get(num)>0){
               list.add(num);
               map1.put(num,map1.get(num)-1);
           }
       }
        
        int[] results = new int[list.size()];
        for(int i=0; i< list.size();i++){
            results[i] = list.get(i);
        }
        
        return results;
    }
   
}
```
#### TODO Solution 2: 排序，双指针法
>先对两个数组A和B排序。
>
>指针a=0，指向A的第一个节点。
>
>指针b=0，指向B的第一个节点。
>
>当A[a] == B[b]时，是一对相同元素。a向前走一步，b向前走一步。
>
>当A[a] < B[b]时。a向前走一步。
>
>当A[a] > B[b]时。b向前走一步。
>
>如此直到A和B有一个遍历完。


### Conclusion:
1. 理解题意，交集和顺序无关，只与元素是否出现以及出现的次数有关
2. Hash性能高，并没说不能用Hash 以及List
3. 熟悉Hash的遍历方式
4. 熟悉Java数组的定义和初始化