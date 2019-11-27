__Description__:
In a list of songs, the i-th song has a duration of time[i] seconds. 

Return the number of pairs of songs for which their total duration in seconds is divisible by 60.  Formally, we want the number of indices i < j with (time[i] + time[j]) % 60 == 0.

__Example:__
Example 1:

Input: [30,20,150,100,40]
Output: 3
Explanation: Three pairs have a total duration divisible by 60:
(time[0] = 30, time[2] = 150): total duration 180
(time[1] = 20, time[3] = 100): total duration 120
(time[1] = 20, time[4] = 40): total duration 60

Example 2:

Input: [60,60,60]
Output: 3
Explanation: All three pairs have a total duration of 120, which is divisible by 60.
 
__Note:__

1 <= time.length <= 60000
1 <= time[i] <= 500

__题目描述__:
在歌曲列表中，第 i 首歌曲的持续时间为 time[i] 秒。

返回其总持续时间（以秒为单位）可被 60 整除的歌曲对的数量。形式上，我们希望索引的数字  i < j 且有 (time[i] + time[j]) % 60 == 0。

__示例 :__
示例 1：

输入：[30,20,150,100,40]
输出：3
解释：这三对的总持续时间可被 60 整数：
(time[0] = 30, time[2] = 150): 总持续时间 180
(time[1] = 20, time[3] = 100): 总持续时间 120
(time[1] = 20, time[4] = 40): 总持续时间 60

示例 2：

输入：[60,60,60]
输出：3
解释：所有三对的总持续时间都是 120，可以被 60 整数。
 
__提示：__

1 <= time.length <= 60000
1 <= time[i] <= 500

__思路__:
1. 最直接的思路, 用两个循环遍历查找和为 60的倍数的情况
时间复杂度O(n ^ 2), 空间复杂度O(1)
2. 参考[LeetCode #1 Two Sum 两数之和](https://www.jianshu.com/p/09dbccebd6bf), 用一个数组记录已经出现过的 time数组中的值, 找到匹配值即可, 思想类似双指针
时间复杂度O(n), 空间复杂度O(1)

__代码__:
__C++__:
```C++
class Solution 
{
public:
    int numPairsDivisibleBy60(vector<int>& time) 
    {
        int result = 0, count[60]{0};
        for (auto t : time)
        {
            result += count[(60 - t % 60) % 60];
            count[t % 60]++;
        }
        return result;
    }
};
```

__Java__:
```Java
class Solution {
    public int numPairsDivisibleBy60(int[] time) {
        int result = 0, count[] = new int[60];
        for (int t : time) {
            result += count[(60 - t % 60) % 60];
            count[t % 60]++;
        }
        return result;
    }
}
```

__Python__:
```Python
class Solution:
    def numPairsDivisibleBy60(self, time: List[int]) -> int:
        count, result = [0] * 60, 0
        for t in time:
            result += count[(60 - t % 60) % 60]
            count[t % 60] += 1
        return result
```