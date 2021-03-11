# 495 Teemo Attacking 提莫攻击

__Description__:
In LOL world, there is a hero called Teemo and his attacking can make his enemy Ashe be in poisoned condition. Now, given the Teemo's attacking ascending time series towards Ashe and the poisoning time duration per Teemo's attacking, you need to output the total time that Ashe is in poisoned condition.

You may assume that Teemo attacks at the very beginning of a specific time point, and makes Ashe be in poisoned condition immediately.

__Example:__

Example 1:

Input: [1,4], 2
Output: 4
Explanation: At time point 1, Teemo starts attacking Ashe and makes Ashe be poisoned immediately.
This poisoned status will last 2 seconds until the end of time point 2.
And at time point 4, Teemo attacks Ashe again, and causes Ashe to be in poisoned status for another 2 seconds.
So you finally need to output 4.

Example 2:

Input: [1,2], 2
Output: 3
Explanation: At time point 1, Teemo starts attacking Ashe and makes Ashe be poisoned.
This poisoned status will last 2 seconds until the end of time point 2.
However, at the beginning of time point 2, Teemo attacks Ashe again who is already in poisoned status.
Since the poisoned status won't add up together, though the second poisoning attack will still work at time point 2, it will stop at the end of time point 3.
So you finally need to output 3.

__Note:__

You may assume the length of given time series array won't exceed 10000.
You may assume the numbers in the Teemo's attacking time series and his poisoning time duration per attacking are non-negative integers, which won't exceed 10,000,000.

__题目描述__:
在《英雄联盟》的世界中，有一个叫 “提莫” 的英雄，他的攻击可以让敌方英雄艾希（编者注：寒冰射手）进入中毒状态。现在，给出提莫对艾希的攻击时间序列和提莫攻击的中毒持续时间，你需要输出艾希的中毒状态总时长。

你可以认为提莫在给定的时间点进行攻击，并立即使艾希处于中毒状态。

__示例 :__

示例1:

输入: [1,4], 2
输出: 4
原因: 第 1 秒初，提莫开始对艾希进行攻击并使其立即中毒。中毒状态会维持 2 秒钟，直到第 2 秒末结束。
第 4 秒初，提莫再次攻击艾希，使得艾希获得另外 2 秒中毒时间。
所以最终输出 4 秒。

示例2:

输入: [1,2], 2
输出: 3
原因: 第 1 秒初，提莫开始对艾希进行攻击并使其立即中毒。中毒状态会维持 2 秒钟，直到第 2 秒末结束。
但是第 2 秒初，提莫再次攻击了已经处于中毒状态的艾希。
由于中毒状态不可叠加，提莫在第 2 秒初的这次攻击会在第 3 秒末结束。
所以最终输出 3 。

__提示:__

你可以假定时间序列数组的总长度不超过 10000。
你可以假定提莫攻击时间序列中的数字和提莫攻击的中毒持续时间都是非负整数，并且不超过 10,000,000。

__思路__:

比较后一个与前一个的差是否在 duration之间
如果不在就加上 duration, 否则加上两个时间差
时间复杂度O(n), 空间复杂度O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int findPoisonedDuration(vector<int>& timeSeries, int duration) 
    {
        int result = 0, n = timeSeries.size();
        for (int i = 1; i < n; i++) 
        {
            if (timeSeries[i - 1] + duration > timeSeries[i]) result += timeSeries[i] - timeSeries[i - 1];
            else result += duration;
        }
        return !n ? result : result + duration;
    }
};
```

__Java__:

```Java
class Solution {
    public int findPoisonedDuration(int[] timeSeries, int duration) {
        int result = 0, n = timeSeries.length;
        for (int i = 1; i < n; i++) {
            if (timeSeries[i - 1] + duration > timeSeries[i]) result += timeSeries[i] - timeSeries[i - 1];
            else result += duration;
        }
        return n == 0 ? result : result + duration;
    }
}
```

__Python__:

```Python
class Solution:
    def findPoisonedDuration(self, timeSeries: List[int], duration: int) -> int:
        result, n = 0, len(timeSeries)
        for i in range(1, n):
            if timeSeries[i - 1] + duration > timeSeries[i]:
                result += timeSeries[i] - timeSeries[i - 1]
            else:
                result += duration
        return result + duration if n else result
```
