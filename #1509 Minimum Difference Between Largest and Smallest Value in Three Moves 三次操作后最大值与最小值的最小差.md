# 1509 Minimum Difference Between Largest and Smallest Value in Three Moves 三次操作后最大值与最小值的最小差

__Description:__

You are given an integer array `nums`.

In one move, you can choose one element of `nums` and change it to __any value__.

Return _the minimum difference between the largest and smallest value of `nums` __after performing at most three moves___.

__Example:__

Example 1:

```text
Input: nums = [5,3,2,4]
Output: 0
Explanation: We can make at most 3 moves.
In the first move, change 2 to 3. nums becomes [5,3,3,4].
In the second move, change 4 to 3. nums becomes [5,3,3,3].
In the third move, change 5 to 3. nums becomes [3,3,3,3].
After performing 3 moves, the difference between the minimum and maximum is 3 - 3 = 0.
```

Example 2:

```text
Input: nums = [1,5,0,10,14]
Output: 1
Explanation: We can make at most 3 moves.
In the first move, change 5 to 0. nums becomes [1,0,0,10,14].
In the second move, change 10 to 0. nums becomes [1,0,0,0,14].
In the third move, change 14 to 1. nums becomes [1,0,0,0,1].
After performing 3 moves, the difference between the minimum and maximum is 1 - 0 = 0.
It can be shown that there is no way to make the difference 0 in 3 moves.
```

Example 3:

```text
Input: nums = [3,100,20]
Output: 0
Explanation: We can make at most 3 moves.
In the first move, change 100 to 7. nums becomes [4,7,20].
In the second move, change 20 to 7. nums becomes [4,7,7].
In the third move, change 4 to 3. nums becomes [7,7,7].
After performing 3 moves, the difference between the minimum and maximum is 7 - 7 = 0.
```

__Constraints:__

- `1 <= nums.length <= 10 ^ 5`
- `-10 ^ 9 <= nums[i] <= 10 ^ 9`

__题目描述:__

给你一个数组 `nums` ，每次操作你可以选择 `nums` 中的任意一个元素并将它改成任意值。

请你返回三次操作后， `nums` 中最大值与最小值的差的最小值。

__示例:__

示例 1：

```text
输入：nums = [5,3,2,4]
输出：0
解释：将数组 [5,3,2,4] 变成 [2,2,2,2].
最大值与最小值的差为 2-2 = 0 。
```

示例 2：

```text
输入：nums = [1,5,0,10,14]
输出：1
解释：将数组 [1,5,0,10,14] 变成 [1,1,0,1,1] 。
最大值与最小值的差为 1-0 = 1 。
```

示例 3：

```text
输入：nums = [6,6,0,1,1,4,6]
输出：2
```

示例 4：

```text
输入：nums = [1,5,6,14,15]
输出：1
```

__提示：__

- `1 <= nums.length <= 10 ^ 5`
- `-10 ^ 9 <= nums[i] <= 10 ^ 9`

__思路:__

```text
首先如果数组的长度小于等于 4, 必然可以将 4 个数调整为一样大小
1. 排序
由于可以任意修改值
排序之后比较最大的 4 个数和最小的 4 个数的差值
返回最小的差值即可
时间复杂度为 O(NlogN), 空间复杂度为 O(N), 主要时间复杂度为排序
2. 记录最大值和最小值
由于只需要用到最大的 4 个数和最小的 4 个数的差值
所以可以用数组保留最大的 4 个数和最小的 4 个数
然后对应求最小的差值
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int minDifference(vector<int>& nums) 
    {
        int n = nums.size(), result = 2e9;
        if (n < 5) return 0;
        vector<int> max4(4, -1e9), min4(4, 1e9);
        for (int i = 0; i < n; i++)
        {
            int cur = 0;
            while (cur < 4 and max4[cur] > nums[i]) ++cur;
            if (cur < 4)
            {
                for (int j = 3; j > cur; j--) max4[j] = max4[j - 1];
                max4[cur] = nums[i];
            }
            cur = 0;
            while (cur < 4 and min4[cur] < nums[i]) ++cur;
            if (cur < 4)
            {
                for (int j = 3; j > cur; j--) min4[j] = min4[j - 1];
                min4[cur] = nums[i];
            }
        }
        for (int i = 0; i < 4; i++) result = min(result, max4[i] - min4[3 - i]);
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int minDifference(int[] nums) {
        int n = nums.length, lastIndex = n - 1, result = Integer.MAX_VALUE;
        if (n <= 4) return 0;
        Arrays.sort(nums);
        for (int i = 3; i > -1; i--) result = Math.min(result, nums[lastIndex--] - nums[i]);
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def minDifference(self, nums: List[int]) -> int:
        nums.sort()
        if (n := len(nums)) < 5:
            return 0
        last_index, result = n - 1, float('inf')
        for i in range(3, -1, -1):
            result = min(nums[last_index] - nums[i], result)
            last_index -= 1
        return result
```
