# 1395 Count Number of Teams 统计作战单位数

__Description:__

There are n soldiers standing in a line. Each soldier is assigned a unique rating value.

You have to form a team of 3 soldiers amongst them under the following rules:

Choose 3 soldiers with index (i, j, k) with rating (rating[i], rating[j], rating[k]).
A team is valid if: (rating[i] < rating[j] < rating[k]) or (rating[i] > rating[j] > rating[k]) where (0 <= i < j < k < n).

Return the number of teams you can form given the conditions. (soldiers can be part of multiple teams).

__Example:__

Example 1:

Input: rating = [2,5,3,4,1]
Output: 3
Explanation: We can form three teams given the conditions. (2,3,4), (5,4,1), (5,3,1).

Example 2:

Input: rating = [2,1,3]
Output: 0
Explanation: We can't form any team given the conditions.

Example 3:

Input: rating = [1,2,3,4]
Output: 4

__Constraints:__

n == rating.length
3 <= n <= 1000
1 <= rating[i] <= 10^5
All the integers in rating are unique.

__题目描述:__

n 名士兵站成一排。每个士兵都有一个 独一无二 的评分 rating 。

每 3 个士兵可以组成一个作战单位，分组规则如下：

从队伍中选出下标分别为 i、j、k 的 3 名士兵，他们的评分分别为 rating[i]、rating[j]、rating[k]
作战单位需满足： rating[i] < rating[j] < rating[k] 或者 rating[i] > rating[j] > rating[k] ，其中  0 <= i < j < k < n

请你返回按上述条件可以组建的作战单位数量。每个士兵都可以是多个作战单位的一部分。

__示例:__

示例 1：

输入：rating = [2,5,3,4,1]
输出：3
解释：我们可以组建三个作战单位 (2,3,4)、(5,4,1)、(5,3,1) 。

示例 2：

输入：rating = [2,1,3]
输出：0
解释：根据题目条件，我们无法组建作战单位。

示例 3：

输入：rating = [1,2,3,4]
输出：4

__提示：__

n == rating.length
3 <= n <= 1000
1 <= rating[i] <= 10^5
rating 中的元素都是唯一的

__思路:__

```text
计数
注意到所有士兵的评分都不相等
以 arr[i] 为中心
只考虑递增的情况
那么在 arr[i] 之前出现的所有比 arr[i] 小的都可以作为一个备选
在 arr[i] 之后出现的所有比 arr[i] 大的可以作为另一个备选
根据乘法原理, 以 arr[i] 为中心的就是上面两种情况的乘积
递减情况类似
时间复杂度为 O(N ^ 2), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int numTeams(vector<int>& rating) 
    {
        int result = 0, n = rating.size();
        for (int i = 0; i < n; i++)
        {
            int count_left_small = 0, count_left_big = 0, count_right_small = 0, count_right_big = 0;
            for (int j = 0; j < i; j++)
            {
                count_left_small += rating[j] < rating[i];
                count_left_big += rating[j] > rating[i];
            }
            for (int j = i + 1; j < n; j++)
            {
                count_right_small += rating[j] > rating[i];
                count_right_big += rating[j] < rating[i];
            }
            result += count_left_small * count_right_small + count_left_big * count_right_big;
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int numTeams(int[] rating) {
        int result = 0, n = rating.length;
        for (int i = 0; i < n; i++) {
            int countLeftSmall = 0, countLeftBig = 0, countRightSmall = 0, countRightBig = 0;
            for (int j = 0; j < i; j++) {
                countLeftSmall += rating[j] < rating[i] ? 1 : 0;
                countLeftBig += rating[j] > rating[i] ? 1 : 0;
            }
            for (int j = i + 1; j < n; j++) {
                countRightSmall += rating[j] > rating[i] ? 1 : 0;
                countRightBig += rating[j] < rating[i] ? 1 : 0;
            }
            result += countLeftSmall * countRightSmall + countLeftBig * countRightBig;
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def numTeams(self, rating: List[int]) -> int:
        return sum(sum(rating[j] < rating[i] for j in range(i)) * sum(rating[j] > rating[i] for j in range(i + 1, len(rating))) + sum(rating[j] > rating[i] for j in range(i)) * sum(rating[j] < rating[i] for j in range(i + 1, len(rating))) for i in range(len(rating)))
```
