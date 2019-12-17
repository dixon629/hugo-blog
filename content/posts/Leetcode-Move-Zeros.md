+++
title = "Leetcode - Move Zeros"
date = 2019-12-07T18:39:12+08:00
draft = true
tags = ["Leetcode"]
categories = ["Leetcode"]
+++
##### Given an array nums, write a function to move all 0's to the end of it while maintaining the relative order of the non-zero elements.

#### Example:

>Input: [0,1,0,3,12]
>Output: [1,3,12,0,0]

**Note:**

You must do this in-place without making a copy of the array.
Minimize the total number of operations.

### Solution:

#### Solution 1: Low efficency 移位

```java
class Solution {
    public void moveZeroes(int[] nums) {
        if(nums == null || nums.length <2)
            return;

        int p = nums.length -1;
        int end = nums.length -1;
        while(p >=0){
            if(nums[p] ==0){
                int q = p;
                while(q <= end){
                    if(q == end){
                        nums[q] = 0;
                    }else{
                        nums[q] = nums[q+1];
                    }
                    q++;
                } 
                end --;
            }
            p--;

        }
        
    }
}
```

#### Solution 2:

```java
class Solution {
    public void moveZeroes(int[] nums) {
        if(nums == null || nums.length <2)
            return;
        
        // None-Zero Pointer
        for (int i = 0, j = 0; i < nums.length; i++)
		{
			if (nums[i] != 0)
			{
				int temp = nums[i];
				nums[i] = nums[j];
				nums[j] = temp;
				j++;
			}
		} 
    }
}
```

>将数组分成两个区域，一个是非0数区，一个是0数区，而我们要做的唯一一件事就是把非0数移动到非0数区。指针j记录最后一个非0数的位置，在j的左边，所有数都是非0数。 第一轮之前: [{非0数区}|0,1,0,3,12,15] 第一轮： [1, {非0数区}|0, 0, 3,12,15] 第二轮： [1, {非0数区}|0, 0, 3,12,15] 第三轮： [1, 3,{非0数区}|0,0,12,15
>

### Conclusion: