# 2149 Rearrange Array Elements by Sign 按符号重排数组

__Description:__

You are given a __0-indexed__ integer array `nums` of __even__ length consisting of an __equal__ number of positive and negative integers.

You should return the array of nums such that the the array follows the given conditions:

Return the modified array after rearranging the elements to satisfy the aforementioned conditions.

__Example:__

Example 1:

```text
Input: nums = [3,1,-2,-5,2,-4]
Output: [3,-2,1,-5,2,-4]
Explanation:
The positive integers in nums are [3,1,2]. The negative integers are [-2,-5,-4].
The only possible way to rearrange them such that they satisfy all conditions is [3,-2,1,-5,2,-4].
Other ways such as [1,-2,2,-5,3,-4], [3,1,2,-2,-5,-4], [-2,3,-5,1,-4,2] are incorrect because they do not satisfy one or more conditions.
```

Example 2:

```text
Input: nums = [-1,1]
Output: [1,-1]
Explanation:
1 is the only positive integer and -1 the only negative integer in nums.
So nums is rearranged to [1,-1].
```

__Constraints:__

- `2 <= nums.length <= 2 * 10 ^ 5`
- `nums.length` is __even__
- `1 <= |nums[i]| <= 10 ^ 5`
- `nums` consists of __equal__ number of positive and negative integers.

__题目描述:__

给你一个下标从 __0__ 开始的整数数组 `nums` ，数组长度为 __偶数__ ，由数目 __相等__ 的正整数和负整数组成。

你需要返回满足下述条件的数组 `nums`:

重排元素满足上述条件后，返回修改后的数组。

__示例:__

示例 1：

```text
输入：nums = [3,1,-2,-5,2,-4]
输出：[3,-2,1,-5,2,-4]
解释：
nums 中的正整数是 [3,1,2] ，负整数是 [-2,-5,-4] 。
重排的唯一可行方案是 [3,-2,1,-5,2,-4]，能满足所有条件。
像 [1,-2,2,-5,3,-4]、[3,1,2,-2,-5,-4]、[-2,3,-5,1,-4,2] 这样的其他方案是不正确的，因为不满足一个或者多个条件。
```

示例 2：

```text
输入：nums = [-1,1]
输出：[1,-1]
解释：
1 是 nums 中唯一一个正整数，-1 是 nums 中唯一一个负整数。
所以 nums 重排为 [1,-1] 。
```

__提示：__

- `2 <= nums.length <= 2 * 10 ^ 5`
- `nums.length` 是 __偶数__
- `1 <= |nums[i]| <= 10 ^ 5`
- `nums` 由 __相等__ 数量的正整数和负整数组成

不需要原地进行修改。

__思路:__

```text
双指针
按照正负将数字放到指针对应的位置即可
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<int> rearrangeArray(vector<int>& nums) 
    {
        int n = nums.size(), i = 0, j = 1;
        vector<int> result(n);
        for (int k = 0; k < n; k++) 
        {
            if (nums[k] > 0) 
            {
                result[i] = nums[k];
                i += 2;
            } 
            else 
            {
                result[j] = nums[k];
                j += 2;
            }
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int[] rearrangeArray(int[] nums) {
        int n = nums.length, result[] = new int[n], i = 0, j = 1;
        for (int k = 0; k < n; k++) {
            if (nums[k] > 0) {
                result[i] = nums[k];
                i += 2;
            } else {
                result[j] = nums[k];
                j += 2;
            }
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def rearrangeArray(self, nums: List[int]) -> List[int]:
        return [*chain(*zip((x for x in nums if x > 0), (x for x in nums if x < 0)))]
```
