# 2148 Count Elements With Strictly Smaller and Greater Elements 元素计数

__Description:__

Given an integer array `nums`, return _the number of elements that have __both__ a strictly smaller and a strictly greater element appear in_ `nums`.

__Example:__

Example 1:

```text
Input:  nums = [11,7,2,15]
Output:  2
Explanation:  The element 7 has the element 2 strictly smaller than it and the element 11 strictly greater than it.
Element 11 has element 7 strictly smaller than it and element 15 strictly greater than it.
In total there are 2 elements having both a strictly smaller and a strictly greater element appear in `nums`.
```

Example 2:

```text
Input:  nums = [-3,3,3,90]
Output:  2
Explanation:  The element 3 has the element -3 strictly smaller than it and the element 90 strictly greater than it.
Since there are two elements with the value 3, in total there are 2 elements having both a strictly smaller and a strictly greater element appear in `nums`.
```

__Constraints:__

- `1 <= nums.length <= 100`
- `-10 ^ 5 <= nums[i] <= 10 ^ 5`

__题目描述:__

给你一个整数数组 `nums` ，统计并返回在 `nums` 中同时至少具有一个严格较小元素和一个严格较大元素的元素数目。

__示例:__

示例 1：

```text
输入：nums = [11,7,2,15]
输出：2
解释：元素 7 ：严格较小元素是元素 2 ，严格较大元素是元素 11 。
元素 11 ：严格较小元素是元素 7 ，严格较大元素是元素 15 。
总计有 2 个元素都满足在 nums 中同时存在一个严格较小元素和一个严格较大元素。
```

示例 2：

```text
输入：nums = [-3,3,3,90]
输出：2
解释：元素 3 ：严格较小元素是元素 -3 ，严格较大元素是元素 90 。
由于有两个元素的值为 3 ，总计有 2 个元素都满足在 nums 中同时存在一个严格较小元素和一个严格较大元素。
```

__提示：__

- `1 <= nums.length <= 100`
- `-10 ^ 5 <= nums[i] <= 10 ^ 5`

__思路:__

```text
模拟
可以统计所有在 (min(nums), max(nums)) 区间内的数字的个数
也可以先找到最值, 如果最大值和最小值相等, 返回 0
否则返回 nums 长度减去最大值和最小值的个数
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int countElements(vector<int>& nums) 
    {
        return max_element(nums.begin(), nums.end()) == min_element(nums.begin(), nums.end()) ? 0 : nums.size() - count_if(nums.begin(), nums.end(), [&](auto a){ return a == *max_element(nums.begin(), nums.end()); }) - count_if(nums.begin(), nums.end(), [&](auto a){ return a == *min_element(nums.begin(), nums.end()); });
    }
};
```

__Java__:

```Java
class Solution {
    public int countElements(int[] nums) {
        return Arrays.stream(nums).max().getAsInt() == Arrays.stream(nums).min().getAsInt() ? 0 : (int)(nums.length - Arrays.stream(nums).filter(i -> i == Arrays.stream(nums).max().getAsInt()).count() - Arrays.stream(nums).filter(i -> i == Arrays.stream(nums).min().getAsInt()).count());
    }
}
```

__Python__:

```Python
class Solution:
    def countElements(self, nums: List[int]) -> int:
        return sum(min(nums) < num < max(nums) for num in nums)
```
