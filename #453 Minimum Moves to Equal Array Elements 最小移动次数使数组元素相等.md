# 453 Minimum Moves to Equal Array Elements 最小移动次数使数组元素相等

__Description__:
Given a non-empty integer array of size n, find the minimum number of moves required to make all array elements equal, where a move is incrementing n - 1 elements by 1.

__Example:__

Input:
[1,2,3]

Output:
3

__Explanation:__
Only three moves are needed (remember each move increments two elements):

[1,2,3]  =>  [2,3,3]  =>  [3,4,3]  =>  [4,4,4]

__题目描述__:
给定一个长度为 n 的非空整数数组，找到让数组所有元素相等的最小移动次数。每次移动可以使 n - 1 个元素增加 1。

__示例:__

输入:
[1,2,3]

输出:
3

__解释:__
只需要3次移动（注意每次移动会增加两个元素的值）：

[1,2,3]  =>  [2,3,3]  =>  [3,4,3]  =>  [4,4,4]

__思路__:

假设最终所有元素均为 x, 那么最小的数加到 x用了 x - min(nums)次, 则有 n - 1次增加了 x - min(nums)
另外, 最后所有元素增加到了 x, 可以得到方程
增加量 = 最终元素和 - 初始元素和, 即:
(x - min(nums)) \* (n - 1) = x \* n - sum(nums)
解得 x = min(nums)(1 - n) + sum(nums)
增加次数为 x - min(nums) = sum(nums) - min(nums) \* n
时间复杂度O(n), 空间复杂度O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int minMoves(vector<int>& nums) 
    {
        // *min_element(nums.begin(),nums.end()) 求最小值
        // *max_element(nums.begin(),nums.end()) 求最大值
        int result = 0, min_num = *min_element(nums.begin(),nums.end());
        for (int num : nums) result += num - min_num;
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int minMoves(int[] nums) {
        int result = 0, min = Arrays.stream(nums).min().getAsInt();
        for (int num : nums) result += num - min;
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def minMoves(self, nums: List[int]) -> int:
        return sum(nums) - min(nums) * len(nums)
```
