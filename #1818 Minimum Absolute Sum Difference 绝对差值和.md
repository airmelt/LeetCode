# 1818 Minimum Absolute Sum Difference 绝对差值和

__Description:__

You are given two positive integer arrays `nums1` and `nums2`, both of length `n`.

The __absolute sum difference__ of arrays `nums1` and `nums2` is defined as the __sum__ of `|nums1[i] - nums2[i]|` for each `0 <= i < n` (__0-indexed__).

You can replace __at most one__ element of `nums1` with __any__ other element in `nums1` to __minimize__ the absolute sum difference.

Return the _minimum absolute sum difference __after__ replacing at most one element in the array `nums1`._ Since the answer may be large, return it __modulo__ `10 ^ 9 + 7`.

`|x|` is defined as:

- `x` if `x >= 0`, or
- `-x` if `x < 0`.

__Example:__

Example 1:

```text
Input:  nums1 = [1,7,5], nums2 = [2,3,5]
Output:  3
Explanation:  There are two possible optimal solutions:
- Replace the second element with the first: [1,7,5] => [1,1,5], or
- Replace the second element with the third: [1,7,5] => [1,5,5].
Both will yield an absolute sum difference of `|1-2| + (|1-3| or |5-3|) + |5-5| =` 3.
```

Example 2:

```text
Input: nums1 = [2,4,6,8,10], nums2 = [2,4,6,8,10]
Output: 0
Explanation: nums1 is equal to nums2 so no replacement is needed. This will result in an 
absolute sum difference of 0.
```

Example 3:

```text
Input:  nums1 = [1,10,4,4,2,7], nums2 = [9,3,5,1,7,4]
Output:  20
Explanation:  Replace the first element with the second: [1,10,4,4,2,7] => [10,10,4,4,2,7].
This yields an absolute sum difference of `|10-9| + |10-3| + |4-5| + |4-1| + |2-7| + |7-4| = 20`
```

__Constraints:__

- `n == nums1.length`
- `n == nums2.length`
- `1 <= n <= 10 ^ 5`
- `1 <= nums1[i], nums2[i] <= 10 ^ 5`

__题目描述:__

给你两个正整数数组 `nums1` 和 `nums2` ，数组的长度都是 `n` 。

数组 `nums1` 和 `nums2` 的 __绝对差值和__ 定义为所有 `|nums1[i] - nums2[i]|`（ `0 <= i < n`）的 __总和__（__下标从 0 开始__）。

你可以选用 `nums1` 中的 __任意一个__ 元素来替换 `nums1` 中的 __至多__ 一个元素，以 __最小化__ 绝对差值和。

在替换数组 `nums1` 中最多一个元素 __之后__ ，返回最小绝对差值和。因为答案可能很大，所以需要对 `10 ^ 9 + 7` __取余__ 后返回。

`|x|` 定义为:

- 如果 `x >= 0` ，值为 `x` ，或者
- 如果 `x <= 0` ，值为 `-x`

__示例:__

示例 1：

```text
输入: nums1 = [1,7,5], nums2 = [2,3,5]
输出: 3
解释: 有两种可能的最优方案:
- 将第二个元素替换为第一个元素:[1,7,5] => [1,1,5] ，或者
- 将第二个元素替换为第三个元素:[1,7,5] => [1,5,5]
两种方案的绝对差值和都是 `|1-2| + (|1-3| 或者 |5-3|) + |5-5| =` 3
```

示例 2：

```text
输入：nums1 = [2,4,6,8,10], nums2 = [2,4,6,8,10]
输出：0
解释：nums1 和 nums2 相等，所以不用替换元素。绝对差值和为 0
```

示例 3：

```text
输入: nums1 = [1,10,4,4,2,7], nums2 = [9,3,5,1,7,4]
输出: 20
解释: 将第一个元素替换为第二个元素:[1,10,4,4,2,7] => [10,10,4,4,2,7]
绝对差值和为 `|10-9| + |10-3| + |4-5| + |4-1| + |2-7| + |7-4| = 20`
```

__提示：__

- `n == nums1.length`
- `n == nums2.length`
- `1 <= n <= 10 ^ 5`
- `1 <= nums1[i], nums2[i] <= 10 ^ 5`

__思路:__

```text
排序
如果不替换元素, 绝对差值和为 sum = sum(|nums1[i] - nums2[i]|)
替换后的元素设为 nums1[j], sum = sum(|nums1[i] - nums2[i]|) - (|nums1[j] - nums2[i]|)
所以要最大化 |nums1[i] - nums2[i]| - (|nums1[j] - nums2[i]|)
当 i 确定时, 需要找一个 j 使得 nums1[j] 尽可能接近 nums2[i]
将 nums1 排序, 记录排序后的数组为 record
用二分查找找到 nums2[i] 在 record 中的位置
计算 nums2[i] 和 record[j] 的差值, 最大化这个差值
最后返回 sum - cur 对 MOD 取余即可
时间复杂度为 O(NlogN), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int minAbsoluteSumDiff(vector<int>& nums1, vector<int>& nums2) 
    {
        vector<int> record(nums1);
        sort(record.begin(), record.end());
        int sum = 0, cur = 0, mod = 1e9 + 7, n = nums1.size();
        for (int i = 0; i < n; i++) 
        {
            int diff = abs(nums1[i] - nums2[i]), j = lower_bound(record.begin(), record.end(), nums2[i]) - record.begin();;
            sum = (sum + diff) % mod;
            if (j < n) cur = max(cur, diff - (record[j] - nums2[i]));
            if (j > 0) cur = max(cur, diff - (nums2[i] - record[j - 1]));
        }
        return (sum - cur + mod) % mod;
    }
};
```

__Java__:

```Java
class Solution {
    public int minAbsoluteSumDiff(int[] nums1, int[] nums2) {
        int n = nums1.length, record[] = Arrays.copyOf(nums1, n), sum = 0, cur = 0, MOD = 1_000_000_007;
        Arrays.sort(record);
        for (int i = 0; i < n; i++) {
            int diff = Math.abs(nums1[i] - nums2[i]), idx = Arrays.binarySearch(record, nums2[i]), j = idx < 0 ? -idx - 1 : idx;
            sum = (sum + diff) % MOD;
            if (j < n) cur = Math.max(cur, diff - (record[j] - nums2[i]));
            if (j > 0) cur = Math.max(cur, diff - (nums2[i] - record[j - 1]));
        }
        return (sum - cur + MOD) % MOD;
    }
}
```

__Python__:

```Python
class Solution:
    def minAbsoluteSumDiff(self, nums1: List[int], nums2: List[int]) -> int:
        n, record, s, cur, MOD = len(nums1), sorted(nums1), 0, 0, 10 ** 9 + 7
        for i in range(n):
            s += (diff := abs(nums1[i] - nums2[i]))
            j = bisect_left(record, nums2[i])
            if j < n:
                cur = max(cur, diff - (record[j] - nums2[i]))
            if j:
                cur = max(cur, diff - (nums2[i] - record[j - 1]))
        return (s - cur + MOD) % MOD
```
