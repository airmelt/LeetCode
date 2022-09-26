# 1326 Minimum Number of Taps to Open to Water a Garden 灌溉花园的最少水龙头数目

__Description:__

There is a one-dimensional garden on the x-axis. The garden starts at the point 0 and ends at the point n. (i.e The length of the garden is n).

There are n + 1 taps located at points [0, 1, ..., n] in the garden.

Given an integer n and an integer array ranges of length n + 1 where ranges[i] (0-indexed) means the i-th tap can water the area [i - ranges[i], i + ranges[i]] if it was open.

Return the minimum number of taps that should be open to water the whole garden, If the garden cannot be watered return -1.

__Example:__

Example 1:

![Taps](https://assets.leetcode.com/uploads/2020/01/16/1685_example_1.png)

Input: n = 5, ranges = [3,4,1,1,0,0]
Output: 1
Explanation: The tap at point 0 can cover the interval [-3,3]
The tap at point 1 can cover the interval [-3,5]
The tap at point 2 can cover the interval [1,3]
The tap at point 3 can cover the interval [2,4]
The tap at point 4 can cover the interval [4,4]
The tap at point 5 can cover the interval [5,5]
Opening Only the second tap will water the whole garden [0,5]

Example 2:

Input: n = 3, ranges = [0,0,0,0]
Output: -1
Explanation: Even if you activate all the four taps you cannot water the whole garden.

__Constraints:__

1 <= n <= 10^4
ranges.length == n + 1
0 <= ranges[i] <= 100

__题目描述：__

在 x 轴上有一个一维的花园。花园长度为 n，从点 0 开始，到点 n 结束。

花园里总共有 n + 1 个水龙头，分别位于 [0, 1, ..., n] 。

给你一个整数 n 和一个长度为 n + 1 的整数数组 ranges ，其中 ranges[i] （下标从 0 开始）表示：如果打开点 i 处的水龙头，可以灌溉的区域为 [i -  ranges[i], i + ranges[i]] 。

请你返回可以灌溉整个花园的 最少水龙头数目 。如果花园始终存在无法灌溉到的地方，请你返回 -1 。

__示例：__

示例 1：

![水龙头](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/01/19/1685_example_1.png)

输入：n = 5, ranges = [3,4,1,1,0,0]
输出：1
解释：
点 0 处的水龙头可以灌溉区间 [-3,3]
点 1 处的水龙头可以灌溉区间 [-3,5]
点 2 处的水龙头可以灌溉区间 [1,3]
点 3 处的水龙头可以灌溉区间 [2,4]
点 4 处的水龙头可以灌溉区间 [4,4]
点 5 处的水龙头可以灌溉区间 [5,5]
只需要打开点 1 处的水龙头即可灌溉整个花园 [0,5] 。

示例 2：

输入：n = 3, ranges = [0,0,0,0]
输出：-1
解释：即使打开所有水龙头，你也无法灌溉整个花园。

__提示：__

1 <= n <= 10^4
ranges.length == n + 1
0 <= ranges[i] <= 100

__思路：__

贪心
将所有区间按照起点排序
选择当前起点能够到达的最右边的的区间加入结果
时间复杂度为 O(nlgn), 空间复杂度为 O(n)

__代码__:

__C++__:

```C++
class Solution 
{
public:
    int minTaps(int n, vector<int>& ranges) 
    {
        vector<pair<int, int>> taps(n + 1);
        for (int i = 0; i <= n; i++)
        {
            taps[i].first = i - ranges[i];
            taps[i].second = i + ranges[i];
        }
        sort(taps.begin(), taps.end());
        int result = 0, right = 0, left = 0;
        while (right < n)
        {
            int cur = -1;
            while (left <= n and taps[left].first <= right) cur = max(cur, taps[left++].second);
            if (cur == -1) return -1;
            ++result;
            right = cur;
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int minTaps(int n, int[] ranges) {
        int taps[][] = new int[n + 1][2], result = 0, right = 0, left = 0;
        for (int i = 0; i <= n; i++) {
            taps[i][0] = i - ranges[i];
            taps[i][1] = i + ranges[i];
        }
        Arrays.sort(taps, (a, b) -> a[0] - b[0]);
        while (right < n) {
            int cur = -1;
            while (left <= n && taps[left][0] <= right) cur = Math.max(cur, taps[left++][1]);
            if (cur == -1) return -1;
            ++result;
            right = cur;
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def minTaps(self, n: int, ranges: List[int]) -> int:
        taps, left, right, result = sorted([[i - ranges[i], i + ranges[i]] for i in range(n + 1)]), 0, 0, 0
        while right < n:
            cur = -1
            while left <= n and taps[left][0] <= right:
                cur = max(cur, taps[left][1])
                left += 1
            if cur == -1:
                return -1
            result += 1
            right = cur
        return result
```
