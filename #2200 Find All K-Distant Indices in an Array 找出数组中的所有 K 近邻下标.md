# 2200 Find All K-Distant Indices in an Array 找出数组中的所有 K 近邻下标

__Description:__

You are given a __0-indexed__ integer array `nums` and two integers `key` and `k`. A __k-distant index__ is an index `i` of `nums` for which there exists at least one index `j` such that `|i - j| <= k` and `nums[j] == key`.

Return a list of all k-distant indices sorted in increasing order.

__Example:__

Example 1:

```text
Input:  nums = [3,4,9,1,3,9,5], key = 9, k = 1
Output:  [1,2,3,4,5,6]
Explanation:  Here, `nums[2] == key` and `nums[5] == key.
```

- For index 0, |0 - 2| > k and |0 - 5| > k, so there is no j` where `|0 - j| <= k` and `nums[j] == key. Thus, 0 is not a k-distant index.
- For index 1, |1 - 2| <= k and nums[2] == key, so 1 is a k-distant index.
- For index 2, |2 - 2| <= k and nums[2] == key, so 2 is a k-distant index.
- For index 3, |3 - 2| <= k and nums[2] == key, so 3 is a k-distant index.
- For index 4, |4 - 5| <= k and nums[5] == key, so 4 is a k-distant index.
- For index 5, |5 - 5| <= k and nums[5] == key, so 5 is a k-distant index.
- For index 6, |6 - 5| <= k and nums[5] == key, so 6 is a k-distant index.

Thus, we return [1,2,3,4,5,6] which is sorted in increasing order.

Example 2:

```text
Input: nums = [2,2,2,2,2], key = 2, k = 2
Output: [0,1,2,3,4]
Explanation: For all indices i in nums, there exists some index j such that |i - j| <= k and nums[j] == key, so every index is a k-distant index. 
Hence, we return [0,1,2,3,4].
```

__Constraints:__

- `1 <= nums.length <= 1000`
- `1 <= nums[i] <= 1000`
- `key` is an integer from the array `nums`.
- `1 <= k <= nums.length`

__题目描述:__

给你一个下标从 __0__ 开始的整数数组 `nums` 和两个整数 `key` 和 `k` 。__K 近邻下标__ 是 `nums` 中的一个下标 `i` ，并满足至少存在一个下标 `j` 使得 `|i - j| <= k` 且 `nums[j] == key` 。

以列表形式返回按 递增顺序 排序的所有 K 近邻下标。

__示例:__

示例 1：

```text
输入: nums = [3,4,9,1,3,9,5], key = 9, k = 1
输出: [1,2,3,4,5,6]
解释: 因此， `nums[2] == key` 且 `nums[5] == key 。
```

- 对下标 0 ，|0 - 2| > k 且 |0 - 5| > k ，所以不存在 j` 使得 `|0 - j| <= k` 且 `nums[j] == key 。所以 0 不是一个 K 近邻下标。
- 对下标 1 ，|1 - 2| <= k 且 nums[2] == key ，所以 1 是一个 K 近邻下标。
- 对下标 2 ，|2 - 2| <= k 且 nums[2] == key ，所以 2 是一个 K 近邻下标。
- 对下标 3 ，|3 - 2| <= k 且 nums[2] == key ，所以 3 是一个 K 近邻下标。
- 对下标 4 ，|4 - 5| <= k 且 nums[5] == key ，所以 4 是一个 K 近邻下标。
- 对下标 5 ，|5 - 5| <= k 且 nums[5] == key ，所以 5 是一个 K 近邻下标。
- 对下标 6 ，|6 - 5| <= k 且 nums[5] == key ，所以 6 是一个 K 近邻下标。

因此，按递增顺序返回 [1,2,3,4,5,6] 。

示例 2：

```text
输入: nums = [2,2,2,2,2], key = 2, k = 2
输出: [0,1,2,3,4]
解释: 对 nums 的所有下标 i ，总存在某个下标 j 使得 |i - j| <= k 且 nums[j] == key ，所以每个下标都是一个 `K 近邻下标。` 
因此，返回 [0,1,2,3,4] 。
```

__提示：__

- `1 <= nums.length <= 1000`
- `1 <= nums[i] <= 1000`
- `key` 是数组 `nums` 中的一个整数
- `1 <= k <= nums.length`

__思路:__

```text
1. 暴力
每次检查 i - k 到 i + k 之间的元素是否等于 key
如果有就把 i 加入结果中
时间复杂度为 O(N ^ 2), 空间复杂度为 O(1)
2. 标记
遍历数组, 如果 nums[i] == key, 则将 i - k 到 i + k 之间的元素标记为已访问
将所有标记的元素加入结果中
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<int> findKDistantIndices(vector<int>& nums, int key, int k) 
    {
        int n = nums.size();
        vector<int> result, visited(n);
        for (int i = 0; i < n; i++) 
        {
            if (nums[i] == key) 
            {
                for (int j = i; j >= i - k and j > -1 and !visited[j]; j--) visited[j] = true;
                for (int j = min(i + k, n - 1); !visited[j]; j--) visited[j] = true;
            }
        }
        for (int i = 0; i < n; i++) if (visited[i]) result.emplace_back(i);
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public List<Integer> findKDistantIndices(int[] nums, int key, int k) {
        List<Integer> result = new ArrayList<>();
        int n = nums.length;
        boolean[] visited = new boolean[n];
        for (int i = 0; i < n; i++) {
            if (nums[i] == key) {
                for (int j = i; j >= i - k && j > -1 && !visited[j]; j--) visited[j] = true;
                for (int j = Math.min(i + k, n - 1); !visited[j]; j--) visited[j] = true;
            }
        }
        for (int i = 0; i < n; i++) if (visited[i]) result.add(i);
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def findKDistantIndices(self, nums: List[int], key: int, k: int) -> List[int]:
        return [i for i in range(len(nums)) if any(nums[j] == key for j in range(max(0, i - k), min(len(nums), i + k + 1)))]
```
