+++
title = "Leetcode - Valid Sudoku"
date = 2019-12-02T18:31:13+08:00
draft = true
tags = ["Leetcode"]
categories = ["Leetcode"]
+++

##### Determine if a 9x9 Sudoku board is valid. Only the filled cells need to be validated according to the following rules:
##### Each row must contain the digits 1-9 without repetition.Each column must contain the digits 1-9 without repetition.Each of the 9 3x3 sub-boxes of the grid must contain the digits 1-9 without repetition.
##### A partially filled sudoku which is valid.The Sudoku board could be partially filled, where empty cells are filled with the character '.'.
#### Example 1:Input:
>[
>  ["5","3",".",".","7",".",".",".","."],
>  ["6",".",".","1","9","5",".",".","."],
>  [".","9","8",".",".",".",".","6","."],
>  ["8",".",".",".","6",".",".",".","3"],
>  ["4",".",".","8",".","3",".",".","1"],
>  ["7",".",".",".","2",".",".",".","6"],
>  [".","6",".",".",".",".","2","8","."],
>  [".",".",".","4","1","9",".",".","5"],
>  [".",".",".",".","8",".",".","7","9"]
>]
>Output: true

#### Example 2:Input:
>[
>  ["8","3",".",".","7",".",".",".","."],
>  ["6",".",".","1","9","5",".",".","."],
>  [".","9","8",".",".",".",".","6","."],
>  ["8",".",".",".","6",".",".",".","3"],
>  ["4",".",".","8",".","3",".",".","1"],
>  ["7",".",".",".","2",".",".",".","6"],
>  [".","6",".",".",".",".","2","8","."],
>  [".",".",".","4","1","9",".",".","5"],
>  [".",".",".",".","8",".",".","7","9"]
>]
>Output: false
>
Explanation: Same as Example 1, except with the 5 in the top left corner being modified to 8. Since there are two 8's in the top left 3x3 sub-box, it is invalid.

**Note:A Sudoku board (partially filled) could be valid but is not necessarily solvable.Only the filled cells need to be validated according to the mentioned rules.The given board contain only digits 1-9 and the character '.'.The given board size is always 9x9.**


### Solution:

#### Solution 1: Hash

```java
class Solution {
    
    public boolean isValidSudoku(char[][] board) {
         if (board == null)
          return false;

        List<Map<Integer, Integer>> subBoxs = new ArrayList(9);
        List<Map<Integer, Integer>> columns = new ArrayList(9);

        // Initialize the fixed 9 sub-box and colums
        for (int i = 0; i < 9; i++) {
          Map<Integer, Integer> subBoxMap = new HashMap();
          subBoxs.add(subBoxMap);

          Map<Integer, Integer> colMap = new HashMap();
          columns.add(colMap);
        }

        for (int rowIndex = 0; rowIndex < board.length; rowIndex++) {
          char[] row = board[rowIndex];
          Map<Integer, Integer> rowMap = new HashMap<>();

          for (int colIndex = 0; colIndex < row.length; colIndex++) {
            char c = row[colIndex];
            if (c == '.')
              continue;

            int num = Integer.parseInt(String.valueOf(row[colIndex]));

            //Put the num to the correct sub box
            int colSection = colIndex / 3;
            int rowSection = rowIndex / 3;
            int section = rowSection * 3 + colSection;
            Map<Integer, Integer> subBoxMap = subBoxs.get(section);
            if (subBoxMap.get(num) == null) {
              subBoxMap.put(num, 1);
            } else {
              return false;
            }

            if (rowMap.get(num) == null) {
              rowMap.put(num, 1);
            } else {
              return false;
            }

            // Put the num to the correct column map
            Map<Integer, Integer> colMap = columns.get(colIndex);
            if (colMap.get(num) == null) {
              colMap.put(num, 1);
            } else {
              return false;
            }
          }

        }

        return true;
    }
    
}
```

#### Solution 2: Offcial version

```java
class Solution {
  public boolean isValidSudoku(char[][] board) {
    // init data
    HashMap<Integer, Integer> [] rows = new HashMap[9];
    HashMap<Integer, Integer> [] columns = new HashMap[9];
    HashMap<Integer, Integer> [] boxes = new HashMap[9];
    for (int i = 0; i < 9; i++) {
      rows[i] = new HashMap<Integer, Integer>();
      columns[i] = new HashMap<Integer, Integer>();
      boxes[i] = new HashMap<Integer, Integer>();
    }

    // validate a board
    for (int i = 0; i < 9; i++) {
      for (int j = 0; j < 9; j++) {
        char num = board[i][j];
        if (num != '.') {
          int n = (int)num;
          int box_index = (i / 3 ) * 3 + j / 3;

          // keep the current cell value
          rows[i].put(n, rows[i].getOrDefault(n, 0) + 1);
          columns[j].put(n, columns[j].getOrDefault(n, 0) + 1);
          boxes[box_index].put(n, boxes[box_index].getOrDefault(n, 0) + 1);

          // check if this value has been already seen before
          if (rows[i].get(n) > 1 || columns[j].get(n) > 1 || boxes[box_index].get(n) > 1)
            return false;
        }
      }
    }

    return true;
  }
}
```



### Conclusion:
1.Hash 算法效率高