# 473 Matchsticks to Square 火柴拼正方形

__Description__:
Remember the story of Little Match Girl? By now, you know exactly what matchsticks the little match girl has, please find out a way you can make one square by using up all those matchsticks. You should not break any stick, but you can link them up, and each matchstick must be used exactly one time.

Your input will be several matchsticks the girl has, represented with their stick length. Your output will either be true or false, to represent whether you could make one square using all the matchsticks the little match girl has.

__Example:__

Example 1:
Input: [1,1,2,2,2]
Output: true

Explanation: You can form a square with length 2, one side of the square came two sticks with length 1.

Example 2:
Input: [3,3,3,3,4]
Output: false

Explanation: You cannot find a way to form a square with all the matchsticks.

__Note:__
The length sum of the given matchsticks is in the range of 0 to 10^9.
The length of the given matchstick array will not exceed 15.

__题目描述__:
还记得童话《卖火柴的小女孩》吗？现在，你知道小女孩有多少根火柴，请找出一种能使用所有火柴拼成一个正方形的方法。不能折断火柴，可以把火柴连接起来，并且每根火柴都要用到。

输入为小女孩拥有火柴的数目，每根火柴用其长度表示。输出即为是否能用所有的火柴拼成正方形。

__示例 :__

示例 1:

输入: [1,1,2,2,2]
输出: true

解释: 能拼成一个边长为2的正方形，每边两根火柴。

示例 2:

输入: [3,3,3,3,4]
输出: false

解释: 不能用所有火柴拼成一个正方形。

__注意:__

给定的火柴长度和在 0 到 10^9之间。
火柴数组的长度不超过15。

__思路__:

数组的总和必须要是 4的倍数, 可以用位运算加快, 数组长度必须大于或等于 4
先排序, 最大值必须小于或等于数组总和除以 4
然后尝试将 4个边填充
如果一根火柴不满足前一条边, 后面的边就不用再次尝试
时间复杂度O(n * 2 ^ n), 空间复杂度O(2 ^ n)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    bool makesquare(vector<int>& nums) 
    {
        int s = accumulate(nums.begin(), nums.end(), 0);
        if ((s & 0b11) or nums.size() < 4) return false;
        s >>= 2;
        sort(nums.rbegin(), nums.rend());
        if (s < nums.front()) return false;
        vector<int> buckets(4, 0);
        return helper(s, 0, buckets, nums);
    }
private:
    bool helper(int s, int i, vector<int>& buckets, vector<int>& nums) 
    {
        if (i == nums.size()) return true;
        for (int j = 0; j < 4; j++) 
        {
            if (buckets[j] + nums[i] <= s) 
            {
                if (j == 0 or buckets[j] != buckets[j - 1]) 
                {
                    buckets[j] += nums[i];
                    if (helper(s, i + 1, buckets, nums)) return true;
                    buckets[j] -= nums[i];
                }
            }
        }
        return false;
    }
};
```

__Java__:

```Java
class Solution {
    public boolean makesquare(int[] nums) {
        int s = 0;
        for (int num : nums) s += num;
        if ((s & 0b11) != 0 || nums.length < 4) return false;
        s >>= 2;
        nums = Arrays.stream(nums).boxed().sorted(Collections.reverseOrder()).mapToInt(Integer::intValue).toArray();
        if (s < nums[0]) return false;
        return helper(s, 0, new int[]{0, 0, 0, 0}, nums);
    }
    
    private boolean helper(int s, int i, int[] buckets, int[] nums) {
        if (i == nums.length) return true;
        for (int j = 0; j < 4; j++) {
            if (buckets[j] + nums[i] <= s) {
                if (j == 0 || buckets[j] != buckets[j - 1]) {
                    buckets[j] += nums[i];
                    if (helper(s, i + 1, buckets, nums)) return true;
                    buckets[j] -= nums[i];
                }
            }
        }
        return false;
    }
}
```

__Python__:

```Python
class Solution:
    def makesquare(self, nums: List[int]) -> bool:
        s = sum(nums)
        if (s & 0b11) or len(nums) < 4:
            return False
        s >>= 2
        nums.sort(reverse=True)
        if s < nums[0]:
            return False
        def dfs(i: int, buckets: List[int]) -> None:
            if i == len(nums):
                return True
            for j in range(4):
                if buckets[j] + nums[i] <= s:
                    if j == 0 or buckets[j] != buckets[j - 1]:
                        buckets[j] += nums[i]
                        if dfs(i + 1, buckets):
                            return True
                        buckets[j] -= nums[i]
            return False
        return dfs(0, [0, 0, 0, 0])
```
