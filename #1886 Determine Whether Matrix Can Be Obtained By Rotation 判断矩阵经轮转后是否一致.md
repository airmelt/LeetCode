# 1886 Determine Whether Matrix Can Be Obtained By Rotation 判断矩阵经轮转后是否一致

__Description:__

Given two `n x n` binary matrices `mat` and `target`, return `true` _if it is possible to make_ `mat` _equal to_ `target` _by __rotating___ `mat` _in __90-degree increments__, or_ `false` _otherwise._

__Example:__

Example 1:

![1886-1](https://assets.leetcode.com/uploads/2021/05/20/grid3.png)

```text
Input: mat = [[0,1],[1,0]], target = [[1,0],[0,1]]
Output: true
Explanation: We can rotate mat 90 degrees clockwise to make mat equal target.
```

Example 2:

![1886-2](https://assets.leetcode.com/uploads/2021/05/20/grid4.png)

```text
Input: mat = [[0,1],[1,1]], target = [[1,0],[0,1]]
Output: false
Explanation: It is impossible to make mat equal to target by rotating mat.
```

Example 3:

![1886-3](https://assets.leetcode.com/uploads/2021/05/26/grid4.png)

```text
Input: mat = [[0,0,0],[0,1,0],[1,1,1]], target = [[1,1,1],[0,1,0],[0,0,0]]
Output: true
Explanation: We can rotate mat 90 degrees clockwise two times to make mat equal target.
```

__Constraints:__

- `n == mat.length == target.length`
- `n == mat[i].length == target[i].length`
- `1 <= n <= 10`
- `mat[i][j]` and `target[i][j]` are either `0` or `1`.

__题目描述:__

给你两个大小为 `n x n` 的二进制矩阵 `mat` 和 `target` 。现 __以 90 度顺时针轮转__ 矩阵 `mat` 中的元素 __若干次__ ，如果能够使 `mat` 与 `target` 一致，返回 `true` ；否则，返回 `false` _。_

__示例:__

示例 1：

![1886-4](https://assets.leetcode.com/uploads/2021/05/20/grid3.png)

```text
输入：mat = [[0,1],[1,0]], target = [[1,0],[0,1]]
输出：true
解释：顺时针轮转 90 度一次可以使 mat 和 target 一致。
```

示例 2：

![1886-5](https://assets.leetcode.com/uploads/2021/05/20/grid4.png)

```text
输入：mat = [[0,1],[1,1]], target = [[1,0],[0,1]]
输出：false
解释：无法通过轮转矩阵中的元素使 equal 与 target 一致。
```

示例 3：

![1886-6](https://assets.leetcode.com/uploads/2021/05/26/grid4.png)

```text
输入：mat = [[0,0,0],[0,1,0],[1,1,1]], target = [[1,1,1],[0,1,0],[0,0,0]]
输出：true
解释：顺时针轮转 90 度两次可以使 mat 和 target 一致。
```

__提示：__

- `n == mat.length == target.length`
- `n == mat[i].length == target[i].length`
- `1 <= n <= 10`
- `mat[i][j]` 和 `target[i][j]` 不是 `0` 就是 `1`

__思路:__

```text
模拟
分别对矩阵旋转 0/90/180/270 度
旋转90度: mat[n - j - 1][i]
旋转180度: mat[n - i - 1][n - j - 1]
旋转270度: mat[j][n - i - 1]
如果 4 次旋转对应都不相等返回 false
检查完所有元素返回 true  
时间复杂度为 O(N ^ 2), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    bool findRotation(vector<vector<int>>& mat, vector<vector<int>>& target) 
    {
        int n = mat.size(), flag0 = true, flag1 = true, flag2 = true, flag3 = true;
        for (int i = 0; i < n; i++) 
        {
            for (int j = 0; j < n; j++) 
            {
                flag0 &= target[i][j] == mat[i][j];
                flag1 &= target[i][j] == mat[n - j - 1][i];
                flag2 &= target[i][j] == mat[n - i - 1][n - j - 1];
                flag3 &= target[i][j] == mat[j][n - i - 1];
                if (!(flag0 | flag1 | flag2 | flag3)) return false;
            }
        }
        return true;
    }
};
```

__Java__:

```Java
class Solution {
    public boolean findRotation(int[][] mat, int[][] target) {
        int n = mat.length, flag0 = 1, flag1 = 1, flag2 = 1, flag3 = 1;
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++) {
                flag0 &= (target[i][j] == mat[i][j] ? 1 : 0);
                flag1 &= (target[i][j] == mat[n - j - 1][i] ? 1 : 0);
                flag2 &= (target[i][j] == mat[n - i - 1][n - j - 1] ? 1 : 0);
                flag3 &= (target[i][j] == mat[j][n - i - 1] ? 1 : 0);
                if ((flag0 | flag1 | flag2 | flag3) == 0) return false;
            }
        }
        return true;
    }
}
```

__Python__:

```Python
class Solution:
    def findRotation(self, mat: List[List[int]], target: List[List[int]]) -> bool:
        return target in [(mat := list(map(list, zip(*mat[::-1])))) for i in range(4)]
```
