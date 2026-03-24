# 2948 Make Lexicographically Smallest Array by Swapping Elements 交换得到字典序最小的数组

__Description:__

You are given a __0-indexed__ array of __positive__ integers `nums` and a __positive__ integer `limit`.

In one operation, you can choose any two indices `i` and `j` and swap `nums[i]` and `nums[j]` __if__ `|nums[i] - nums[j]| <= limit`.

Return the lexicographically smallest array that can be obtained by performing the operation any number of times.

An array `a` is lexicographically smaller than an array `b` if in the first position where `a` and `b` differ, array `a` has an element that is less than the corresponding element in `b`. For example, the array `[2,10,3]` is lexicographically smaller than the array `[10,2,3]` because they differ at index `0` and `2 < 10`.

__Example:__

Example 1:

```text
Input: nums = [1,5,3,9,8], limit = 2
Output: [1,3,5,8,9]
Explanation: Apply the operation 2 times:
```

- Swap nums[1] with nums[2]. The array becomes [1,3,5,9,8]
- Swap nums[3] with nums[4]. The array becomes [1,3,5,8,9]

We cannot obtain a lexicographically smaller array by applying any more operations.

Note that it may be possible to get the same result by doing different operations.

Example 2:

```text
Input: nums = [1,7,6,18,2,1], limit = 3
Output: [1,6,7,18,1,2]
Explanation: Apply the operation 3 times:
```

- Swap nums[1] with nums[2]. The array becomes [1,6,7,18,2,1]
- Swap nums[0] with nums[4]. The array becomes [2,6,7,18,1,1]
- Swap nums[0] with nums[5]. The array becomes [1,6,7,18,1,2]

We cannot obtain a lexicographically smaller array by applying any more operations.

Example 3:

```text
Input: nums = [1,7,28,19,10], limit = 3
Output: [1,7,28,19,10]
Explanation: [1,7,28,19,10] is the lexicographically smallest array we can obtain because we cannot apply the operation on any two indices.
```

__Constraints:__

- `1 <= nums.length <= 10 ^ 5`
- `1 <= nums[i] <= 10 ^ 9`
- `1 <= limit <= 10 ^ 9`

__题目描述:__

给你一个下标从 __0__ 开始的 __正整数__ 数组 `nums` 和一个 __正整数__ `limit` 。

在一次操作中，你可以选择任意两个下标 `i` 和 `j`，__如果__ 满足 `|nums[i] - nums[j]| <= limit` ，则交换 `nums[i]` 和 `nums[j]` 。

返回执行任意次操作后能得到的 字典序最小的数组 。

如果在数组 `a` 和数组 `b` 第一个不同的位置上，数组 `a` 中的对应元素比数组 `b` 中的对应元素的字典序更小，则认为数组 `a` 就比数组 `b` 字典序更小。例如，数组 `[2,10,3]` 比数组 `[10,2,3]` 字典序更小，下标 `0` 处是两个数组第一个不同的位置，且 `2 < 10` 。

__示例:__

示例 1：

```text
输入：nums = [1,5,3,9,8], limit = 2
输出：[1,3,5,8,9]
解释：执行 2 次操作：
```

- 交换 nums[1] 和 nums[2] 。数组变为 [1,3,5,9,8] 。
- 交换 nums[3] 和 nums[4] 。数组变为 [1,3,5,8,9] 。

即便执行更多次操作，也无法得到字典序更小的数组。

注意，执行不同的操作也可能会得到相同的结果。

示例 2：

```text
输入：nums = [1,7,6,18,2,1], limit = 3
输出：[1,6,7,18,1,2]
解释：执行 3 次操作：
```

- 交换 nums[1] 和 nums[2] 。数组变为 [1,6,7,18,2,1] 。
- 交换 nums[0] 和 nums[4] 。数组变为 [2,6,7,18,1,1] 。
- 交换 nums[0] 和 nums[5] 。数组变为 [1,6,7,18,1,2] 。

即便执行更多次操作，也无法得到字典序更小的数组。

示例 3：

```text
输入：nums = [1,7,28,19,10], limit = 3
输出：[1,7,28,19,10]
解释：[1,7,28,19,10] 是字典序最小的数组，因为不管怎么选择下标都无法执行操作。
```

__提示：__

- `1 <= nums.length <= 10 ^ 5`
- `1 <= nums[i] <= 10 ^ 9`
- `1 <= limit <= 10 ^ 9`

__思路:__

```text
分组排序
将 limit 范围内的数字排序按照分组输出即可
先按照 nums 中元素的大小给下标进行排序
如果下标对应元素的大小绝对值差在 limit 范围内就是同一组元素
找到一组所有元素映射到结果中
时间复杂度为 O(NlogN), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<int> lexicographicallySmallestArray(vector<int>& nums, int limit) 
    {
        int n = nums.size();
        vector<int> idx(n), result(n);
        iota(idx.begin(), idx.end(), 0);
        sort(idx.begin(), idx.end(), [&](const auto& i, const auto& j) { return nums[i] < nums[j]; });
        for (int i = 0, start = 0; i < n; )
        {
            for (start = i++; i < n and nums[idx[i]] - nums[idx[i - 1]] <= limit; i++) ;
            vector<int> cur(idx.begin() + start, idx.begin() + i);
            sort(cur.begin(), cur.end());
            for (int j = 0, m = i - start; j < m; j++) result[cur[j]] = nums[idx[start + j]];
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int[] lexicographicallySmallestArray(int[] nums, int limit) {
        int n = nums.length, result[] = new int[n], idx[] = IntStream.range(0, n).boxed().sorted((i, j) -> nums[i] - nums[j]).mapToInt(i -> i).toArray();
        for (int i = 0, start = 0; i < n; ) {
            for (start = i++; i < n && nums[idx[i]] - nums[idx[i - 1]] <= limit; i++) ;
            for (int j = 0, cur[] = Arrays.stream(Arrays.copyOfRange(idx, start, i)).sorted().toArray(), m = i - start; j < m; j++) result[cur[j]] = nums[idx[start + j]];
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def lexicographicallySmallestArray(self, nums: List[int], limit: int) -> List[int]:
        result, i, a = [0] * (n := len(nums)), 0, sorted(zip(nums, range(n)))
        while i < n:
            start = i
            i += 1
            while i < n and a[i][0] - a[i - 1][0] <= limit:
                i += 1
            cur, cur_idx = a[start:i], sorted(j for _, j in a[start:i])
            for j, (num, _) in zip(cur_idx, cur):
                result[j] = num
        return result
```
