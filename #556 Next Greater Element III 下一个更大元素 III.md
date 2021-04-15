# 556 Next Greater Element III 下一个更大元素 III

__Description__:
Given a positive integer n, find the smallest integer which has exactly the same digits existing in the integer n and is greater in value than n. If no such positive integer exists, return -1.

Note that the returned integer should fit in 32-bit integer, if there is a valid answer but it does not fit in 32-bit integer, return -1.

__Example:__

Example 1:

Input: n = 12
Output: 21

Example 2:

Input: n = 21
Output: -1

__Constraints:__

1 <= n <= 2^31 - 1

__题目描述__:
给你一个正整数 n ，请你找出符合条件的最小整数，其由重新排列 n 中存在的每位数字组成，并且其值大于 n 。如果不存在这样的正整数，则返回 -1 。

注意 ，返回的整数应当是一个 32 位整数 ，如果存在满足题意的答案，但不是 32 位整数 ，同样返回 -1 。

__示例 :__

示例 1：

输入：n = 12
输出：21

示例 2：

输入：n = 21
输出：-1

__提示:__

1 <= n <= 2^31 - 1

__思路__:

参考[LeetCode #31 Next Permutation 下一个排列](https://www.jianshu.com/p/ad45640f4198)
从后往前找到第一个逆序的数对
交换之后将逆序数对之后的元素逆序
时间复杂度 O(n), 空间复杂度 O(n)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int nextGreaterElement(int n) 
    {
        string str = to_string(n);
        next_permutation(str.begin(), str.end());
        long num = stol(str);
        return num > INT_MAX || num <= n ? -1 : num;
    }
};
```

__Java__:

```Java
class Solution {
    public int nextGreaterElement(int n) {
        char[] nums = ("" + n).toCharArray();
        int i = nums.length - 2, j = nums.length - 1;
        while (i >= 0 && nums[i + 1] <= nums[i]) --i;
        if (i < 0) return -1;
        while (j >= 0 && nums[j] <= nums[i]) --j;
        swap(nums, i, j);
        reverse(nums, i + 1);
        try {
            return Integer.parseInt(new String(nums));
        } catch (Exception e) {
            return -1;
        }
    }
    
    private void reverse(char[] nums, int start) {
        int i = start, j = nums.length - 1;
        while (i < j) swap(nums, i++, j--);
    }
    
    private void swap(char[] nums, int i, int j) {
        char temp = nums[i];
        nums[i] = nums[j];
        nums[j] = temp;
    }
}
```

__Python__:

```Python
class Solution:
    def nextGreaterElement(self, n: int) -> int:
        nums, m = [x for x in str(n)], len(str(n))
        for i in range(m - 2, -1, -1):
            if nums[i] < nums[i + 1]:
                for j in range(m - 1, i, -1):
                    if nums[j] > nums[i]:
                        nums[i], nums[j] = nums[j], nums[i]
                        nums[i + 1:] = nums[i + 1:][::-1]
                        break
                break
        return result if (result := int(''.join(nums))) < 2 ** 31 and result != n else -1
```
