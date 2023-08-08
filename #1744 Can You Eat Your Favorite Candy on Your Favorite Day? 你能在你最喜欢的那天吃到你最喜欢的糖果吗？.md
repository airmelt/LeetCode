# 1744 Can You Eat Your Favorite Candy on Your Favorite Day? 你能在你最喜欢的那天吃到你最喜欢的糖果吗？

__Description:__

You are given a __(0-indexed)__ array of positive integers `candiesCount` where `candiesCount[i]` represents the number of candies of the `i ^ th` type you have. You are also given a 2D array `queries` where `queries[i] = [favoriteTypei, favoriteDayi, dailyCapi]`.

You play a game with the following rules:

- You start eating candies on day `__0__`.
- You _cannot_ eat __any__ candy of type `i` unless you have eaten __all__ candies of type `i - 1`.
- You must eat __at least__ __one__ candy per day until you have eaten all the candies.

Construct a boolean array `answer` such that `answer.length == queries.length` and `answer[i]` is `true` if you can eat a candy of type `favoriteTypei` on day `favoriteDayi` without eating __more than__ `dailyCapi` candies on __any__ day, and `false` otherwise. Note that you can eat different types of candy on the same day, provided that you follow rule 2.

Return _the constructed array_ `answer`.

__Example:__

Example 1:

```text
Input: candiesCount = [7,4,5,3,8], queries = [[0,2,2],[4,2,4],[2,13,1000000000]]
Output: [true,false,true]
Explanation:
1- If you eat 2 candies (type 0) on day 0 and 2 candies (type 0) on day 1, you will eat a candy of type 0 on day 2.
2- You can eat at most 4 candies each day.
   If you eat 4 candies every day, you will eat 4 candies (type 0) on day 0 and 4 candies (type 0 and type 1) on day 1.
   On day 2, you can only eat 4 candies (type 1 and type 2), so you cannot eat a candy of type 4 on day 2.
3- If you eat 1 candy each day, you will eat a candy of type 2 on day 13.
```

Example 2:

```text
Input: candiesCount = [5,2,6,4,1], queries = [[3,1,2],[4,10,3],[3,10,100],[4,100,30],[1,3,1]]
Output: [false,true,true,false,false]
```

__Constraints:__

- `1 <= candiesCount.length <= 10 ^ 5`
- `1 <= candiesCount[i] <= 10 ^ 5`
- `1 <= queries.length <= 10 ^ 5`
- `queries[i].length == 3`
- `0 <= favoriteTypei < candiesCount.length`
- `0 <= favoriteDayi <= 10 ^ 9`
- `1 <= dailyCapi <= 10 ^ 9`

__题目描述:__

给你一个下标从 __0__ 开始的正整数数组 `candiesCount` ，其中 `candiesCount[i]` 表示你拥有的第 `i` 类糖果的数目。同时给你一个二维数组 `queries` ，其中 `queries[i] = [favoriteTypei, favoriteDayi, dailyCapi]` 。

你按照如下规则进行一场游戏：

- 你从第 `__0__` 天开始吃糖果。
- 你在吃完 __所有__ 第 `i - 1` 类糖果之前，__不能__ 吃任何一颗第 `i` 类糖果。
- 在吃完所有糖果之前，你必须每天 __至少__ 吃 __一颗__ 糖果。

请你构建一个布尔型数组 `answer` ，用以给出 `queries` 中每一项的对应答案。此数组满足:

- `answer.length == queries.length` 。 `answer[i]` 是 `queries[i]` 的答案。
- `answer[i]` 为 `true` 的条件是:在每天吃 __不超过__ `dailyCapi` 颗糖果的前提下，你可以在第 `favoriteDayi` 天吃到第 `favoriteTypei` 类糖果；否则 `answer[i]` 为 `false` 。

注意，只要满足上面 3 条规则中的第二条规则，你就可以在同一天吃不同类型的糖果。

请你返回得到的数组 `answer` 。

__示例:__

示例 1：

__输入：candiesCount = [7,4,5,3,8], queries = [[0,2,2],[4,2,4],[2,13,1000000000]]
输出：[true,false,true]
提示：
1- 在第 0 天吃 2 颗糖果(类型 0），第 1 天吃 2 颗糖果（类型 0），第 2 天你可以吃到类型 0 的糖果。
2- 每天你最多吃 4 颗糖果。即使第 0 天吃 4 颗糖果（类型 0），第 1 天吃 4 颗糖果（类型 0 和类型 1），你也没办法在第 2 天吃到类型 4 的糖果。换言之，你没法在每天吃 4 颗糖果的限制下在第 2 天吃到第 4 类糖果。
3- 如果你每天吃 1 颗糖果，你可以在第 13 天吃到类型 2 的糖果。__

示例 2：

```text
输入：candiesCount = [5,2,6,4,1], queries = [[3,1,2],[4,10,3],[3,10,100],[4,100,30],[1,3,1]]
输出：[false,true,true,false,false]
```

__提示：__

- `1 <= candiesCount.length <= 10 ^ 5`
- `1 <= candiesCount[i] <= 10 ^ 5`
- `1 <= queries.length <= 10 ^ 5`
- `queries[i].length == 3`
- `0 <= favoriteTypei < candiesCount.length`
- `0 <= favoriteDayi <= 10 ^ 9`
- `1 <= dailyCapi <= 10 ^ 9`

__思路:__

```text
前缀和
先求出所有糖果的前缀和
对于 queries 每一个 q, 判断是否满足 q[1] < pre[q[0] + 1] and (q[1] + 1) * q[2] > pre[q[0]] 即可
时间复杂度为 O(N + Q), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<bool> canEat(vector<int>& candiesCount, vector<vector<int>>& queries) 
    {
        int m = candiesCount.size(), n = queries.size();
        vector<long long> pre(m + 1);
        for (int i = 0; i < m; i++) pre[i + 1] = pre[i] + candiesCount[i];
        vector<bool> result(n);
        for (int i = 0; i < n; i++) result[i] = (long long)queries[i][1] < pre[queries[i][0] + 1] && (long long)(queries[i][1] + 1) * (long long)queries[i][2] > pre[queries[i][0]];
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public boolean[] canEat(int[] candiesCount, int[][] queries) {
        int m = candiesCount.length, n = queries.length;
        long pre[] = new long[m + 1];
        for (int i = 0; i < m; i++) pre[i + 1] = pre[i] + candiesCount[i];
        boolean[] result = new boolean[n];
        for (int i = 0; i < n; i++) result[i] = (long)queries[i][1] < pre[queries[i][0] + 1] && (long)(queries[i][1] + 1) * (long)queries[i][2] > pre[queries[i][0]];
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def canEat(self, candiesCount: List[int], queries: List[List[int]]) -> List[bool]:
        pre, result = list(accumulate([0] + candiesCount)), [False] * len(queries)
        for i, q in enumerate(queries):
            result[i] = q[1] < pre[q[0] + 1] and (q[1] + 1) * q[2] > pre[q[0]]
        return result
```
