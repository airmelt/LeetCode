# 2300 Successful Pairs of Spells and Potions 咒语和药水的成功对数

__Description:__

You are given two positive integer arrays `spells` and `potions`, of length `n` and `m` respectively, where `spells[i]` represents the strength of the `i ^ th` spell and `potions[j]` represents the strength of the `j ^ th` potion.

You are also given an integer `success`. A spell and potion pair is considered __successful__ if the __product__ of their strengths is __at least__ `success`.

Return _an integer array_ `pairs` _of length_ `n` _where_ `pairs[i]` _is the number of __potions__ that will form a successful pair with the_ `i ^ th` _spell._

__Example:__

Example 1:

```text
Input: spells = [5,1,3], potions = [1,2,3,4,5], success = 7
Output: [4,0,3]
Explanation:
```

- 0th spell: 5 * [1,2,3,4,5] = [5,10,15,20,25]. 4 pairs are successful.
- 1st spell: 1 * [1,2,3,4,5] = [1,2,3,4,5]. 0 pairs are successful.
- 2nd spell: 3 * [1,2,3,4,5] = [3,6,9,12,15]. 3 pairs are successful.

Thus, [4,0,3] is returned.

Example 2:

```text
Input: spells = [3,1,2], potions = [8,5,8], success = 16
Output: [2,0,2]
Explanation:
```

- 0th spell: 3 * [8,5,8] = [24,15,24]. 2 pairs are successful.
- 1st spell: 1 * [8,5,8] = [8,5,8]. 0 pairs are successful.
- 2nd spell: 2 * [8,5,8] = [16,10,16]. 2 pairs are successful.

Thus, [2,0,2] is returned.

__Constraints:__

- `n == spells.length`
- `m == potions.length`
- `1 <= n, m <= 10 ^ 5`
- `1 <= spells[i], potions[i] <= 10 ^ 5`
- `1 <= success <= 10 ^ 10`

__题目描述:__

给你两个正整数数组 `spells` 和 `potions` ，长度分别为 `n` 和 `m` ，其中 `spells[i]` 表示第 `i` 个咒语的能量强度， `potions[j]` 表示第 `j` 瓶药水的能量强度。

同时给你一个整数 `success` 。一个咒语和药水的能量强度 __相乘__ 如果 __大于等于__ `success` ，那么它们视为一对 __成功__ 的组合。

请你返回一个长度为 `n` 的整数数组 `pairs`，其中 `pairs[i]` 是能跟第 `i` 个咒语成功组合的 _药水_ 数目。

__示例:__

示例 1：

```text
输入：spells = [5,1,3], potions = [1,2,3,4,5], success = 7
输出：[4,0,3]
解释：
```

- 第 0 个咒语：5 * [1,2,3,4,5] = [5,10,15,20,25] 。总共 4 个成功组合。
- 第 1 个咒语：1 * [1,2,3,4,5] = [1,2,3,4,5] 。总共 0 个成功组合。
- 第 2 个咒语：3 * [1,2,3,4,5] = [3,6,9,12,15] 。总共 3 个成功组合。

所以返回 [4,0,3] 。

示例 2：

```text
输入：spells = [3,1,2], potions = [8,5,8], success = 16
输出：[2,0,2]
解释：
```

- 第 0 个咒语：3 * [8,5,8] = [24,15,24] 。总共 2 个成功组合。
- 第 1 个咒语：1 * [8,5,8] = [8,5,8] 。总共 0 个成功组合。
- 第 2 个咒语：2 * [8,5,8] = [16,10,16] 。总共 2 个成功组合。

所以返回 [2,0,2] 。

__提示：__

- `n == spells.length`
- `m == potions.length`
- `1 <= n, m <= 10 ^ 5`
- `1 <= spells[i], potions[i] <= 10 ^ 5`
- `1 <= success <= 10 ^ 10`

__思路:__

```text
排序 ➕ 二分
对药水的能量强度进行排序
遍历咒语的能量强度, 对每个咒语能够成功组合的药水数目进行计算
使用二分查找找到能够成功组合的药水的最小下标
注意 long 的范围
可以使用乘法代替除法防止精度的问题
时间复杂度为 O(NlogM + MlogM), 空间复杂度为 O(logM)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<int> successfulPairs(vector<int>& spells, vector<int>& potions, long long success) 
    {
        sort(potions.begin(), potions.end());
        for (int &x : spells) x = (success - 1LL) / x < potions.back() ? potions.end() - ranges::upper_bound(potions, (int)((success - 1LL) / (long long)x)) : 0;
        return spells;
    }
};
```

__Java__:

```Java
class Solution {
    public int[] successfulPairs(int[] spells, int[] potions, long success) {
        int n = spells.length, m = potions.length, result[] = new int[n], idx = 0;
        Arrays.sort(potions);
        for (int i = 0; i < n; i++) result[i] = m - helper(potions, ((double)success / spells[i]));
        return result;
    }

    private int helper(int[] nums, double target) {
        int l = 0, r = nums.length, mid = 0;
        while (l < r) {
            if (nums[mid = (l + r) >>> 1] >= target) r = mid;
            else l = mid + 1;
        }
        return l;
    }
}
```

__Python__:

```Python
class Solution:
    def successfulPairs(self, spells: List[int], potions: List[int], success: int) -> List[int]:
        n, m = len(spells), len(potions)
        potions.sort()
        result = [0] * n
        for i in range(n):
            result[i] = m - bisect_left(potions, success // spells[i] + (success % spells[i] != 0))
        return result
```
