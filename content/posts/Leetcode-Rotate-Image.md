+++
title = "Leetcode - Rotate Image"
date = 2019-11-30T18:28:14+08:00
draft = true
tags = ["Leetcode"]
categories = ["Leetcode"]
+++

##### You are given an n x n 2D matrix representing an image.
##### 
##### Rotate the image by 90 degrees (clockwise).
##### 
##### Note:
##### 
##### You have to rotate the image in-place, which means you have to modify the input 2D matrix directly. DO NOT allocate another 2D matrix and do the rotation.

**Example 1:**

>Given input matrix = 
>[
>  [1,2,3],
>  [4,5,6],
>  [7,8,9]
>],
>
>rotate the input matrix in-place such that it becomes:
>[
>  [7,4,1],
>  [8,5,2],
>  [9,6,3]
>]

**Example 2:**
>
>Given input matrix =
>[
>  [ 5, 1, 9,11],
>  [ 2, 4, 8,10],
>  [13, 3, 6, 7],
>  [15,14,12,16]
>], 
>
>rotate the input matrix in-place such that it becomes:
>[
>  [15,13, 2, 5],
>  [14, 3, 4, 1],
>  [12, 6, 8, 9],
>  [16, 7,10,11]
>]

### Solution:

#### Solution 1: 环形移位


```java
class Solution {
    public void rotate(int[][] matrix) {
        if(matrix == null)
            return;
        
        int temp = 0;
        int n = matrix.length;
        Set<String> roated = new HashSet();
        for(int rowIndex = 0; rowIndex < n; rowIndex++){
            for(int colIndex =0; colIndex < n; colIndex++){

                String pos = String.valueOf(rowIndex)+String.valueOf(colIndex);
                if(roated.contains(pos)) continue;
                
                int currentRowIndex = rowIndex;
                int currentColIndex = colIndex;
                int previous = matrix[rowIndex][colIndex];
  
                do{
                    int nextRowIndex = currentColIndex;
                    int nextColIndex = n-1-currentRowIndex;
                    temp = matrix[nextRowIndex][nextColIndex];
                    matrix[nextRowIndex][nextColIndex] = previous;
                    previous = temp;
                    
                    roated.add(String.valueOf(currentRowIndex)+String.valueOf(currentColIndex));
                    currentRowIndex = nextRowIndex;
                    currentColIndex = nextColIndex;
     
                } while(currentRowIndex != rowIndex || currentColIndex!=colIndex);
                
            } 
        } 
    }
}
```

#### Solution 2: 先转270 再转-180
```java

class Solution {
    public void rotate(int[][] matrix) {
        if(matrix == null)
            return;
        
        // rotate 270
        int n = matrix.length;
        for(int i=0;i < n;i++){
            for(int j=i; j< n; j++){
                int temp = matrix[i][j];
                matrix[i][j] = matrix[j][i];
                matrix[j][i] = temp; 
            }
        }
        // rotate -180
        for(int i=0;i <n;i++){
            for(int j=0; j< n/2; j++){
                int temp =  matrix[i][j];
                matrix[i][j] = matrix[i][n-1-j];
                matrix[i][n-1-j] = temp;
            }
        }
    }
}
```

#### Solution 3: 官方答案，循环移动
```java
class Solution {
  public void rotate(int[][] matrix) {
    int n = matrix.length;
    for (int i = 0; i < n / 2 + n % 2; i++) {
      for (int j = 0; j < n / 2; j++) {
        int[] tmp = new int[4];
        int row = i;
        int col = j;
        for (int k = 0; k < 4; k++) {
          tmp[k] = matrix[row][col];
          int x = row;
          row = col;
          col = n - 1 - x;
        }
        for (int k = 0; k < 4; k++) {
          matrix[row][col] = tmp[(k + 3) % 4];
          int x = row;
          row = col;
          col = n - 1 - x;
        }
      }
    }
  }
}
```


### Conclusion:
1. 注意观察数组转换的特性，如果所有元素行列互换，其实就是转了270度，要达到旋转90度的效果，就需要再旋转-180度，其实就是行数组的元素反转
2. 第3种方案的循环条件比较难确定