# 2420 Find All Good Indices 找到所有好下标

__Description:__

You are given a __0-indexed__ integer array `nums` of size `n` and a positive integer `k`.

We call an index `i` in the range `k <= i < n - k` __good__ if the following conditions are satisfied:

- The `k` elements that are just __before__ the index `i` are in __non-increasing__ order.
- The `k` elements that are just __after__ the index `i` are in __non-decreasing__ order.

Return an array of all good indices sorted in increasing order.

__Example:__

Example 1:

```text
Input: nums = [2,1,1,1,3,4,1], k = 2
Output: [2,3]
Explanation: There are two good indices in the array:
```

- Index 2. The subarray [2,1] is in non-increasing order, and the subarray [1,3] is in non-decreasing order.
- Index 3. The subarray [1,1] is in non-increasing order, and the subarray [3,4] is in non-decreasing order.

Note that the index 4 is not good because [4,1] is not non-decreasing.

Example 2:

```text
Input: nums = [2,1,1,2], k = 2
Output: []
Explanation: There are no good indices in this array.
```

__Constraints:__

- `n == nums.length`
- `3 <= n <= 10 ^ 5`
- `1 <= nums[i] <= 10 ^ 6`
- `1 <= k <= n / 2`

__题目描述:__

给你一个大小为 `n` 下标从 __0__ 开始的整数数组 `nums` 和一个正整数 `k` 。

对于 `k <= i < n - k` 之间的一个下标 `i` ，如果它满足以下条件，我们就称它为一个 __好__ 下标:

- 下标 `i` __之前__ 的 `k` 个元素是 __非递增的__ 。
- 下标 `i` __之后__ 的 `k` 个元素是 __非递减的__ 。

按 升序 返回所有好下标。

__示例:__

示例 1：

```text
输入：nums = [2,1,1,1,3,4,1], k = 2
输出：[2,3]
解释：数组中有两个好下标：
```

- 下标 2 。子数组 [2,1] 是非递增的，子数组 [1,3] 是非递减的。
- 下标 3 。子数组 [1,1] 是非递增的，子数组 [3,4] 是非递减的。

注意，下标 4 不是好下标，因为 [4,1] 不是非递减的。

示例 2：

```text
输入：nums = [2,1,1,2], k = 2
输出：[]
解释：数组中没有好下标。
```

__提示：__

- `n == nums.length`
- `3 <= n <= 10 ^ 5`
- `1 <= nums[i] <= 10 ^ 6`
- `1 <= k <= n / 2`

__思路:__

```text
模拟
从后往前记录最长非递减子数组长度
从前往后遍历数组, 同时记录最长非递增子数组长度
如果下标满足 inc >= k and dec[i + 1] >= k, 则加入结果数组
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<int> goodIndices(vector<int>& nums, int k) 
    {
        int n = nums.size(), inc = 1;
        vector<int> dec(n, 1), result;
        for (int i = n - 2; i > k; i--) if (nums[i] <= nums[i + 1]) dec[i] = dec[i + 1] + 1;
        for (int i = 1; i < n - k; i++) 
        {
            if (inc >= k and dec[i + 1] >= k) result.emplace_back(i);
            if (nums[i] <= nums[i - 1]) ++inc;
            else inc = 1;
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public List<Integer> goodIndices(int[] nums, int k) {
        int n = nums.length, dec[] = new int[n], inc = 1;
        for (int i = n - 2; i > k; i--) if (nums[i] <= nums[i + 1]) dec[i] = dec[i + 1] + 1;
        List<Integer> result = new ArrayList<>();
        for (int i = 1; i < n - k; i++) {
            if (inc >= k && dec[i + 1] >= k - 1) result.add(i);
            if (nums[i] <= nums[i - 1]) ++inc;
            else inc = 1;
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def goodIndices(self, nums: List[int], k: int) -> List[int]:
        dec, result, inc = [1] * (n := len(nums)), [], 1
        for i in range(n - 2, k, -1):
            if nums[i] <= nums[i + 1]:
                dec[i] = dec[i + 1] + 1
        for i in range(1, n - k):
            if inc >= k and dec[i + 1] >= k:
                result.append(i)
            if nums[i - 1] >= nums[i]:
                inc += 1
            else:
                inc = 1
        return result
```
