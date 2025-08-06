# 2616 Minimize the Maximum Difference of Pairs 最小化数对的最大差值

__Description:__

You are given a __0-indexed__ integer array `nums` and an integer `p`. Find `p` pairs of indices of `nums` such that the __maximum__ difference amongst all the pairs is __minimized__. Also, ensure no index appears more than once amongst the `p` pairs.

Note that for a pair of elements at the index `i` and `j`, the difference of this pair is `|nums[i] - nums[j]|`, where `|x|` represents the __absolute__ __value__ of `x`.

Return _the __minimum__ __maximum__ difference among all_ `p` _pairs._ We define the maximum of an empty set to be zero.

__Example:__

Example 1:

```text
Input: nums = [10,1,2,7,1,3], p = 2
Output: 1
Explanation: The first pair is formed from the indices 1 and 4, and the second pair is formed from the indices 2 and 5. 
The maximum difference is max(|nums[1] - nums[4]|, |nums[2] - nums[5]|) = max(0, 1) = 1. Therefore, we return 1.
```

Example 2:

```text
Input: nums = [4,2,1,2], p = 1
Output: 0
Explanation: Let the indices 1 and 3 form a pair. The difference of that pair is |2 - 2| = 0, which is the minimum we can attain.
```

__Constraints:__

- `1 <= nums.length <= 10 ^ 5`
- `0 <= nums[i] <= 10 ^ 9`
- `0 <= p <= (nums.length)/2`

__题目描述:__

给你一个下标从 __0__ 开始的整数数组 `nums` 和一个整数 `p` 。请你从 `nums` 中找到 `p` 个下标对，每个下标对对应数值取差值，你需要使得这 `p` 个差值的 __最大值__ __最小__。同时，你需要确保每个下标在这 `p` 个下标对中最多出现一次。

对于一个下标对 `i` 和 `j` ，这一对的差值为 `|nums[i] - nums[j]|` ，其中 `|x|` 表示 `x` 的 __绝对值__ 。

请你返回 `p` 个下标对对应数值 __最大差值__ 的 __最小值__ 。我们定义空集的最大值为零。

__示例:__

示例 1：

```text
输入：nums = [10,1,2,7,1,3], p = 2
输出：1
解释：第一个下标对选择 1 和 4 ，第二个下标对选择 2 和 5 。
最大差值为 max(|nums[1] - nums[4]|, |nums[2] - nums[5]|) = max(0, 1) = 1 。所以我们返回 1 。
```

示例 2：

```text
输入：nums = [4,2,1,2], p = 1
输出：0
解释：选择下标 1 和 3 构成下标对。差值为 |2 - 2| = 0 ，这是最大差值的最小值。
```

__提示：__

- `1 <= nums.length <= 10 ^ 5`
- `0 <= nums[i] <= 10 ^ 9`
- `0 <= p <= (nums.length)/2`

__思路:__

```text
二分
由于选择顺序与 nums 初始顺序无关
排序之后两个相邻的数值差值最小
先将 nums 排序
二分查找最大差值
可以在 [0, nums[n - 1] - nums[0]] 范围内查找
对于每个 mid 值, 检查是否可以找到 p 个下标对使得最大
如果相邻元素差值小于等于 mid, 则可以组成一对此时 i += 2 跳过下一个元素
否则继续查找下一个元素是否满足
最后检查对数是否超过 p
如果不超过 p, 则说明 mid 太小, 需要增大, left = mid + 1
如果超过 p, 则说明 mid 太大, 需要减小, right = mid - 1
最终 left 即为所求的最小最大差值
时间复杂度为 O(NlogN), 空间复杂度为 O(logN)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int minimizeMax(vector<int>& nums, int p) 
    {
        sort(nums.begin(), nums.end());
        int left = 0, right = nums.back() - nums.front(), n = nums.size(), mid = 0;
        auto check = [&](int mid, int p) -> bool
        {
            for (int i = 0; i < n - 1 and p; i++) 
            {
                if (nums[i + 1] - nums[i] <= mid) 
                {
                    --p;
                    ++i;
                }
            }
            return p == 0;
        };
        while (left <= right) 
        {
            if (check(mid = left + ((right - left) >> 1), p)) right = mid - 1;
            else left = mid + 1;
        }
        return left;
    }
};
```

__Java__:

```Java
class Solution {
    public int minimizeMax(int[] nums, int p) {
        Arrays.sort(nums);
        int left = 0, right = 0x3f3f3f3f, n = nums.length, mid = 0;
        while (left <= right) {
            if (check(mid = left + ((right - left) >>> 1), nums, n, p)) right = mid - 1;
            else left = mid + 1;
        }
        return left;
    }

    private boolean check(int mid, int[] nums, int n, int p) {
        for (int i = 0; i < n - 1 && p > 0; i++) {
            if (nums[i + 1] - nums[i] <= mid) {
                --p;
                ++i;
            }
        }
        return p == 0;
    }
}
```

__Python__:

```Python
class Solution:
    def minimizeMax(self, nums: List[int], p: int) -> int:
        nums.sort()

        def check(mid: int) -> bool:
            cur = i = 0
            while i < n - 1:
                if nums[i + 1] - nums[i] <= mid:
                    cur += 1
                    i += 1
                i += 1
            return cur >= p

        left, right, n = 0, nums[-1] - nums[0], len(nums)
        while left <= right:
            if check(mid := left + ((right - left) >> 1)):
                right = mid - 1
            else:
                left = mid + 1
        return left
```
