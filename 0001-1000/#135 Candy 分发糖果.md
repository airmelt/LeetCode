# 135 Candy 分发糖果

__Description__:
There are N children standing in a line. Each child is assigned a rating value.

You are giving candies to these children subjected to the following requirements:

Each child must have at least one candy.
Children with a higher rating get more candies than their neighbors.
What is the minimum candies you must give?

__Example:__

Example 1:

Input: [1,0,2]
Output: 5
Explanation: You can allocate to the first, second and third child with 2, 1, 2 candies respectively.

Example 2:

Input: [1,2,2]
Output: 4
Explanation: You can allocate to the first, second and third child with 1, 2, 1 candies respectively.
             The third child gets 1 candy because it satisfies the above two conditions.

__题目描述__:
老师想给孩子们分发糖果，有 N 个孩子站成了一条直线，老师会根据每个孩子的表现，预先给他们评分。

你需要按照以下要求，帮助老师给这些孩子分发糖果：

每个孩子至少分配到 1 个糖果。
相邻的孩子中，评分高的孩子必须获得更多的糖果。
那么这样下来，老师至少需要准备多少颗糖果呢？

__示例 :__

示例 1:

输入: [1,0,2]
输出: 5
解释: 你可以分别给这三个孩子分发 2、1、2 颗糖果。

示例 2:

输入: [1,2,2]
输出: 4
解释: 你可以分别给这三个孩子分发 1、2、1 颗糖果。
     第三个孩子只得到 1 颗糖果，这已满足上述两个条件。

__思路__:

1. 给每个人初始化 1个糖果
先正向遍历数组, 记录比前面大的评分的孩子的糖果为前一人加 1
再反向遍历数组, 记录比后面大的评分的孩子的糖果为后一人加 1
时间复杂度O(n), 空间复杂度O(n)
2. 由于分配糖果的形式一定是从 1 -> n或者从 m -> 1, 所以我们可以记录上坡的长度和下坡的次数及峰顶和谷底的糖果数, 用等差数列求和求出各个坡的糖果数
时间复杂度O(n), 空间复杂度O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int candy(vector<int>& ratings) 
    {
        vector<int> nums(ratings.size(), 1);
        for (int i = 1; i < ratings.size(); i++) if (ratings[i] > ratings[i - 1]) nums[i] = nums[i - 1] + 1;
        for (int i = ratings.size() - 2; i > -1; i--) if (ratings[i] > ratings[i + 1] and nums[i] <= nums[i + 1]) nums[i] = nums[i + 1] + 1;
        return accumulate(nums.begin(), nums.end(), 0);
    }
};
```

__Java__:

```Java
class Solution {
    public int candy(int[] ratings) {
        if (ratings.length <= 1) return ratings.length;
        int result = 0, up = 0, down = 0, oldSlope = 0;
        for (int i = 1; i < ratings.length; i++) {
            int newSlope = (ratings[i] > ratings[i - 1]) ? 1 : (ratings[i] < ratings[i - 1] ? -1 : 0);
            if ((oldSlope > 0 && newSlope == 0) || (oldSlope < 0 && newSlope >= 0)) {
                result += count(up) + count(down) + Math.max(up, down);
                up = 0;
                down = 0;
            }
            if (newSlope > 0) up++;
            if (newSlope < 0) down++;
            if (newSlope == 0) result++;
            oldSlope = newSlope;
        }
        result += count(up) + count(down) + Math.max(up, down) + 1;
        return result;
    }

    private int count(int a) {
        return ((a * (a + 1)) >> 1);
    }
}
```

__Python__:

```Python
class Solution:
    def candy(self, ratings: List[int]) -> int:
        candies = [1] * len(ratings)
        for i in range(1, len(ratings)):
            if ratings[i] > ratings[i - 1]:
                candies[i] = candies[i - 1] + 1
        for i in range(len(candies) - 2, -1, -1):
            if ratings[i] > ratings[i + 1] and candies[i] <= candies[i + 1]:
                candies[i] = candies[i + 1] + 1
        return sum(candies)
```
