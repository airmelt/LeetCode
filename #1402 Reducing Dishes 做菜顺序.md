# 1402 Reducing Dishes 做菜顺序

__Description:__

A chef has collected data on the satisfaction level of his n dishes. Chef can cook any dish in 1 unit of time.

Like-time coefficient of a dish is defined as the time taken to cook that dish including previous dishes multiplied by its satisfaction level i.e. time[i] * satisfaction[i].

Return the maximum sum of like-time coefficient that the chef can obtain after dishes preparation.

Dishes can be prepared in any order and the chef can discard some dishes to get this maximum value.

__Example:__

Example 1:

Input: satisfaction = [-1,-8,0,5,-9]
Output: 14
Explanation: After Removing the second and last dish, the maximum total like-time coefficient will be equal to (-1*1 + 0*2 + 5*3 = 14).
Each dish is prepared in one unit of time.

Example 2:

Input: satisfaction = [4,3,2]
Output: 20
Explanation: Dishes can be prepared in any order, (2*1 + 3*2 + 4*3 = 20)

Example 3:

Input: satisfaction = [-1,-4,-5]
Output: 0
Explanation: People do not like the dishes. No dish is prepared.

__Constraints:__

n == satisfaction.length
1 <= n <= 500
-1000 <= satisfaction[i] <= 1000

__题目描述:__

一个厨师收集了他 n 道菜的满意程度 satisfaction ，这个厨师做出每道菜的时间都是 1 单位时间。

一道菜的 「喜爱时间」系数定义为烹饪这道菜以及之前每道菜所花费的时间乘以这道菜的满意程度，也就是 time[i]*satisfaction[i] 。

请你返回做完所有菜 「喜爱时间」总和的最大值为多少。

你可以按 任意 顺序安排做菜的顺序，你也可以选择放弃做某些菜来获得更大的总和。

__示例:__

示例 1：

输入：satisfaction = [-1,-8,0,5,-9]
输出：14
解释：去掉第二道和最后一道菜，最大的喜爱时间系数和为 (-1*1 + 0*2 + 5*3 = 14) 。每道菜都需要花费 1 单位时间完成。

示例 2：

输入：satisfaction = [4,3,2]
输出：20
解释：按照原来顺序相反的时间做菜 (2*1 + 3*2 + 4*3 = 20)

示例 3：

输入：satisfaction = [-1,-4,-5]
输出：0
解释：大家都不喜欢这些菜，所以不做任何菜可以获得最大的喜爱时间系数。

__提示：__

n == satisfaction.length
1 <= n <= 500
-1000 <= satisfaction[i] <= 1000

__思路:__

```text
贪心
其实只要将数组反向排序得到 [a0, a1, a2, ...]
计算 s = a0 + 2 * a1 + 3 * a2 + ... 取最大值即可
为了方便计算可以先求取数组的前缀和 pre = [0, a0 + a1, a0 + a1 + a2, ...]
前缀和的前缀和的最大项就是我们需要求的
可以简化为 cur += s + satisfaction[i], result = max(result, cur), s += satisfaction[i]
时间复杂度为 O(NlogN), 空间复杂度为 O(logN), 主要开销在排序上
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int maxSatisfaction(vector<int>& satisfaction) 
    {
        int cur = 0, result = 0, s = 0;
        sort(satisfaction.begin(), satisfaction.end());
        for (auto iter = satisfaction.rbegin(); iter != satisfaction.rend(); iter++) 
        {
            cur += s + *iter;
            result = max(result, cur);
            s += *iter;
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int maxSatisfaction(int[] satisfaction) {
        int n = satisfaction.length, cur = 0, result = 0, s = 0;
        Arrays.sort(satisfaction);
        for (int i = n - 1; i > -1; i--) {
            cur += s + satisfaction[i];
            result = Math.max(result, cur);
            s += satisfaction[i];
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def maxSatisfaction(self, satisfaction: List[int]) -> int:
        return max(accumulate(accumulate([0] + sorted(satisfaction, reverse = True))))
```
