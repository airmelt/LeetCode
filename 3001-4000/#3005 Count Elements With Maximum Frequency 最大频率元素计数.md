# 3005 Count Elements With Maximum Frequency 最大频率元素计数

__Description:__

You are given an array `nums` consisting of __positive__ integers.

Return _the __total frequencies__ of elements in_ `nums` _such that those elements all have the __maximum__ frequency_.

The frequency of an element is the number of occurrences of that element in the array.

__Example:__

Example 1:

```text
Input: nums = [1,2,2,3,1,4]
Output: 4
Explanation: The elements 1 and 2 have a frequency of 2 which is the maximum frequency in the array.
So the number of elements in the array with maximum frequency is 4.
```

Example 2:

```text
Input: nums = [1,2,3,4,5]
Output: 5
Explanation: All elements of the array have a frequency of 1 which is the maximum.
So the number of elements in the array with maximum frequency is 5.
```

__Constraints:__

- `1 <= nums.length <= 100`
- `1 <= nums[i] <= 100`

__题目描述:__

给你一个由 __正整数__ 组成的数组 `nums` 。

返回数组 `nums` 中所有具有 __最大__ 频率的元素的 __总频率__ 。

元素的 频率 是指该元素在数组中出现的次数。

__示例:__

示例 1：

```text
输入：nums = [1,2,2,3,1,4]
输出：4
解释：元素 1 和 2 的频率为 2 ，是数组中的最大频率。
因此具有最大频率的元素在数组中的数量是 4 。
```

示例 2：

```text
输入：nums = [1,2,3,4,5]
输出：5
解释：数组中的所有元素的频率都为 1 ，是最大频率。
因此具有最大频率的元素在数组中的数量是 5 。
```

__提示：__

- `1 <= nums.length <= 100`
- `1 <= nums[i] <= 100`

__思路:__

```text
模拟
用哈希集合记录每个元素出现的次数
如果最大次数更新那么修改最大值和结果都为当前值
如果其他的数也达到最大次数则累加
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int maxFrequencyElements(vector<int>& nums) 
    {
        unordered_map<int, int> m;
        int mx = 0, result = 0;
        for (auto& num : nums) 
        {
            if ((num = ++m[num]) > mx) result = mx = num;
            else if (num == mx) result += num;
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int maxFrequencyElements(int[] nums) {
        var map = new HashMap<Integer, Integer>();
        int mx = 0, result = 0;
        for (int num : nums) {
            if ((num = map.merge(num, 1, Integer::sum)) > mx) result = mx = num;
            else if (num == mx) result += num;
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def maxFrequencyElements(self, nums: List[int]) -> int:
        return sum(v for v in Counter(nums).values() if v == max(Counter(nums).values()))
```
