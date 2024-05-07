# 2099 Find Subsequence of Length K With the Largest Sum 找到和最大的长度为 K 的子序列

__Description:__

You are given an integer array `nums` and an integer `k`. You want to find a __subsequence__ of `nums` of length `k` that has the __largest__ sum.

Return ___any__ such subsequence as an integer array of length_ `k`.

A subsequence is an array that can be derived from another array by deleting some or no elements without changing the order of the remaining elements.

__Example:__

Example 1:

```text
Input: nums = [2,1,3,3], k = 2
Output: [3,3]
Explanation:
The subsequence has the largest sum of 3 + 3 = 6.
```

Example 2:

```text
Input: nums = [-1,-2,3,4], k = 3
Output: [-1,3,4]
Explanation: 
The subsequence has the largest sum of -1 + 3 + 4 = 6.
```

Example 3:

```text
Input: nums = [3,4,3,3], k = 2
Output: [3,4]
Explanation:
The subsequence has the largest sum of 3 + 4 = 7. 
Another possible subsequence is [4, 3].
```

__Constraints:__

- `1 <= nums.length <= 1000`
- `-10 ^ 5 <= nums[i] <= 10 ^ 5`
- `1 <= k <= nums.length`

__题目描述:__

给你一个整数数组 `nums` 和一个整数 `k` 。你需要找到 `nums` 中长度为 `k` 的 __子序列__ ，且这个子序列的 __和最大__ 。

请你返回 __任意__ 一个长度为 `k` 的整数子序列。

子序列 定义为从一个数组里删除一些元素后，不改变剩下元素的顺序得到的数组。

__示例:__

示例 1：

```text
输入：nums = [2,1,3,3], k = 2
输出：[3,3]
解释：
子序列有最大和：3 + 3 = 6 。
```

示例 2：

```text
输入：nums = [-1,-2,3,4], k = 3
输出：[-1,3,4]
解释：
子序列有最大和：-1 + 3 + 4 = 6 。
```

示例 3：

```text
输入：nums = [3,4,3,3], k = 2
输出：[3,4]
解释：
子序列有最大和：3 + 4 = 7 。
另一个可行的子序列为 [4, 3] 。
```

__提示：__

- `1 <= nums.length <= 1000`
- `-10 ^ 5 <= nums[i] <= 10 ^ 5`
- `1 <= k <= nums.length`

__思路:__

```text
排序
将原数组的下标记录下来
按照数组元素的大小先进行一次排序
这样可以得到最大的 k 个数
为了得到按照原数组的顺序的数
再按照下标进行排序
则前 k 个数即为要求的数
时间复杂度为 O(NlogN), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<int> maxSubsequence(vector<int>& nums, int k) 
    {
        int n = nums.size();
        vector<pair<int, int>> idx;
        for (int i = 0; i < n; i++) idx.emplace_back(i, nums[i]);
        sort(idx.begin(), idx.end(), [&](auto x1, auto x2) { return x1.second > x2.second; });
        sort(idx.begin(), idx.begin() + k);
        vector<int> result(k);
        for (int i = 0; i < k; i++) result[i] = idx[i].second;
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int[] maxSubsequence(int[] nums, int k) {
        int n = nums.length, idx[][] = new int[n][2], result[] = new int[k];
        for (int i = 0; i < n; i++) {
            idx[i][1] = nums[i];
            idx[i][0] = i;
        }
        Arrays.sort(idx, (a, b) -> b[1] - a[1]);
        Arrays.sort(idx, 0, k, (a, b) -> a[0] - b[0]);
        for (int i = 0; i < k; i++) result[i] = idx[i][1];
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def maxSubsequence(self, nums: List[int], k: int) -> List[int]:
        return list(zip(*sorted(sorted(enumerate(nums), key = lambda x : -x[1])[:k])))[1]
```
