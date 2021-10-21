# 881 Boats to Save People 救生艇

__Description__:
You are given an array people where people[i] is the weight of the ith person, and an infinite number of boats where each boat can carry a maximum weight of limit. Each boat carries at most two people at the same time, provided the sum of the weight of those people is at most limit.

Return the minimum number of boats to carry every given person.

__Example:__

Example 1:

Input: people = [1,2], limit = 3
Output: 1
Explanation: 1 boat (1, 2)

Example 2:

Input: people = [3,2,2,1], limit = 3
Output: 3
Explanation: 3 boats (1, 2), (2) and (3)

Example 3:

Input: people = [3,5,3,4], limit = 5
Output: 4
Explanation: 4 boats (3), (3), (4), (5)

__Constraints:__

1 <= people.length <= 5 \* 10^4
1 <= people[i] <= limit <= 3 \* 10^4

__题目描述__:
第 i 个人的体重为 people[i]，每艘船可以承载的最大重量为 limit。

每艘船最多可同时载两人，但条件是这些人的重量之和最多为 limit。

返回载到每一个人所需的最小船数。(保证每个人都能被船载)。

__示例 :__

示例 1：

输入：people = [1,2], limit = 3
输出：1
解释：1 艘船载 (1, 2)

示例 2：

输入：people = [3,2,2,1], limit = 3
输出：3
解释：3 艘船分别载 (1, 2), (2) 和 (3)

示例 3：

输入：people = [3,5,3,4], limit = 5
输出：4
解释：4 艘船分别载 (3), (3), (4), (5)

__提示:__

1 <= people.length <= 50000
1 <= people[i] <= limit <= 30000

__思路__:

贪心
先排序
每次将最重的和最轻的放入同一艘船
如果满足 limit 条件, 指向轻的指针向前移动
时间复杂度为 O(nlgn), 空间复杂度为 O(lgn)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int numRescueBoats(vector<int>& people, int limit) 
    {
        int result = 0, n = people.size(), right = n - 1, left = 0;
        sort(people.begin(), people.end());
        while (left <= right) 
        {
            if (people[left] + people[right] <= limit) ++left;
            --right;
            ++result;
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int numRescueBoats(int[] people, int limit) {
        int result = 0, n = people.length, right = n - 1, left = 0;
        Arrays.sort(people);
        while (left <= right) {
            if (people[left] + people[right] <= limit) ++left;
            --right;
            ++result;
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def numRescueBoats(self, people: List[int], limit: int) -> int:
        result, right, left = 0, (n := len(people)) - 1, 0
        people.sort()
        while left <= right:
            if people[left] + people[right] <= limit:
                left += 1
            right -= 1
            result += 1
        return result
```
