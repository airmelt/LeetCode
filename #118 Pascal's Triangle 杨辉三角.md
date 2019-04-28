__Description__:
Given a non-negative integer *numRows*, generate the first *numRows* of Pascal's triangle.

![In Pascal's triangle, each number is the sum of the two numbers directly above it.](http://upload-images.jianshu.io/upload_images/16639143-4492288ba931415f.gif?imageMogr2/auto-orient/strip)

**Example:**

**Input:** 5
**Output:**
```
[
     [1],
    [1,1],
   [1,2,1],
  [1,3,3,1],
 [1,4,6,4,1]
]
```

__题目描述__:
给定一个非负整数 *numRows，*生成杨辉三角的前 *numRows *行。

![在杨辉三角中，每个数是它左上方和右上方的数的和。](http://upload-images.jianshu.io/upload_images/16639143-635269ec9e469820.gif?imageMogr2/auto-orient/strip)

**示例:**

**输入:** 5
**输出:**
```
[
     [1],
    [1,1],
   [1,2,1],
  [1,3,3,1],
 [1,4,6,4,1]
]
```

__思路__:
1. 动态规划 result[i][j] = result[i - 1][j - 1] + result[i - 1][j]
2. 可以设置头尾为0作为结束标志, 错位相加:
```
0 1 0
  0 1 0
| | | |
0 1 1 0

0 1 2 1 0
  0 1 2 1 0
| | | | | |
0 1 3 3 1 0
```
注意初始值为0的情况
时间复杂度O(n^2), 空间复杂度O(n^2), 因为需要记录n行, 每行有n个元素, 合计(n^2 + n)/ 2

__代码__:
__C++__:
```
class Solution {
public:
    vector<vector<int>> generate(int numRows) {
        vector<vector<int>> result(numRows, vector<int>());
        for (int i = 0; i < numRows; i++) {
            result[i].resize(i + 1);
            result[i][0] = 1;
            result[i][i] = 1;
        }
        for (int i = 2; i < numRows; i++) {
            for (int j = 1; j < i; j++) {
                result[i][j] = result[i - 1][j - 1] + result[i - 1][j];
            }
        }
        return result;
    }
};
```

__Java__:
```
class Solution {
    public List<List<Integer>> generate(int numRows) {
        List<List<Integer>> result = new ArrayList<>(numRows);
        if (numRows == 0) return result;
        List<Integer> preRow = new ArrayList<>(1);
        preRow.add(1);
        result.add(preRow);
        for (int i = 1; i < numRows; i++) {
            List<Integer> row = new ArrayList<>(i + 1);
            row.add(0, 1);
            row.add(row.size() - 1, 1);
            for (int j = 1; j < i; j++) {
                row.add(j, preRow.get(j - 1) + preRow.get(j));
            }
            preRow = row;
            result.add(i, row);
        }
        return result;
    }
}
```

__Python__:
```
class Solution:
    def generate(self, numRows: int) -> List[List[int]]:
        result = []
        row = [0, 1, 0]
        for i in range(numRows):     
            result.append(row[1:-1])
            row = [0] + [x + y for x, y in zip(row[1:], row[:-1])] + [0]
        return result
```
