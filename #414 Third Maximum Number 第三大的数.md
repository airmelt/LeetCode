# 414 Third Maximum Number 第三大的数

__Description__:
Given a non-empty array of integers, return the third maximum number in this array. If it does not exist, return the maximum number. The time complexity must be in O(n).

__Example:__

Example 1:
Input: [3, 2, 1]

Output: 1

Explanation: The third maximum is 1.

Example 2:
Input: [1, 2]

Output: 2

Explanation: The third maximum does not exist, so the maximum (2) is returned instead.

Example 3:
Input: [2, 2, 3, 1]

Output: 1

__Explanation:__ Note that the third maximum here means the third maximum distinct number.
Both numbers with value 2 are both considered as second maximum.

__题目描述__:
给定一个非空数组，返回此数组中第三大的数。如果不存在，则返回数组中最大的数。要求算法时间复杂度必须是O(n)。

__示例：__

示例 1:

输入: [3, 2, 1]

输出: 1

解释: 第三大的数是 1.

示例 2:

输入: [1, 2]

输出: 2

解释: 第三大的数不存在, 所以返回最大的数 2 .

示例 3:

输入: [2, 2, 3, 1]

输出: 1

解释: 注意，要求返回第三大的数，是指第三大且唯一出现的数。
存在两个值为2的数，它们都排第二。

__思路__:

需要注意边界条件及相同值不用更新, 所以不要使用 int型最小值
采用别的方式判断是否更新最大的三个值
时间复杂度O(n), 空间复杂度O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int thirdMax(vector<int>& nums) 
    {
        int first = nums[0], second = nums[0], third = nums[0];
        for (int num : nums) 
        {
            if (num > first) 
            {
                third = second;
                second = first;
                first = num;
            } 
            else if (num < first and (num > second || second == first)) 
            {
                third = second;
                second = num;
            } 
            else if (num < second and (num > third || third == second || third == first)) 
            {
                third = num;
            }
        }
        if (first > second and second > third) return third;
        return first;        
    }
};
```

__Java__:

```Java
class Solution {
    public int thirdMax(int[] nums) {
        int first = nums[0], second = nums[0], third = nums[0];
        for (int num : nums) {
            if (num > first) {
                third = second;
                second = first;
                first = num;
            } else if (num < first && (num > second || second == first)) {
                third = second;
                second = num;
            } else if (num < second && (num > third || third == second || third == first)) {
                third = num;
            }
        }
        if (first > second && second > third) return third;
        return first;
    }
}
```

__Python__:

```Python
class Solution:
    def thirdMax(self, nums: List[int]) -> int:
        first = second = third =  nums[0]
        for num in nums:
            if num > first:
                third, second, first = second, first, num
            elif first > num and (num > second or second == first):
                third, second = second, num
            elif second > num and (num > third or third == first or third == second):
                third = num
        return third if first > second > third else first
```
