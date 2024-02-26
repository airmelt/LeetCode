# 2009 Minimum Number of Operations to Make Array Continuous 使数组连续的最少操作数

__Description:__

You are given an integer array `nums`. In one operation, you can replace __any__ element in `nums` with __any__ integer.

`nums` is considered __continuous__ if both of the following conditions are fulfilled:

- All elements in `nums` are __unique__.
- The difference between the __maximum__ element and the __minimum__ element in `nums` equals `nums.length - 1`.

For example, `nums = [4, 2, 5, 3]` is __continuous__, but `nums = [1, 2, 3, 5, 6]` is __not continuous__.

Return _the __minimum__ number of operations to make_ `nums` ___continuous___.

__Example:__

Example 1:

```text
Input: nums = [4,2,5,3]
Output: 0
Explanation: nums is already continuous.
```

Example 2:

```text
Input: nums = [1,2,3,5,6]
Output: 1
Explanation: One possible solution is to change the last element to 4.
The resulting array is [1,2,3,5,4], which is continuous.
```

Example 3:

```text
Input: nums = [1,10,100,1000]
Output: 3
Explanation: One possible solution is to:
```

- Change the second element to 2.
- Change the third element to 3.
- Change the fourth element to 4.
The resulting array is [1,2,3,4], which is continuous.

__Constraints:__

- `1 <= nums.length <= 10 ^ 5`
- `1 <= nums[i] <= 10 ^ 9`

__题目描述:__

给你一个整数数组 `nums` 。每一次操作中，你可以将 `nums` 中 __任意__ 一个元素替换成 __任意__ 整数。

如果 `nums` 满足以下条件，那么它是 __连续的__ :

- `nums` 中所有元素都是 _互不相同_ 的。
- `nums` 中 __最大__ 元素与 __最小__ 元素的差等于 `nums.length - 1` 。

比方说， `nums = [4, 2, 5, 3]` 是 __连续的__ ，但是 `nums = [1, 2, 3, 5, 6]` __不是连续的__ 。

请你返回使 `nums` __连续__ 的 __最少__ 操作次数。

__示例:__

示例 1：

```text
输入：nums = [4,2,5,3]
输出：0
解释：nums 已经是连续的了。
```

示例 2：

```text
输入：nums = [1,2,3,5,6]
输出：1
解释：一个可能的解是将最后一个元素变为 4 。
结果数组为 [1,2,3,5,4] ，是连续数组。
```

示例 3：

```text
输入：nums = [1,10,100,1000]
输出：3
解释：一个可能的解是：
```

- 将第二个元素变为 2 。
- 将第三个元素变为 3 。
- 将第四个元素变为 4 。
结果数组为 [1,2,3,4] ，是连续数组。

__提示：__

- `1 <= nums.length <= 10 ^ 5`
- `1 <= nums[i] <= 10 ^ 9`

__思路:__

```text
排序 ➕ 二分查找
计算最多可以保留的元素个数，即最大元素与最小元素的差值不大于 nums[r] - n + 1
先对 nums 数组进行排序，并去重
遍历 nums 数组，对于每个元素 nums[i]，找到最大的 j 使得 nums[j] - nums[i] + 1 <= n
则最多可以保留的元素个数为 i - j + 1
最后返回 n - 最多可以保留的元素个数
时间复杂度为 O(NlogN), 空间复杂度为 O(logN), 去重使用原地算法, 排序和二分查找使用的空间复杂度为 O(NlogN), 空间复杂度为 O(logN), 主要是排序的空间复杂度
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int minOperations(vector<int>& nums) 
    {
        int n = nums.size(), result = 0;
        sort(nums.begin(), nums.end());
        nums.erase(unique(nums.begin(), nums.end()), nums.end());
        for (int i = 0, k = nums.size(); i < k; i++) 
        {
            int l = 0, r = i;
            while (l < r) 
            {
                int mid = l + ((r - l) >> 1);
                if (nums[mid] < nums[i] - n + 1) l = mid + 1;
                else r = mid;
            }
            result = max(result, i - r + 1);
        }
        return n - result;
    }
};
```

__Java__:

```Java
class Solution {
    public int minOperations(int[] nums) {
        int n = nums.length, k = 0, result = 0;
        Arrays.sort(nums);
        for (int i = 1; i < n; i++) if (nums[k] != nums[i]) nums[++k] = nums[i];
        for (int i = 0; i <= k; i++) {
            int l = 0, r = i;
            while (l < r) {
                int mid = l + ((r - l) >> 1);
                if (nums[mid] < nums[i] - n + 1) l = mid + 1;
                else r = mid;
            }
            result = Math.max(result, i - r + 1);
        }
        return n - result;
    }
}
```

__Python__:

```Python
class Solution:
    def minOperations(self, nums: List[int]) -> int:
        n, nums, result = len(nums), sorted(list(set(nums))), 0
        for r, v in enumerate(nums):
            result = max(result, r - bisect_left(nums, v - n + 1, 0, r) + 1)
        return n - result
```
