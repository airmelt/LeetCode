# 2961 Double Modular Exponentiation 双模幂运算

__Description:__

You are given a __0-indexed__ 2D array `variables` where `variables[i] = [ai, bi, ci, mi]`, and an integer `target`.

An index `i` is __good__ if the following formula holds:

- `0 <= i < variables.length`
- `((ai ^ bi % 10) ^ ci) % mi == target`

Return an array consisting of good indices in any order.

__Example:__

Example 1:

```text
Input: variables = [[2,3,3,10],[3,3,3,1],[6,1,1,4]], target = 2
Output: [0,2]
Explanation: For each index i in the variables array:
1) For the index 0, variables[0] = [2,3,3,10], (23 % 10)3 % 10 = 2.
2) For the index 1, variables[1] = [3,3,3,1], (33 % 10)3 % 1 = 0.
3) For the index 2, variables[2] = [6,1,1,4], (61 % 10)1 % 4 = 2.
Therefore we return [0,2] as the answer.
```

Example 2:

```text
Input: variables = [[39,3,1000,1000]], target = 17
Output: []
Explanation: For each index i in the variables array:
1) For the index 0, variables[0] = [39,3,1000,1000], (393 % 10)1000 % 1000 = 1.
Therefore we return [] as the answer.
```

__Constraints:__

- `1 <= variables.length <= 100`
- `variables[i] == [ai, bi, ci, mi]`
- `1 <= ai, bi, ci, mi <= 10 ^ 3`
- `0 <= target <= 10 ^ 3`

__题目描述:__

给你一个下标从 __0__ 开始的二维数组 `variables` ，其中 `variables[i] = [ai, bi, ci, mi]`，以及一个整数 `target` 。

如果满足以下公式，则下标 `i` 是 __好下标__:

- `0 <= i < variables.length`
- `((ai ^ bi % 10) ^ ci) % mi == target`

返回一个由 好下标 组成的数组，顺序不限 。

__示例:__

示例 1：

```text
输入：variables = [[2,3,3,10],[3,3,3,1],[6,1,1,4]], target = 2
输出：[0,2]
解释：对于 variables 数组中的每个下标 i ：
1) 对于下标 0 ，variables[0] = [2,3,3,10] ，(23 % 10)3 % 10 = 2 。
2) 对于下标 1 ，variables[1] = [3,3,3,1] ，(33 % 10)3 % 1 = 0 。
3) 对于下标 2 ，variables[2] = [6,1,1,4] ，(61 % 10)1 % 4 = 2 。
因此，返回 [0,2] 作为答案。
```

示例 2：

```text
输入：variables = [[39,3,1000,1000]], target = 17
输出：[]
解释：对于 variables 数组中的每个下标 i ：
1) 对于下标 0 ，variables[0] = [39,3,1000,1000] ，(393 % 10)1000 % 1000 = 1 。
因此，返回 [] 作为答案。
```

__提示：__

- `1 <= variables.length <= 100`
- `variables[i] == [ai, bi, ci, mi]`
- `1 <= ai, bi, ci, mi <= 10 ^ 3`
- `0 <= target <= 10 ^ 3`

__思路:__

```text
快速幂
按照题意检查快速幂的结果是否为 target 即可
时间复杂度为 O(NlogM), 空间复杂度为 O(1), M 为 variables 数组的 bi 和 ci 的最大值, 这里为 10 ^ 3
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<int> getGoodIndices(vector<vector<int>>& variables, int target) 
    {
        vector<int> result;
        auto quick_pow = [](int a, int n, int mod) -> int
        {
            int result = 1;
            while (n > 0) 
            {
                if (n & 1) result = result * a % mod;
                a = a * a % mod;
                n >>= 1;
            }
            return result;
        };
        for (int i = 0, n = variables.size(); i < n; i++) if (quick_pow(quick_pow(variables[i][0], variables[i][1], 10), variables[i][2], variables[i][3]) == target) result.emplace_back(i);
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public List<Integer> getGoodIndices(int[][] variables, int target) {
        List<Integer> result = new ArrayList<>();
        for (int i = 0, n = variables.length; i < n; i++) if (quickPow(quickPow(variables[i][0], variables[i][1], 10), variables[i][2], variables[i][3]) == target) result.add(i);
        return result;
    }

    private int quickPow(int a, int n, int mod) {
        int result = 1;
        while (n > 0) {
            if ((n & 1) == 1) result = result * a % mod;
            a = a * a % mod;
            n >>>= 1;
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def getGoodIndices(self, variables: List[List[int]], target: int) -> List[int]:
        return [i for i, (a, b, c, m) in enumerate(variables) if pow(pow(a, b, 10), c, m) == target]
```
