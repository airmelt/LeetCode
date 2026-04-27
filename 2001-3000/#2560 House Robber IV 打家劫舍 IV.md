# 2560 House Robber IV 打家劫舍 IV

__Description:__

There are several consecutive houses along a street, each of which has some money inside. There is also a robber, who wants to steal money from the homes, but he refuses to steal from adjacent homes.

The capability of the robber is the maximum amount of money he steals from one house of all the houses he robbed.

You are given an integer array `nums` representing how much money is stashed in each house. More formally, the `i ^ th` house from the left has `nums[i]` dollars.

You are also given an integer `k`, representing the __minimum__ number of houses the robber will steal from. It is always possible to steal at least `k` houses.

Return _the __minimum__ capability of the robber out of all the possible ways to steal at least_ `k` _houses_.

__Example:__

Example 1:

```text
Input: nums = [2,3,5,9], k = 2
Output: 5
Explanation: 
There are three ways to rob at least 2 houses:
```

- Rob the houses at indices 0 and 2. Capability is max(nums[0], nums[2]) = 5.
- Rob the houses at indices 0 and 3. Capability is max(nums[0], nums[3]) = 9.
- Rob the houses at indices 1 and 3. Capability is max(nums[1], nums[3]) = 9.

Therefore, we return min(5, 9, 9) = 5.

Example 2:

```text
Input: nums = [2,7,9,3,1], k = 2
Output: 2
Explanation: There are 7 ways to rob the houses. The way which leads to minimum capability is to rob the house at index 0 and 4. Return max(nums[0], nums[4]) = 2.
```

__Constraints:__

- `1 <= nums.length <= 10 ^ 5`
- `1 <= nums[i] <= 10 ^ 9`
- `1 <= k <= (nums.length + 1)/2`

__题目描述:__

沿街有一排连续的房屋。每间房屋内都藏有一定的现金。现在有一位小偷计划从这些房屋中窃取现金。

由于相邻的房屋装有相互连通的防盗系统，所以小偷 不会窃取相邻的房屋 。

小偷的 窃取能力 定义为他在窃取过程中能从单间房屋中窃取的 最大金额 。

给你一个整数数组 `nums` 表示每间房屋存放的现金金额。形式上，从左起第 `i` 间房屋中放有 `nums[i]` 美元。

另给你一个整数 `k` ，表示窃贼将会窃取的 __最少__ 房屋数。小偷总能窃取至少 `k` 间房屋。

返回小偷的 最小 窃取能力。

__示例:__

示例 1：

```text
输入：nums = [2,3,5,9], k = 2
输出：5
解释：
小偷窃取至少 2 间房屋，共有 3 种方式：
```

- 窃取下标 0 和 2 处的房屋，窃取能力为 max(nums[0], nums[2]) = 5 。
- 窃取下标 0 和 3 处的房屋，窃取能力为 max(nums[0], nums[3]) = 9 。
- 窃取下标 1 和 3 处的房屋，窃取能力为 max(nums[1], nums[3]) = 9 。

因此，返回 min(5, 9, 9) = 5 。

示例 2：

```text
输入：nums = [2,7,9,3,1], k = 2
输出：2
解释：共有 7 种窃取方式。窃取能力最小的情况所对应的方式是窃取下标 0 和 4 处的房屋。返回 max(nums[0], nums[4]) = 2 。
```

__提示：__

- `1 <= nums.length <= 10 ^ 5`
- `1 <= nums[i] <= 10 ^ 9`
- `1 <= k <= (nums.length + 1)/2`

__思路:__

```text
二分
答案一定为 nums 中的某个值
[left, right] 应该为 [min(nums), max(nums)]
设 dp[i] 为不大于 mid 的 nums[:i] 的取的元素数
dp[i] = max(dp[i], dp[i - 2] + 1)
由于 dp[i] 只与 dp[i - 2] 有关可以优化为两个变量
当当前元素 x > mid 时, 一定不能取, a = b
否则 b 取 max(b, a + 1), a 取 b
最后返回 b
将 b 与 k 比较, 如果拿够了 k 个, 说明 mid 足够大, right = mid
否则说明 left 不够 left = mid + 1
最后返回 right
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int minCapability(vector<int>& nums, int k) 
    {
        int left = *min_element(nums.begin(), nums.end()), right = *max_element(nums.begin(), nums.end());
        while (left < right) 
        {
            int mid = left + ((right - left) >> 1), a = 0, b = 0, t = 0;
            for (const auto& x : nums) 
            {
                if (x > mid) a = b;
                else 
                {
                    t = b;
                    b = max(b, a + 1);
                    a = t;
                }
            }
            if (b < k) left = mid + 1;
            else right = mid;
        }
        return right;
    }
};
```

__Java__:

```Java
class Solution {
    public int minCapability(int[] nums, int k) {
        int left = Arrays.stream(nums).min().getAsInt(), right = Arrays.stream(nums).max().getAsInt();
        while (left < right) {
            int mid = left + ((right - left) >>> 1), a = 0, b = 0, t = 0;
            for (int x : nums) {
                if (x > mid) a = b;
                else {
                    t = b;
                    b = Math.max(b, a + 1);
                    a = t;
                }
            }
            if (b < k) left = mid + 1;
            else right = mid;
        }
        return right;
    }
}
```

__Python__:

```Python
class Solution:
    def minCapability(self, nums: List[int], k: int) -> int:
        def check(mid: int) -> int:
            a = b = 0
            for x in nums:
                if x > mid:
                    a = b
                else:
                    a, b = b, max(b, a + 1)
            return b
        return bisect_left(range(max(nums)), k, key=check)
```
