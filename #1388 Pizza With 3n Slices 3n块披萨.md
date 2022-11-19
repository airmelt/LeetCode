# 1388 Pizza With 3n Slices 3n块披萨

__Description:__

There is a pizza with 3n slices of varying size, you and your friends will take slices of pizza as follows:

You will pick any pizza slice.
Your friend Alice will pick the next slice in the anti-clockwise direction of your pick.
Your friend Bob will pick the next slice in the clockwise direction of your pick.
Repeat until there are no more slices of pizzas.

Given an integer array slices that represent the sizes of the pizza slices in a clockwise direction, return the maximum possible sum of slice sizes that you can pick.

__Example:__

Example 1:

![1388-1](https://assets.leetcode.com/uploads/2020/02/18/sample_3_1723.png)

Input: slices = [1,2,3,4,5,6]
Output: 10
Explanation: Pick pizza slice of size 4, Alice and Bob will pick slices with size 3 and 5 respectively. Then Pick slices with size 6, finally Alice and Bob will pick slice of size 2 and 1 respectively. Total = 4 + 6.

Example 2:

![1388-2](https://assets.leetcode.com/uploads/2020/02/18/sample_4_1723.png)

Input: slices = [8,9,8,6,1,1]
Output: 16
Explanation: Pick pizza slice of size 8 in each turn. If you pick slice with size 9 your partners will pick slices of size 8.

__Constraints:__

3 * n == slices.length
1 <= slices.length <= 500
1 <= slices[i] <= 1000

__题目描述:__

给你一个披萨，它由 3n 块不同大小的部分组成，现在你和你的朋友们需要按照如下规则来分披萨：

你挑选 任意 一块披萨。
Alice 将会挑选你所选择的披萨逆时针方向的下一块披萨。
Bob 将会挑选你所选择的披萨顺时针方向的下一块披萨。
重复上述过程直到没有披萨剩下。

每一块披萨的大小按顺时针方向由循环数组 slices 表示。

请你返回你可以获得的披萨大小总和的最大值。

__示例:__

示例 1：

![1388-3](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/03/21/sample_3_1723.png)

输入：slices = [1,2,3,4,5,6]
输出：10
解释：选择大小为 4 的披萨，Alice 和 Bob 分别挑选大小为 3 和 5 的披萨。然后你选择大小为 6 的披萨，Alice 和 Bob 分别挑选大小为 2 和 1 的披萨。你获得的披萨总大小为 4 + 6 = 10 。

示例 2：

![1388-4](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/03/21/sample_4_1723.png)

输入：slices = [8,9,8,6,1,1]
输出：16
解释：两轮都选大小为 8 的披萨。如果你选择大小为 9 的披萨，你的朋友们就会选择大小为 8 的披萨，这种情况下你的总和不是最大的。

__提示：__

1 <= slices.length <= 500
slices.length % 3 == 0
1 <= slices[i] <= 1000

__思路:__

```text
注意到顺时针和逆时针会各取一个
实际上题意可转化为从 3n 个数中选择不相邻的 n 个数的最大和

1. 动态规划
设 dp[i][j] 表示从前 i 个数中取 j 个数的最大和
dp[i][j] = max(slices[:3]), if i <= 2
dp[i][j] = max(slices[i - 1] + dp[i - 2][j - 1], dp[i - 1][j])
即要么取 1 个数, 这个数不能与当前的数相邻, 要么不取数, 直接使用相邻的结果, 取两者较大值即可
最后返回 dp[-1][-1]
为了简化环的操作, 可以使用一个技巧
计算两次, 第一次不取最后一个数, 第二次不取第一个数
这样就能保证第一个数和最后一个数不同时被选取, 保证了环中相邻的数不会被选择
注意到 dp[i] 只与 dp[i - 1], dp[i - 2] 有关, 可以使用滚动数组的方式将空间复杂度降至 O(N)
时间复杂度为 O(N ^ 2), 空间复杂度为 O(N)
2. 贪心
如果每次选择环形中最大的数, 并删除相邻的两个数, 可以得到一个答案, 但是这个是不正确的
比如, [8, 9, 8, 6, 1, 2], 选择 9, 6 是小于 8, 8 的
因为选择了 9 之后, 再也不能反悔选择 8, 8 已经被删除
要保留两个 8 的选项供以后反悔
可以将差值保留在原地, 反复操作直到选择够了 n / 3 个数
选了 9 之后将 8 + 8 - 9 = 7 放在 9 的位置并删除 8, 即 [7, 6, 1, 2]
这样就能得到正确答案 16
用两个数组模拟双向链表并用优先队列记录最大值
为了保证优先队列中记录的都是有效值, 用一个布尔数组记录删除的下标
时间复杂度为 O(NlgN), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
private:
    int helper(vector<int> s)
    {
        int n = s.size(), choose = (n + 1) / 3;
        vector<vector<int>> dp(n + 1, vector<int>(choose + 1, 0));
        for (int i = 1; i <= n; i++) for (int j = 1; j <= choose; j++) dp[i][j] = max(dp[i - 1][j], (i - 2 >= 0 ? dp[i - 2][j - 1] : 0) + s[i - 1]);
        return dp.back().back();
    }
public:
    int maxSizeSlices(vector<int>& slices) {
        return max(helper(vector<int>(slices.begin() + 1, slices.end())), helper(vector<int>(slices.begin(), slices.end() - 1)));
    }
};
```

__Java__:

```Java
class Solution {
    public int maxSizeSlices(int[] slices) {
        int result = 0, n = slices.length, left[] = new int[n], right[] = new int[n];
        boolean[] visited = new boolean[n];
        PriorityQueue<int[]> pq = new PriorityQueue<int[]>(new Comparator<int[]>() {
            public int compare(int[] a1, int[] a2) {
                return a1[0] == a2[0] ? a2[1] - a1[1] : a2[0] - a1[0];
            }
        });
        for (int i = 0; i < n; i++) {
            left[i] = i == 0 ? n - 1 : i - 1;
            right[i] = i == n - 1 ? 0 : i + 1;
            pq.offer(new int[]{slices[i], i});
        }
        for (int j = 0; j < n / 3; j++) {
            while (visited[pq.peek()[1]]) pq.poll();
            int pos = pq.poll()[1];
            result += slices[pos];
            slices[pos] = slices[left[pos]] + slices[right[pos]] - slices[pos];
            pq.offer(new int[]{slices[pos], pos});
            visited[left[pos]] = visited[right[pos]] = true;
            left[right[right[pos]]] = right[left[left[pos]]] = pos;
            left[pos] = left[left[pos]];
            right[pos] = right[right[pos]];
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def maxSizeSlices(self, slices: List[int]) -> int:
        n = len(slices)
        
        def helper(s: List[int]) -> int:
            dp = [[0] * (n // 3 + 1) for _ in range(n)]
            for i in range(1, n):
                for j in range(1, n // 3 + 1):
                    dp[i][j] = max(dp[i - 1][j], (dp[i - 2][j - 1] if i - 2 >= 0 else 0) + s[i - 1])
            return dp[-1][-1]
        return max(helper(slices[1:]), helper(slices[:-1]))
```
