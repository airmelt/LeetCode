# 1929 Concatenation of Array 数组串联

__Description:__

Given an integer array `nums` of length `n`, you want to create an array `ans` of length `2n` where `ans[i] == nums[i]` and `ans[i + n] == nums[i]` for `0 <= i < n` (__0-indexed__).

Specifically, `ans` is the __concatenation__ of two `nums` arrays.

Return _the array_ `ans`.

__Example:__

Example 1:

```text
Input: nums = [1,2,1]
Output: [1,2,1,1,2,1]
Explanation: The array ans is formed as follows:
- ans = [nums[0],nums[1],nums[2],nums[0],nums[1],nums[2]]
- ans = [1,2,1,1,2,1]
```

Example 2:

```text
Input: nums = [1,3,2,1]
Output: [1,3,2,1,1,3,2,1]
Explanation: The array ans is formed as follows:
- ans = [nums[0],nums[1],nums[2],nums[3],nums[0],nums[1],nums[2],nums[3]]
- ans = [1,3,2,1,1,3,2,1]
```

__Constraints:__

- `n == nums.length`
- `1 <= n <= 1000`
- `1 <= nums[i] <= 1000`

__题目描述:__

给你一个长度为 `n` 的整数数组 `nums` 。请你构建一个长度为 `2n` 的答案数组 `ans` ，数组下标 __从 0 开始计数__ ，对于所有 `0 <= i < n` 的 `i` ，满足下述所有要求:

- `ans[i] == nums[i]`
- `ans[i + n] == nums[i]`

具体而言， `ans` 由两个 `nums` 数组 __串联__ 形成。

返回数组 `ans` 。

__示例:__

示例 1：

```text
输入：nums = [1,2,1]
输出：[1,2,1,1,2,1]
解释：数组 ans 按下述方式形成：
- ans = [nums[0],nums[1],nums[2],nums[0],nums[1],nums[2]]
- ans = [1,2,1,1,2,1]
```

示例 2：

```text
输入：nums = [1,3,2,1]
输出：[1,3,2,1,1,3,2,1]
解释：数组 ans 按下述方式形成：
- ans = [nums[0],nums[1],nums[2],nums[3],nums[0],nums[1],nums[2],nums[3]]
- ans = [1,3,2,1,1,3,2,1]
```

__提示：__

- `n == nums.length`
- `1 <= n <= 1000`
- `1 <= nums[i] <= 1000`

__思路:__

```text
模拟
直接在 nums 后面添加对应 nums 即可















































时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<int> getConcatenation(vector<int>& nums) 
    {
        for (int i = 0, n = nums.size(); i < n; i++) nums.emplace_back(nums[i]);
        return nums;
    }
};
```

__Java__:

```Java
class Solution {
    public int[] getConcatenation(int[] nums) {
        int n = nums.length, result[] = new int[n << 1];
        for (int i = 0; i < n; i++) result[i] = result[i + n] = nums[i];
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def getConcatenation(self, nums: List[int]) -> List[int]:
        return nums + nums
```
