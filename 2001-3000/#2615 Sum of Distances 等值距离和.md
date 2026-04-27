# 2615 Sum of Distances 等值距离和

__Description:__

You are given a __0-indexed__ integer array `nums`. There exists an array `arr` of length `nums.length`, where `arr[i]` is the sum of `|i - j|` over all `j` such that `nums[j] == nums[i]` and `j != i`. If there is no such `j`, set `arr[i]` to be `0`.

Return _the array_ `arr`_._

__Example:__

Example 1:

```text
Input: nums = [1,3,1,1,2]
Output: [5,0,3,4,0]
Explanation: 
When i = 0, nums[0] == nums[2] and nums[0] == nums[3]. Therefore, arr[0] = |0 - 2| + |0 - 3| = 5. 
When i = 1, arr[1] = 0 because there is no other index with value 3.
When i = 2, nums[2] == nums[0] and nums[2] == nums[3]. Therefore, arr[2] = |2 - 0| + |2 - 3| = 3. 
When i = 3, nums[3] == nums[0] and nums[3] == nums[2]. Therefore, arr[3] = |3 - 0| + |3 - 2| = 4. 
When i = 4, arr[4] = 0 because there is no other index with value 2.
```

Example 2:

```text
Input: nums = [0,5,3]
Output: [0,0,0]
Explanation: Since each element in nums is distinct, arr[i] = 0 for all i.
```

__Constraints:__

- `1 <= nums.length <= 10 ^ 5`
- `0 <= nums[i] <= 10 ^ 9`

Note: This question is the same as 2121: Intervals Between Identical Elements.

__题目描述:__

给你一个下标从 __0__ 开始的整数数组 `nums` 。现有一个长度等于 `nums.length` 的数组 `arr` 。对于满足 `nums[j] == nums[i]` 且 `j != i` 的所有 `j` ， `arr[i]` 等于所有 `|i - j|` 之和。如果不存在这样的 `j` ，则令 `arr[i]` 等于 `0` 。

返回数组 `arr` _。_

__示例:__

示例 1：

```text
输入：nums = [1,3,1,1,2]
输出：[5,0,3,4,0]
解释：
i = 0 ，nums[0] == nums[2] 且 nums[0] == nums[3] 。因此，arr[0] = |0 - 2| + |0 - 3| = 5 。 
i = 1 ，arr[1] = 0 因为不存在值等于 3 的其他下标。
i = 2 ，nums[2] == nums[0] 且 nums[2] == nums[3] 。因此，arr[2] = |2 - 0| + |2 - 3| = 3 。
i = 3 ，nums[3] == nums[0] 且 nums[3] == nums[2] 。因此，arr[3] = |3 - 0| + |3 - 2| = 4 。 
i = 4 ，arr[4] = 0 因为不存在值等于 2 的其他下标。
```

示例 2：

```text
输入：nums = [0,5,3]
输出：[0,0,0]
解释：因为 nums 中的元素互不相同，对于所有 i ，都有 arr[i] = 0 。
```

__提示：__

- `1 <= nums.length <= 10 ^ 5`
- `0 <= nums[i] <= 10 ^ 9`

__思路:__

```text
数学
按照数字将下标分成组
设 nums[i] 对应的组为 v
当 v 的长度为 1 时, arr[i] = 0
对于 v[0], 令 s = sum(v) - v[0] * v.length
则 arr[v[0]] = s
对于 v[1], arr[v[1]] = sum(v) - v[1] * v.length + 2 * (v[0] - v[1])
即 s + (2 - v.length) * (v[1] - v[0])
所以 arr[v[i]] = s + (2 * i - v.length) * (v[i] - v[i - 1])
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<long long> distance(vector<int>& nums) 
    {
        int n = nums.size(), m = 0;
        unordered_map<int, vector<int>> group;
        for (int i = 0; i < n; i++) group[nums[i]].emplace_back(i);
        vector<long long> result(n);
        long long s = 0LL;
        for (const auto& [_, v] : group)
        {
            if ((m = v.size()) > 1) 
            {
                result[v.front()] = s = accumulate(v.begin(), v.end(), 0LL) - m * v.front();
                for (int i = 1; i < m; i++) result[v[i]] = s += (long long)((i << 1) - m) * (v[i] - v[i - 1]);
            }
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public long[] distance(int[] nums) {
        int n = nums.length, m = 0;
        var group = new HashMap<Integer, List<Integer>>();
        for (int i = 0; i < n; i++) group.computeIfAbsent(nums[i], k -> new ArrayList<>()).add(i);
        long result[] = new long[n], s = 0L;
        for (var v : group.values()) {
            if ((m = v.size()) > 1) {
                result[v.get(0)] = s = v.stream().mapToLong(k -> k).sum() - m * v.get(0);
                for (int i = 1; i < m; i++) result[v.get(i)] = s += (long)((i << 1) - m) * (v.get(i) - v.get(i - 1));
            }
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def distance(self, nums: List[int]) -> List[int]:
        group, result= defaultdict(list), [0] * len(nums)
        for i, num in enumerate(nums):
            group[num].append(i)
        for v in group.values():
            if (n := len(v)) != 1:
                result[v[0]] = s = sum(x - v[0] for x in v)
                for i in range(1, n):
                    result[v[i]] = (s := s + ((i << 1) - n) * (v[i] - v[i - 1]))
        return result
```
