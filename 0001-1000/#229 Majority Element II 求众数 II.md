# 229 Majority Element II 求众数 II

__Description__:
Given an integer array of size n, find all elements that appear more than ⌊ n/3 ⌋ times.

__Note:__
The algorithm should run in linear time and in O(1) space.

__Example:__

Example 1:

Input: [3,2,3]
Output: [3]

Example 2:

Input: [1,1,1,3,3,2,2,2]
Output: [1,2]

__题目描述__:
给定一个大小为 n 的数组，找出其中所有出现超过 ⌊ n/3 ⌋ 次的元素。

__说明：__
要求算法的时间复杂度为 O(n)，空间复杂度为 O(1)。

__示例 :__

示例 1:

输入: [3,2,3]
输出: [3]

示例 2:

输入: [1,1,1,3,3,2,2,2]
输出: [1,2]

__思路__:

1. 排序之后找到最多的两个元素, 看是否出现次数超过 n / 3
时间复杂度O(nlgn), 空间复杂度O(1)
2. 由于需要超过 n / 3的出现次数
最多有两个备选数字, 不妨设为 nums[0]和第一个与 nums[0]不相等的数
之后如果出现相同数字, 则 +1, 否则 -1, 如果计数器到 0就更换备选数字
然后遍历列表, 看备选数字是否出现次数达到要求
时间复杂度O(n), 空间复杂度O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    vector<int> majorityElement(vector<int>& nums) 
    {
        vector<int> result;
        int a = 0, b = 0, x = 0, y = 0, count = 0;
        for (auto num : nums)
        {
            if ((x == 0 or num == a) and num != b)
            {
                ++x;
                a = num;
            }
            else if (y == 0 or num == b)
            {
                ++y;
                b = num;
            }
            else
            {
                --x;
                --y;
            }
        }
        for (auto num : nums) if (a == num) ++count;
        if (count > nums.size() / 3) result.push_back(a);
        count = 0;
        for (auto num : nums) if (b == num) ++count;
        if (count > nums.size() / 3 and a != b) result.push_back(b);
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public List<Integer> majorityElement(int[] nums) {
        List<Integer> result = new ArrayList<>();
        int a = 0, b = 0, x = 0, y = 0, count = 0;
        for (int num : nums) {
            if ((x == 0 || num == a) && num != b) {
                ++x;
                a = num;
            } else if (y == 0 || num == b) {
                ++y;
                b = num;
            } else {
                --x;
                --y;
            }
        }
        for (int num : nums) if (a == num) ++count;
        if (count > nums.length / 3) result.add(a);
        count = 0;
        for (int num : nums) if (b == num) ++count;
        if (count > nums.length / 3 && a != b) result.add(b);
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def majorityElement(self, nums: List[int]) -> List[int]:
        return [k for k, v in Counter(nums).items() if v > len(nums) // 3]
```
