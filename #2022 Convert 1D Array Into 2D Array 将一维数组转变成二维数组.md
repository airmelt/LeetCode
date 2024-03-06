# 2022 Convert 1D Array Into 2D Array 将一维数组转变成二维数组

__Description:__

You are given a __0-indexed__ 1-dimensional (1D) integer array `original`, and two integers, `m` and `n`. You are tasked with creating a 2-dimensional (2D) array with `m` rows and `n` columns using __all__ the elements from `original`.

The elements from indices `0` to `n - 1` (__inclusive__) of `original` should form the first row of the constructed 2D array, the elements from indices `n` to `2 * n - 1` (__inclusive__) should form the second row of the constructed 2D array, and so on.

Return _an_ `m x n` _2D array constructed according to the above procedure, or an empty 2D array if it is impossible_.

__Example:__

Example 1:

![2022-1](https://assets.leetcode.com/uploads/2021/08/26/image-20210826114243-1.png)

```text
Input: original = [1,2,3,4], m = 2, n = 2
Output: [[1,2],[3,4]]
Explanation: The constructed 2D array should contain 2 rows and 2 columns.
The first group of n=2 elements in original, [1,2], becomes the first row in the constructed 2D array.
The second group of n=2 elements in original, [3,4], becomes the second row in the constructed 2D array.
```

Example 2:

```text
Input: original = [1,2,3], m = 1, n = 3
Output: [[1,2,3]]
Explanation: The constructed 2D array should contain 1 row and 3 columns.
Put all three elements in original into the first row of the constructed 2D array.
```

Example 3:

```text
Input: original = [1,2], m = 1, n = 1
Output: []
Explanation: There are 2 elements in original.
It is impossible to fit 2 elements in a 1x1 2D array, so return an empty 2D array.
```

__Constraints:__

- `1 <= original.length <= 5 * 10 ^ 4`
- `1 <= original[i] <= 10 ^ 5`
- `1 <= m, n <= 4 * 10 ^ 4`

__题目描述:__

给你一个下标从 __0__ 开始的一维整数数组 `original` 和两个整数 `m` 和  `n` 。你需要使用 `original` 中 __所有__ 元素创建一个 `m` 行 `n` 列的二维数组。

`original` 中下标从 `0` 到 `n - 1` （都 __包含__ ）的元素构成二维数组的第一行，下标从 `n` 到 `2 * n - 1` （都 __包含__ ）的元素构成二维数组的第二行，依此类推。

请你根据上述过程返回一个 `m x n` 的二维数组。如果无法构成这样的二维数组，请你返回一个空的二维数组。

__示例:__

示例 1：

![2022-2](https://assets.leetcode.com/uploads/2021/08/26/image-20210826114243-1.png)

```text
输入：original = [1,2,3,4], m = 2, n = 2
输出：[[1,2],[3,4]]
解释：
构造出的二维数组应该包含 2 行 2 列。
original 中第一个 n=2 的部分为 [1,2] ，构成二维数组的第一行。
original 中第二个 n=2 的部分为 [3,4] ，构成二维数组的第二行。
```

示例 2：

```text
输入：original = [1,2,3], m = 1, n = 3
输出：[[1,2,3]]
解释：
构造出的二维数组应该包含 1 行 3 列。
将 original 中所有三个元素放入第一行中，构成要求的二维数组。
```

示例 3：

```text
输入：original = [1,2], m = 1, n = 1
输出：[]
解释：
original 中有 2 个元素。
无法将 2 个元素放入到一个 1x1 的二维数组中，所以返回一个空的二维数组。
```

示例 4：

```text
输入：original = [3], m = 1, n = 2
输出：[]
解释：
original 中只有 1 个元素。
无法将 1 个元素放满一个 1x2 的二维数组，所以返回一个空的二维数组。
```

__提示：__

- `1 <= original.length <= 5 * 10 ^ 4`
- `1 <= original[i] <= 10 ^ 5`
- `1 <= m, n <= 4 * 10 ^ 4`

__思路:__

```text
模拟
首先判断 original 的长度是否等于 m * n, 如果不等于则返回空数组
然后逐行构造二维数组
时间复杂度为 O(M), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<vector<int>> construct2DArray(vector<int>& original, int m, int n) 
    {
        vector<vector<int>> result;
        if (original.size() != m * n) return result;
        for (auto it = original.begin(); it != original.end(); it += n) result.emplace_back(it, it + n);
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int[][] construct2DArray(int[] original, int m, int n) {
        int len = original.length, result[][] = new int[m][n];
        if (len != m * n) return new int[0][];
        for (int i = 0; i < len; i += n) System.arraycopy(original, i, result[i / n], 0, n);
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def construct2DArray(self, original: List[int], m: int, n: int) -> List[List[int]]:
        return [] if len(original) != m * n else [original[x * n:x * n + n] for x in range(m)]
```
