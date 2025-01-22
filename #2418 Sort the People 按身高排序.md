# 2418 Sort the People 按身高排序

__Description:__

You are given an array of strings `names`, and an array `heights` that consists of __distinct__ positive integers. Both arrays are of length `n`.

For each index `i`, `names[i]` and `heights[i]` denote the name and height of the `i ^ th` person.

Return `names` _sorted in __descending__ order by the people's heights_.

__Example:__

Example 1:

```text
Input: names = ["Mary","John","Emma"], heights = [180,165,170]
Output: ["Mary","Emma","John"]
Explanation: Mary is the tallest, followed by Emma and John.
```

Example 2:

```text
Input: names = ["Alice","Bob","Bob"], heights = [155,185,150]
Output: ["Bob","Alice","Bob"]
Explanation: The first Bob is the tallest, followed by Alice and the second Bob.
```

__Constraints:__

- `n == names.length == heights.length`
- `1 <= n <= 10 ^ 3`
- `1 <= names[i].length <= 20`
- `1 <= heights[i] <= 10 ^ 5`
- `names[i]` consists of lower and upper case English letters.
- All the values of `heights` are distinct.

__题目描述:__

给你一个字符串数组 `names` ，和一个由 __互不相同__ 的正整数组成的数组 `heights` 。两个数组的长度均为 `n` 。

对于每个下标 `i`， `names[i]` 和 `heights[i]` 表示第 `i` 个人的名字和身高。

请按身高 __降序__ 顺序返回对应的名字数组 `names` 。

__示例:__

示例 1：

```text
输入：names = ["Mary","John","Emma"], heights = [180,165,170]
输出：["Mary","Emma","John"]
解释：Mary 最高，接着是 Emma 和 John 。
```

示例 2：

```text
输入：names = ["Alice","Bob","Bob"], heights = [155,185,150]
输出：["Bob","Alice","Bob"]
解释：第一个 Bob 最高，然后是 Alice 和第二个 Bob 。
```

__提示：__

- `n == names.length == heights.length`
- `1 <= n <= 10 ^ 3`
- `1 <= names[i].length <= 20`
- `1 <= heights[i] <= 10 ^ 5`
- `names[i]` 由大小写英文字母组成
- `heights` 中的所有值互不相同

__思路:__

```text
模拟
给出下标数组
根据高度给下标数组排序
将下标数组对应的名字数组返回
这样可以不打乱原来的名字数组
时间复杂度为 O(NlogN), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<string> sortPeople(vector<string>& names, vector<int>& heights) 
    {
        int n = names.size(), idx[n];
        iota(idx, idx + n, 0);
        sort(idx, idx + n, [&](const auto &i, const auto &j) { return heights[i] > heights[j]; });
        vector<string> result(n);
        for (int i = 0; i < n; i++) result[i] = names[idx[i]];
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public String[] sortPeople(String[] names, int[] heights) {
        int n = names.length;
        var idx = new Integer[n];
        for (int i = 0; i < n; i++) idx[i] = i;
        Arrays.sort(idx, (i, j) -> heights[j] - heights[i]);
        var result = new String[n];
        for (int i = 0; i < n; i++) result[i] = names[idx[i]];
        return result;
    }
}
```

__Python__:

```Python
class Solution 
{
public:
    vector<string> sortPeople(vector<string>& names, vector<int>& heights) 
    {
        int n = names.size(), idx[n];
        iota(idx, idx + n, 0);
        sort(idx, idx + n, [&](const auto &i, const auto &j) { return heights[i] > heights[j]; });
        vector<string> result(n);
        for (int i = 0; i < n; i++) result[i] = names[idx[i]];
        return result;
    }
};
```
