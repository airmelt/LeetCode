# 2679 Sum in a Matrix 矩阵中的和

__Description:__

You are given a __0-indexed__ 2D integer array `nums`. Initially, your score is `0`. Perform the following operations until the matrix becomes empty:

Return the final score.

__Example:__

Example 1:

```text
Input: nums = [[7,2,1],[6,4,2],[6,5,3],[3,2,1]]
Output: 15
Explanation: In the first operation, we remove 7, 6, 6, and 3. We then add 7 to our score. Next, we remove 2, 4, 5, and 2. We add 5 to our score. Lastly, we remove 1, 2, 3, and 1. We add 3 to our score. Thus, our final score is 7 + 5 + 3 = 15.
```

Example 2:

```text
Input: nums = [[1]]
Output: 1
Explanation: We remove 1 and add it to the answer. We return 1.
```

__Constraints:__

- `1 <= nums.length <= 300`
- `1 <= nums[i].length <= 500`
- `0 <= nums[i][j] <= 10 ^ 3`

__题目描述:__

给你一个下标从 __0__ 开始的二维整数数组 `nums` 。一开始你的分数为 `0` 。你需要执行以下操作直到矩阵变为空:

请你返回最后的 分数 。

__示例:__

示例 1：

```text
输入：nums = [[7,2,1],[6,4,2],[6,5,3],[3,2,1]]
输出：15
解释：第一步操作中，我们删除 7 ，6 ，6 和 3 ，将分数增加 7 。下一步操作中，删除 2 ，4 ，5 和 2 ，将分数增加 5 。最后删除 1 ，2 ，3 和 1 ，将分数增加 3 。所以总得分为 7 + 5 + 3 = 15 。
```

示例 2：

```text
输入：nums = [[1]]
输出：1
解释：我们删除 1 并将分数增加 1 ，所以返回 1 。
```

__提示：__

- `1 <= nums.length <= 300`
- `1 <= nums[i].length <= 500`
- `0 <= nums[i][j] <= 10 ^ 3`

__思路:__

```text
排序
先给每行排序
然后按列取最大值
结果加上每列的最大值
时间复杂度为 O(MNlogN), 空间复杂度为 O(logN)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int matrixSum(vector<vector<int>>& nums) 
    {
        for (auto& row : nums) sort(row.begin(), row.end());
        int result = 0, n = nums.front().size();
        for (int j = 0; j < n; j++) 
        {
            int cur = 0;
            for (const auto& row : nums) cur = max(cur, row[j]);
            result += cur;
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int matrixSum(int[][] nums) {
        for (var row : nums) Arrays.sort(row);
        int result = 0, n = nums[0].length;
        for (int j = 0; j < n; j++) {
            int cur = 0;
            for (var row : nums) cur = Math.max(cur, row[j]);
            result += cur;
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def matrixSum(self, nums: List[List[int]]) -> int:
        return sum(max(row) for row in zip(*nums)) if (nums := [sorted(num) for num in nums]) else 0
```
