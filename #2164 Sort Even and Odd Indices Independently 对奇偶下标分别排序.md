# 2164 Sort Even and Odd Indices Independently 对奇偶下标分别排序

__Description:__

You are given a __0-indexed__ integer array `nums`. Rearrange the values of `nums` according to the following rules:

- For example, if nums = [4,__1__,2,__3__] before this step, it becomes [4,__3__,2,__1__] after. The values at odd indices `1` and `3` are sorted in non-increasing order.

- For example, if nums = [__4__,1,__2__,3] before this step, it becomes [__2__,1,__4__,3] after. The values at even indices `0` and `2` are sorted in non-decreasing order.

Return _the array formed after rearranging the values of_ `nums`.

__Example:__

Example 1:

```text
Input: nums = [4,1,2,3]
Output: [2,3,4,1]
Explanation: 
First, we sort the values present at odd indices (1 and 3) in non-increasing order.
So, nums changes from [4,1,2,3] to [4,3,2,1].
Next, we sort the values present at even indices (0 and 2) in non-decreasing order.
So, nums changes from [4,1,2,3] to [2,3,4,1].
Thus, the array formed after rearranging the values is [2,3,4,1].
```

Example 2:

```text
Input: nums = [2,1]
Output: [2,1]
Explanation: 
Since there is exactly one odd index and one even index, no rearrangement of values takes place.
The resultant array formed is [2,1], which is the same as the initial array.
```

__Constraints:__

- `1 <= nums.length <= 100`
- `1 <= nums[i] <= 100`

__题目描述:__

给你一个下标从 __0__ 开始的整数数组 `nums` 。根据下述规则重排 `nums` 中的值:

- 举个例子，如果排序前 nums = [4,___1___,2,___3___] ，对奇数下标的值排序后变为 [4,___3___,2,___1___] 。奇数下标 `1` 和 `3` 的值按照非递增顺序重排。

- 举个例子，如果排序前 nums = [___4___,1,___2___,3] ，对偶数下标的值排序后变为 [___2___,1,___4___,3] 。偶数下标 `0` 和 `2` 的值按照非递减顺序重排。

返回重排 `nums` 的值之后形成的数组。

__示例:__

示例 1：

```text
输入：nums = [4,1,2,3]
输出：[2,3,4,1]
解释：
首先，按非递增顺序重排奇数下标（1 和 3）的值。
所以，nums 从 [4,1,2,3] 变为 [4,3,2,1] 。
然后，按非递减顺序重排偶数下标（0 和 2）的值。
所以，nums 从 [4,1,2,3] 变为 [2,3,4,1] 。
因此，重排之后形成的数组是 [2,3,4,1] 。
```

示例 2：

```text
输入：nums = [2,1]
输出：[2,1]
解释：
由于只有一个奇数下标和一个偶数下标，所以不会发生重排。
形成的结果数组是 [2,1] ，和初始数组一样。
```

__提示：__

- `1 <= nums.length <= 100`
- `1 <= nums[i] <= 100`

__思路:__

```text
模拟
分别将奇数下标和偶数下标的值分别排
然后按照奇数下标和偶数下标的顺序重排
时间复杂度为 O(NlogN), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<int> sortEvenOdd(vector<int>& nums) 
    {
        vector<int> even, odd;
        for (int i = 0, n = nums.size(); i < n; i++) 
        {
            if (i & 1) odd.push_back(nums[i]);
            else even.push_back(nums[i]);
        }
        sort(even.begin(), even.end());
        sort(odd.begin(), odd.end(), greater<int>());
        for (int i = 0, n = even.size(); i < n; i++) nums[i << 1] = even[i];
        for (int i = 0, n = odd.size(); i < n; i++) nums[(i << 1) + 1] = odd[i];
        return nums;
    }
};
```

__Java__:

```Java
class Solution {
    public int[] sortEvenOdd(int[] nums) {
        List<Integer> even = new ArrayList<>(), odd = new ArrayList<>();
        for (int i = 0, n = nums.length; i < n; i++) {
            if ((i & 1) == 1) odd.add(nums[i]);
            else even.add(nums[i]);
        }
        Collections.sort(even);
        Collections.sort(odd, (a, b) -> b - a);
        for (int i = 0, n = even.size(); i < n; i++) nums[i << 1] = even.get(i);
        for (int i = 0, n = odd.size(); i < n; i++) nums[(i << 1) + 1] = odd.get(i);
        return nums;
    }
}
```

__Python__:

```Python
class Solution:
    def sortEvenOdd(self, nums: List[int]) -> List[int]:
        return list(chain(*[(x, y) for x, y in zip(sorted(nums[::2]), sorted(nums[1::2],reverse=True))])) + [max([i for i in nums[::2]])] if len(nums) & 1 else list(chain(*[(x, y) for x, y in zip(sorted(nums[::2]), sorted(nums[1::2],reverse=True))]))
```
