# 659 Split Array into Consecutive Subsequences 分割数组为连续子序列

__Description__:
Given an integer array nums that is sorted in ascending order, return true if and only if you can split it into one or more subsequences such that each subsequence consists of consecutive integers and has a length of at least 3.

__Example:__

Example 1:

Input: nums = [1,2,3,3,4,5]
Output: true
Explanation:
You can split them into two consecutive subsequences :
1, 2, 3
3, 4, 5

Example 2:

Input: nums = [1,2,3,3,4,4,5,5]
Output: true
Explanation:
You can split them into two consecutive subsequences :
1, 2, 3, 4, 5
3, 4, 5

Example 3:

Input: nums = [1,2,3,4,4,5]
Output: false

__Constraints:__

1 <= nums.length <= 10^4
-1000 <= nums[i] <= 1000
nums is sorted in an ascending order.

__题目描述__:
给你一个按升序排序的整数数组 num（可能包含重复数字），请你将它们分割成一个或多个长度至少为 3 的子序列，其中每个子序列都由连续整数组成。

如果可以完成上述分割，则返回 true ；否则，返回 false 。

__示例 :__

示例 1：

输入: [1,2,3,3,4,5]
输出: True
解释:
你可以分割出这样两个连续子序列 :
1, 2, 3
3, 4, 5

示例 2：

输入: [1,2,3,3,4,4,5,5]
输出: True
解释:
你可以分割出这样两个连续子序列 :
1, 2, 3, 4, 5
3, 4, 5

示例 3：

输入: [1,2,3,4,4,5]
输出: False

__提示:__

1 <= nums.length <= 10000

__思路__:

贪心
对于 nums 数组中的一个 num
考虑到尽量加入一个已经存在的序列中, 比新建一个序列更好
要么加入以 num - 1 为结尾的序列, 这时需要已经存在一个 num - 1 结尾的序列
要么新建一个以 num 开头的序列, 这时 num + 1, num + 2 必须在序列中, 否则建立失败
用一个哈希表记录数组元素的个数, 另一个哈希表记录以某个 num 结尾的长度不小于 3 的子序列的个数
时间复杂度 O(n), 空间复杂度 O(n)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    bool isPossible(vector<int>& nums) 
    {
        map<int, int> count, end;
        for (const auto &num : nums) ++count[num];
        for (const auto &num : nums) 
        {
            if (!count[num]) continue;
            --count[num];
            if (end[num - 1]) 
            {
                --end[num - 1];
                ++end[num];
            } 
            else if (count[num + 1] and count[num + 2]) 
            {
                --count[num + 1];
                --count[num + 2];
                ++end[num + 2];
            } 
            else return false;
        }
        return true;
    }
};
```

__Java__:

```Java
class Solution {
    public boolean isPossible(int[] nums) {
        Map<Integer, Integer> count = new HashMap<>(), end = new HashMap<>();
        for (int num : nums) count.put(num, count.getOrDefault(num, 0) + 1);
        for (int num : nums) {
            if (count.get(num) == 0) continue;
            count.put(num, count.get(num) - 1);
            if (end.containsKey(num - 1) && end.get(num - 1) > 0) {
                end.put(num - 1, end.get(num - 1) - 1);
                end.put(num, end.getOrDefault(num, 0) + 1);
            } else if (count.containsKey(num + 1) && count.get(num + 1) > 0 && count.containsKey(num + 2) && count.get(num + 2) > 0) {
                count.put(num + 1, count.get(num + 1) - 1);
                count.put(num + 2, count.get(num + 2) - 1);
                end.put(num + 2, end.getOrDefault(num + 2, 0) + 1);
            } else return false;
        }
        return true;
    }
}
```

__Python__:

```Python
class Solution:
    def isPossible(self, nums: List[int]) -> bool:
        count, end = dict(Counter(nums)), {}
        for num in nums:
            if not count[num]:
                continue
            count[num] -= 1
            if num - 1 in end and end[num - 1] > 0:
                end[num - 1] -= 1
                end[num] = end.get(num, 0) + 1
            elif num + 1 in count and count[num + 1] > 0 and num + 2 in count and count[num + 2] > 0:
                count[num + 1] -= 1
                count[num + 2] -= 1
                end[num + 2] = end.get(num + 2, 0) + 1
            else:
                return False
        return True
```
