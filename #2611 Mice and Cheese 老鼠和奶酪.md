# 2611 Mice and Cheese 老鼠和奶酪

__Description:__

There are two mice and `n` different types of cheese, each type of cheese should be eaten by exactly one mouse.

A point of the cheese with index `i` (__0-indexed__) is:

- `reward1[i]` if the first mouse eats it.
- `reward2[i]` if the second mouse eats it.

You are given a positive integer array `reward1`, a positive integer array `reward2`, and a non-negative integer `k`.

Return ___the maximum__ points the mice can achieve if the first mouse eats exactly_ `k` _types of cheese._

__Example:__

Example 1:

```text
Input: reward1 = [1,1,3,4], reward2 = [4,4,1,1], k = 2
Output: 15
Explanation: In this example, the first mouse eats the 2nd (0-indexed) and the 3rd types of cheese, and the second mouse eats the 0th and the 1st types of cheese.
The total points are 4 + 4 + 3 + 4 = 15.
It can be proven that 15 is the maximum total points that the mice can achieve.
```

Example 2:

```text
Input: reward1 = [1,1], reward2 = [1,1], k = 2
Output: 2
Explanation: In this example, the first mouse eats the 0th (0-indexed) and 1st types of cheese, and the second mouse does not eat any cheese.
The total points are 1 + 1 = 2.
It can be proven that 2 is the maximum total points that the mice can achieve.
```

__Constraints:__

- `1 <= n == reward1.length == reward2.length <= 10 ^ 5`
- `1 <= reward1[i], reward2[i] <= 1000`
- `0 <= k <= n`

__题目描述:__

有两只老鼠和 `n` 块不同类型的奶酪，每块奶酪都只能被其中一只老鼠吃掉。

下标为 `i` 处的奶酪被吃掉的得分为:

- 如果第一只老鼠吃掉，则得分为 `reward1[i]` 。
- 如果第二只老鼠吃掉，则得分为 `reward2[i]` 。

给你一个正整数数组 `reward1` ，一个正整数数组 `reward2` ，和一个非负整数 `k` 。

请你返回第一只老鼠恰好吃掉 `k` 块奶酪的情况下，__最大__ 得分为多少。

__示例:__

示例 1：

```text
输入：reward1 = [1,1,3,4], reward2 = [4,4,1,1], k = 2
输出：15
解释：这个例子中，第一只老鼠吃掉第 2 和 3 块奶酪（下标从 0 开始），第二只老鼠吃掉第 0 和 1 块奶酪。
总得分为 4 + 4 + 3 + 4 = 15 。
15 是最高得分。
```

示例 2：

```text
输入：reward1 = [1,1], reward2 = [1,1], k = 2
输出：2
解释：这个例子中，第一只老鼠吃掉第 0 和 1 块奶酪（下标从 0 开始），第二只老鼠不吃任何奶酪。
总得分为 1 + 1 = 2 。
2 是最高得分。
```

__提示：__

- `1 <= n == reward1.length == reward2.length <= 10 ^ 5`
- `1 <= reward1[i], reward2[i] <= 1000`
- `0 <= k <= n`

__思路:__

```text
贪心
假设都吃第二个老鼠的奶酪, 那么总共得分为 sum(reward2)
然后我们让第一只老鼠在第一个老鼠的奶酪中选择 k 个最大的奶酪
将 reward1 中的每个元素减去 reward2 中对应的元素, 得到 reward1[i] - reward2[i]
并排序或者快速选择前 k 个最大的元素
最后将 reward2 中的所有元素加上 reward1 中最大的 k 个元素的和
这样就可以得到最大得分
时间复杂度为 O(NlogN), 空间复杂度为 O(logN)
如果使用快速选择
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int miceAndCheese(vector<int>& reward1, vector<int>& reward2, int k) 
    {
        for (int i = 0, n = reward1.size(); i < n; i++) reward1[i] -= reward2[i];
        ranges::nth_element(reward1, reward1.end() - k);
        return reduce(reward2.begin(), reward2.end()) + reduce(reward1.end() - k, reward1.end());
    }
};
```

__Java__:

```Java
class Solution {
    public int miceAndCheese(int[] reward1, int[] reward2, int k) {
        int result = (int)Arrays.stream(reward2).sum(), n = reward1.length;
        for (int i = 0; i < n; i++) reward1[i] -= reward2[i];
        Arrays.sort(reward1);
        for (int i = n - k; i < n; i++) result += reward1[i];
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def miceAndCheese(self, reward1: List[int], reward2: List[int], k: int) -> int:
        return sum(reward2) + sum(sorted([r1 - r2 for r1, r2 in zip(reward1, reward2)], reverse=True)[:k])
```
