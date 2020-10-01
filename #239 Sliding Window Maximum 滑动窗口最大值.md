__Description__:
Given an array nums, there is a sliding window of size k which is moving from the very left of the array to the very right. You can only see the k numbers in the window. Each time the sliding window moves right by one position. Return the max sliding window.

__Follow up:__
Could you solve it in linear time?

__Example:__

Input: nums = [1,3,-1,-3,5,3,6,7], and k = 3
Output: [3,3,5,5,6,7] 
Explanation: 
```
Window position                Max
---------------               -----
[1  3  -1] -3  5  3  6  7       3
 1 [3  -1  -3] 5  3  6  7       3
 1  3 [-1  -3  5] 3  6  7       5
 1  3  -1 [-3  5  3] 6  7       5
 1  3  -1  -3 [5  3  6] 7       6
 1  3  -1  -3  5 [3  6  7]      7
``` 

__Constraints:__

1 <= nums.length <= 10^5
-10^4 <= nums[i] <= 10^4
1 <= k <= nums.length

__题目描述__:
给定一个数组 nums，有一个大小为 k 的滑动窗口从数组的最左侧移动到数组的最右侧。你只可以看到在滑动窗口内的 k 个数字。滑动窗口每次只向右移动一位。

返回滑动窗口中的最大值。

__进阶：__

你能在线性时间复杂度内解决此题吗？

__示例 :__

输入: nums = [1,3,-1,-3,5,3,6,7], 和 k = 3
输出: [3,3,5,5,6,7] 
解释: 
```
  滑动窗口的位置                最大值
---------------               -----
[1  3  -1] -3  5  3  6  7       3
 1 [3  -1  -3] 5  3  6  7       3
 1  3 [-1  -3  5] 3  6  7       5
 1  3  -1 [-3  5  3] 6  7       5
 1  3  -1  -3 [5  3  6] 7       6
 1  3  -1  -3  5 [3  6  7]      7
 ```

__提示：__

1 <= nums.length <= 10^5
-10^4 <= nums[i] <= 10^4
1 <= k <= nums.length

__思路__:
用一个队列记录最大值
1. 当队首的元素超出滑动窗口, 弹出
2. 小于新加入的元素, 弹出全部队尾元素(保证队首元素最大)
时间复杂度O(n), 空间复杂度O(1)

__代码__:
__C++__:
```C++
class Solution 
{
public:
    vector<int> maxSlidingWindow(vector<int>& nums, int k) 
    {
        vector<int> result, d;
        for (int i = 0; i < nums.size(); i++)
        {
            if (!d.empty() and d.front() == i - k) d.erase(d.begin());
            while (!d.empty() and nums[i] > nums[d.back()]) d.erase(d.end() - 1);
            d.push_back(i);
            if (i > k - 2) result.push_back(nums[d.front()]);
        }
        return result;
    }
};
```

__Java__:
```Java
class Solution {
    public int[] maxSlidingWindow(int[] nums, int k) {
        int result[] = new int[nums.length - k + 1];
        List<Integer> list = new ArrayList<>();
        for (int i = 0; i < nums.length; i++) {
            if (!list.isEmpty() && list.get(0) == i - k) list.remove(0);
            while (!list.isEmpty() && nums[i] > nums[list.get(list.size() - 1)]) list.remove(list.size() - 1);
            list.add(i);
            if (i > k - 2) result[i - k + 1] = nums[list.get(0)];
        }
        return result;
    }
}
```

__Python__:
```Python
class Solution:
    def maxSlidingWindow(self, nums: List[int], k: int) -> List[int]:
        result, l = [], []
        for i in range(len(nums)):
            if l and l[0] == i - k:
                l.pop(0)
            while l and nums[i] > nums[l[-1]]:
                l.pop(-1)
            l.append(i)
            if i > k - 2:
                result.append(nums[l[0]])
        return result
```