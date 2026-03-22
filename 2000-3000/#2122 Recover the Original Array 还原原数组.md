# 2122 Recover the Original Array 还原原数组

__Description:__

Alice had a __0-indexed__ array `arr` consisting of `n` __positive__ integers. She chose an arbitrary __positive integer__ `k` and created two new __0-indexed__ integer arrays `lower` and `higher` in the following manner:

Unfortunately, Alice lost all three arrays. However, she remembers the integers that were present in the arrays `lower` and `higher`, but not the array each integer belonged to. Help Alice and recover the original array.

Given an array `nums` consisting of `2n` integers, where __exactly__ `n` of the integers were present in `lower` and the remaining in `higher`, return _the __original__ array_ `arr`. In case the answer is not unique, return ___any__ valid array_.

__Note:__ The test cases are generated such that there exists __at least one__ valid array `arr`.

__Example:__

Example 1:

```text
Input: nums = [2,10,6,4,8,12]
Output: [3,7,11]
Explanation:
If arr = [3,7,11] and k = 1, we get lower = [2,6,10] and higher = [4,8,12].
Combining lower and higher gives us [2,6,10,4,8,12], which is a permutation of nums.
Another valid possibility is that arr = [5,7,9] and k = 3. In that case, lower = [2,4,6] and higher = [8,10,12].
```

Example 2:

```text
Input: nums = [1,1,3,3]
Output: [2,2]
Explanation:
If arr = [2,2] and k = 1, we get lower = [1,1] and higher = [3,3].
Combining lower and higher gives us [1,1,3,3], which is equal to nums.
Note that arr cannot be [1,3] because in that case, the only possible way to obtain [1,1,3,3] is with k = 0.
This is invalid since k must be positive.
```

Example 3:

```text
Input: nums = [5,435]
Output: [220]
Explanation:
The only possible combination is arr = [220] and k = 215. Using them, we get lower = [5] and higher = [435].
```

__Constraints:__

- `2 * n == nums.length`
- `1 <= n <= 1000`
- `1 <= nums[i] <= 10 ^ 9`
- The test cases are generated such that there exists __at least one__ valid array `arr`.

__题目描述:__

Alice 有一个下标从 __0__ 开始的数组 `arr` ，由 `n` 个正整数组成。她会选择一个任意的 __正整数__ `k` 并按下述方式创建两个下标从 __0__ 开始的新整数数组 `lower` 和 `higher` :

不幸地是，Alice 丢失了全部三个数组。但是，她记住了在数组 `lower` 和 `higher` 中出现的整数，但不知道每个整数属于哪个数组。请你帮助 Alice 还原原数组。

给你一个由 2n 个整数组成的整数数组 `nums` ，其中 __恰好__ `n` 个整数出现在 `lower` ，剩下的出现在 `higher` ，还原并返回 __原数组__ `arr` 。如果出现答案不唯一的情况，返回 __任一__ 有效数组。

__注意:__
生成的测试用例保证存在 __至少一个__ 有效数组 `arr` 。

__示例:__

示例 1：

```text
输入：nums = [2,10,6,4,8,12]
输出：[3,7,11]
解释：
如果 arr = [3,7,11] 且 k = 1 ，那么 lower = [2,6,10] 且 higher = [4,8,12] 。
组合 lower 和 higher 得到 [2,6,10,4,8,12] ，这是 nums 的一个排列。
另一个有效的数组是 arr = [5,7,9] 且 k = 3 。在这种情况下，lower = [2,4,6] 且 higher = [8,10,12] 。
```

示例 2：

```text
输入：nums = [1,1,3,3]
输出：[2,2]
解释：
如果 arr = [2,2] 且 k = 1 ，那么 lower = [1,1] 且 higher = [3,3] 。
组合 lower 和 higher 得到 [1,1,3,3] ，这是 nums 的一个排列。
注意，数组不能是 [1,3] ，因为在这种情况下，获得 [1,1,3,3] 唯一可行的方案是 k = 0 。
这种方案是无效的，k 必须是一个正整数。
```

示例 3：

```text
输入：nums = [5,435]
输出：[220]
解释：
唯一可行的组合是 arr = [220] 且 k = 215 。在这种情况下，lower = [5] 且 higher = [435] 。
```

__提示：__

- `2 * n == nums.length`
- `1 <= n <= 1000`
- `1 <= nums[i] <= 10 ^ 9`
- 生成的测试用例保证存在 __至少一个__ 有效数组 `arr`

__思路:__

```text
双指针 ➕ 排序
由于 lower[0] 一定是最小值
将 nums 排序, 第一个数一定是 lower[0]
假设 higher[i] - lower[i] = d, 则 d 一定是偶数, 且是定值
可以假设第 i 个数是 higher[0], 直到 i == half (数组长度的一半)
计算 d = higher[0] - lower[0], 不满足条件直接跳过循环
low, high 从 1, i + 1 开始分别查找对应 lower[1] 和 higher[1]
higher[1] - lower[1] == d 且 low 必须未被访问过
high 要找到第一个使得 nums[high] - nums[low] == d 的值
如果找不到直接剪枝
时间复杂度为 O(N ^ 2), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<int> recoverArray(vector<int>& nums) 
    {
        sort(nums.begin(), nums.end());
        int n = nums.size(), half = n >> 1;
        vector<int> result(half);
        for (int i = 1, j = 0, d = 0; i <= half; i++, j = 0) 
        {
            if ((d = nums[i] - nums[0]) && (!(d & 1)))
            {
                vector<bool> visited(n);
                visited[i] = true;
                int low = 1, high = i + 1;
                result[j++] = (nums[i] + nums.front()) >> 1;
                while (high < n) 
                {
                    while (low < n and visited[low]) ++low;
                    while (high < n and nums[high] - nums[low] < d) ++high;
                    if (high < n and nums[high] - nums[low] == d) 
                    {
                        visited[high] = true;
                        result[j++] = (nums[low++] + nums[high++]) >> 1;
                    } 
                    else break;
                }
                if (j == half) break;
            }
        }
        return result; 
    }
};
```

__Java__:

```Java
class Solution {
    public int[] recoverArray(int[] nums) {
        Arrays.sort(nums);
        int n = nums.length, half = n >> 1, result[] = new int[half];
        for (int i = 1, j = 0, d = 0; i <= half; i++, j = 0) {
            if ((d = nums[i] - nums[0]) != 0 && (d & 1) == 0) {
                boolean[] visited = new boolean[n];
                visited[i] = true;
                int low = 1, high = i + 1;
                result[j++] = (nums[i] + nums[0]) >> 1;
                while (high < n) {
                    while (low < n && visited[low]) ++low;
                    while (high < n && nums[high] - nums[low] < d) ++high;
                    if (high < n && nums[high] - nums[low] == d) {
                        visited[high] = true;
                        result[j++] = (nums[low++] + nums[high++]) >> 1;
                    } else break;
                }
                if (j == half) break;
            }
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def recoverArray(self, nums: List[int]) -> List[int]:
        nums.sort()
        half = (n := len(nums)) >> 1
        for i in range(1, half + 1):
            if (d := nums[i] - nums[0]) and not (d & 1):
                visited = [False] * n
                visited[i] = True
                result = [(nums[i] + nums[0]) >> 1]
                low, high = 1, i + 1
                while high < n:
                    while low < n and visited[low]:
                        low += 1
                    while high < n and nums[high] - nums[low] < d:
                        high += 1
                    if high == n or nums[high] - nums[low] > d:
                        break
                    visited[high] = True
                    result.append((nums[low] + nums[high]) >> 1)
                    low += 1
                    high += 1
                if len(result) == half:
                    break
        return result
```
