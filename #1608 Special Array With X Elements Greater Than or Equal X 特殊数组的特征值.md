# 1608 Special Array With X Elements Greater Than or Equal X 特殊数组的特征值

__Description:__

You are given an array `nums` of non-negative integers. `nums` is considered __special__ if there exists a number `x` such that there are __exactly__ `x` numbers in `nums` that are __greater than or equal to__ `x`.

Notice that `x` __does not__ have to be an element in `nums`.

Return `x` _if the array is __special__, otherwise, return_ `-1`. It can be proven that if `nums` is special, the value for `x` is __unique__.

__Example:__

Example 1:

```text
Input: nums = [3,5]
Output: 2
Explanation: There are 2 values (3 and 5) that are greater than or equal to 2.
```

Example 2:

```text
Input: nums = [0,0]
Output: -1
Explanation: No numbers fit the criteria for x.
If x = 0, there should be 0 numbers >= x, but there are 2.
If x = 1, there should be 1 number >= x, but there are 0.
If x = 2, there should be 2 numbers >= x, but there are 0.
x cannot be greater since there are only 2 numbers in nums.
```

Example 3:

```text
Input: nums = [0,4,3,0,4]
Output: 3
Explanation: There are 3 values that are greater than or equal to 3.
```

__Constraints:__

- `1 <= nums.length <= 100`
- `0 <= nums[i] <= 1000`

__题目描述:__

给你一个非负整数数组 `nums` 。如果存在一个数 `x` ，使得 `nums` 中恰好有 `x` 个元素 __大于或者等于__ `x` ，那么就称 `nums` 是一个 __特殊数组__ ，而 `x` 是该数组的 __特征值__ 。

注意: `x` __不必__ 是 `nums` 的中的元素。

如果数组 `nums` 是一个 __特殊数组__ ，请返回它的特征值 `x` 。否则，返回 `-1` 。可以证明的是，如果 `nums` 是特殊数组，那么其特征值 `x` 是 __唯一的__ 。

__示例:__

示例 1：

```text
输入：nums = [3,5]
输出：2
解释：有 2 个元素（3 和 5）大于或等于 2 。
```

示例 2：

```text
输入：nums = [0,0]
输出：-1
解释：没有满足题目要求的特殊数组，故而也不存在特征值 x 。
如果 x = 0，应该有 0 个元素 >= x，但实际有 2 个。
如果 x = 1，应该有 1 个元素 >= x，但实际有 0 个。
如果 x = 2，应该有 2 个元素 >= x，但实际有 0 个。
x 不能取更大的值，因为 nums 中只有两个元素。
```

示例 3：

```text
输入：nums = [0,4,3,0,4]
输出：3
解释：有 3 个元素大于或等于 3 。
```

示例 4：

```text
输入：nums = [3,6,7,7,0]
输出：-1
```

__提示：__

- `1 <= nums.length <= 100`
- `0 <= nums[i] <= 1000`

__思路:__

```text
虽然给的 nums 中元素的范围为 0 到 1000
但是由于数组 nums 的长度最多为 100, 实际上 x 到取值范围为 [0, 100]
1. 计数
先统计 nums 中每个数出现的次数
逆序检查出现次数的前缀和, 前缀和和当前元素相等即可返回
时间复杂度为 O(N), 空间复杂度为 O(N)
2. 排序 ➕ 二分
先逆序排序
再通过二分查找找到满足题意的数
时间复杂度为 O(NlogN), 空间复杂度为 O(lgN)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int specialArray(vector<int>& nums) 
    {
        sort(nums.begin(), nums.end(), greater<int>());
        for (int i = 1, n = nums.size(); i <= n; i++) if (nums[i - 1] >= i and (i == n or nums[i] < i)) return i;
        return -1;
    }
};
```

__Java__:

```Java
class Solution {
    public int specialArray(int[] nums) {
        Arrays.sort(nums);
        for (int x = 0, n = nums.length; x < 1010; x++) {
            int l = 0, r = n - 1;
            while (l < r) {
                int mid = l + ((r - l) >> 1);
                if (nums[mid] >= x) r = mid;
                else l = mid + 1;
            }
            if (nums[r] >= x && x == n - r) return x;
        }
        return -1;
    }
}
```

__Python__:

```Python
class Solution:
    def specialArray(self, nums: List[int]) -> int:
        return min((x for x in range(101) if sum(i >= x for i in nums) == x), default=-1)
```
