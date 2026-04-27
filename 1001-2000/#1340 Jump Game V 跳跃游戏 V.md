# 1340 Jump Game V 跳跃游戏 V

__Description:__

Given an array of integers arr and an integer d. In one step you can jump from index i to index:

i + x where: i + x < arr.length and 0 < x <= d.
i - x where: i - x >= 0 and 0 < x <= d.
In addition, you can only jump from index i to index j if arr[i] > arr[j] and arr[i] > arr[k] for all indices k between i and j (More formally min(i, j) < k < max(i, j)).

You can choose any index of the array and start jumping. Return the maximum number of indices you can visit.

Notice that you can not jump outside of the array at any time.

__Example:__

Example 1:

![Jump Game](https://assets.leetcode.com/uploads/2020/01/23/meta-chart.jpeg)

Input: arr = [6,4,14,6,8,13,9,7,10,6,12], d = 2
Output: 4
Explanation: You can start at index 10. You can jump 10 --> 8 --> 6 --> 7 as shown.
Note that if you start at index 6 you can only jump to index 7. You cannot jump to index 5 because 13 > 9. You cannot jump to index 4 because index 5 is between index 4 and 6 and 13 > 9.
Similarly You cannot jump from index 3 to index 2 or index 1.

Example 2:

Input: arr = [3,3,3,3,3], d = 3
Output: 1
Explanation: You can start at any index. You always cannot jump to any index.

Example 3:

Input: arr = [7,6,5,4,3,2,1], d = 1
Output: 7
Explanation: Start at index 0. You can visit all the indicies.

__Constraints:__

1 <= arr.length <= 1000
1 <= arr[i] <= 10^5
1 <= d <= arr.length

__题目描述：__

给你一个整数数组 arr 和一个整数 d 。每一步你可以从下标 i 跳到：

i + x ，其中 i + x < arr.length 且 0 < x <= d 。
i - x ，其中 i - x >= 0 且 0 < x <= d 。
除此以外，你从下标 i 跳到下标 j 需要满足：arr[i] > arr[j] 且 arr[i] > arr[k] ，其中下标 k 是所有 i 到 j 之间的数字（更正式的，min(i, j) < k < max(i, j)）。

你可以选择数组的任意下标开始跳跃。请你返回你 最多 可以访问多少个下标。

请注意，任何时刻你都不能跳到数组的外面。

__示例：__

示例 1：

![跳跃游戏](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/02/02/meta-chart.jpeg)

输入：arr = [6,4,14,6,8,13,9,7,10,6,12], d = 2
输出：4
解释：你可以从下标 10 出发，然后如上图依次经过 10 --> 8 --> 6 --> 7 。
注意，如果你从下标 6 开始，你只能跳到下标 7 处。你不能跳到下标 5 处因为 13 > 9 。你也不能跳到下标 4 处，因为下标 5 在下标 4 和 6 之间且 13 > 9 。
类似的，你不能从下标 3 处跳到下标 2 或者下标 1 处。

示例 2：

输入：arr = [3,3,3,3,3], d = 3
输出：1
解释：你可以从任意下标处开始且你永远无法跳到任何其他坐标。

示例 3：

输入：arr = [7,6,5,4,3,2,1], d = 1
输出：7
解释：从下标 0 处开始，你可以按照数值从大到小，访问所有的下标。

示例 4：

输入：arr = [7,1,7,1,7,1], d = 2
输出：2

示例 5：

输入：arr = [66], d = 1
输出：1

__提示：__

1 <= arr.length <= 1000
1 <= arr[i] <= 10^5
1 <= d <= arr.length

__思路：__

记忆化搜索
令 dp[i] 表示从下标 i 出发能到达的最多的下标总数
dp[i] = max(dp[j]) + 1
其中 0 <= j < n, abs(i - j) <= d, dp[i] > dp[j1] > dp[j2] > ...
所以每次从 i 开始向左右分别搜索 d 个位置并更新当前的最多下标总数
时间复杂度为 O(nd), 空间复杂度为 O(n)

__代码__:

__C++__:

```C++
class Solution 
{
private:
    void dfs(vector<int>& arr, vector<int>& dp, int cur, int d, int n) 
    {
        if (dp[cur] != -1) return;
        dp[cur] = 1;
        for (int i = cur - 1; i > -1 and cur - i <= d and arr[cur] > arr[i]; i--) 
        {
            dfs(arr, dp, i, d, n);
            dp[cur] = max(dp[cur], dp[i] + 1);
        }
        for (int i = cur + 1; i < n and i - cur <= d and arr[cur] > arr[i]; i++) 
        {
            dfs(arr, dp, i, d, n);
            dp[cur] = max(dp[cur], dp[i] + 1);
        }
    }
public:
    int maxJumps(vector<int>& arr, int d) 
    {
        int n = arr.size();
        vector<int> dp(n, -1);
        for (int i = 0; i < n; ++i) dfs(arr, dp, i, d, n);
        return *max_element(dp.begin(), dp.end());
    }
};
```

__Java__:

```Java
class Solution {
    public int maxJumps(int[] arr, int d) {
        int n = arr.length, dp[] = new int[n], result = 0;
        Arrays.fill(dp, -1);
        for (int i = 0; i < n; i++) dfs(arr, dp, i, d, n);
        for (int i : dp) result = Math.max(i, result);
        return result;
    }
    
    private void dfs(int[] arr, int[] dp, int cur, int d, int n) {
        if (dp[cur] != -1) return;
        dp[cur] = 1;
        for (int i = cur - 1; i > -1 && cur - i <= d && arr[cur] > arr[i]; i--) {
            dfs(arr, dp, i, d, n);
            dp[cur] = Math.max(dp[cur], dp[i] + 1);
        }
        for (int i = cur + 1; i < n && i - cur <= d && arr[cur] > arr[i]; i++) {
            dfs(arr, dp, i, d, n);
            dp[cur] = Math.max(dp[cur], dp[i] + 1);
        }
    }
}
```

__Python__:

```Python
class Solution:
    def maxJumps(self, arr: List[int], d: int) -> int:
        dp = [-1] * (n := len(arr))
        
        def dfs(cur: int) -> None:
            if dp[cur] != -1:
                return
            dp[cur], i = 1, cur - 1
            while i > -1 and cur - i <= d and arr[i] < arr[cur]:
                dfs(i)
                dp[cur] = max(dp[i] + 1, dp[cur])
                i -= 1
            i = cur + 1
            while i < n and i - cur <= d and arr[i] < arr[cur]:
                dfs(i)
                dp[cur] = max(dp[i] + 1, dp[cur])
                i += 1
        
        for i in range(n):
            dfs(i)
        return max(dp)
```
