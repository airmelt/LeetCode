# 825 Friends Of Appropriate Ages 适龄的朋友

__Description__:
There are n persons on a social media website. You are given an integer array ages where ages[i] is the age of the ith person.

A Person x will not send a friend request to a person y (x != y) if any of the following conditions is true:

age[y] <= 0.5 \* age[x] + 7
age[y] > age[x]
age[y] > 100 && age[x] < 100
Otherwise, x will send a friend request to y.

Note that if x sends a request to y, y will not necessarily send a request to x. Also, a person will not send a friend request to themself.

Return the total number of friend requests made.

__Example:__

Example 1:

Input: ages = [16,16]
Output: 2
Explanation: 2 people friend request each other.

Example 2:

Input: ages = [16,17,18]
Output: 2
Explanation: Friend requests are made 17 -> 16, 18 -> 17.

Example 3:

Input: ages = [20,30,100,110,120]
Output: 3
Explanation: Friend requests are made 110 -> 100, 120 -> 110, 120 -> 100.

__Constraints:__

n == ages.length
1 <= n <= 2 * 10^4
1 <= ages[i] <= 120

__题目描述__:
人们会互相发送好友请求，现在给定一个包含有他们年龄的数组，ages[i] 表示第 i 个人的年龄。

当满足以下任一条件时，A 不能给 B（A、B不为同一人）发送好友请求：

age[B] <= 0.5 \* age[A] + 7
age[B] > age[A]
age[B] > 100 && age[A] < 100
否则，A 可以给 B 发送好友请求。

注意如果 A 向 B 发出了请求，不等于 B 也一定会向 A 发出请求。而且，人们不会给自己发送好友请求。

求总共会发出多少份好友请求?

__示例 :__

示例 1：

输入：[16,16]
输出：2
解释：二人可以互发好友申请。

示例 2：

输入：[16,17,18]
输出：2
解释：好友请求可产生于 17 -> 16, 18 -> 17.

示例 3：

输入：[20,30,100,110,120]
输出：3
解释：好友请求可产生于 110 -> 100, 120 -> 110, 120 -> 100.

__提示:__

1 <= ages.length <= 20000
1 <= ages[i] <= 120

__思路__:

模拟
用一个长度为 121 的数组 count 记录每个年龄出现的次数
遍历 count 数组, 按照题目要求去掉不符合的情况
如果 a 和 b 相等需要减去重复计算的部分
时间复杂度为 O(m ^ 2 + n), 空间复杂度为 O(m), 其中 n 为人数, m 表示不同的年龄的数量

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int numFriendRequests(vector<int>& ages) 
    {
        int count[121] = {}, result = 0;
        for (const auto& age : ages) ++count[age];
        for (int i = 15; i <= 120; i++) for (int j = 0.5 * i + 8; j <= i; j++) result += count[j] * (count[i] - (i == j));
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int numFriendRequests(int[] ages) {
        int[] count = new int[121];
        for (int age: ages) ++count[age];
        int result = 0;
        for (int ageA = 0; ageA <= 120; ageA++) {
            for (int ageB = 0; ageB <= 120; ageB++) {
                if (ageA * 0.5 + 7 >= ageB || ageA < ageB || ageA < 100 && 100 < ageB) continue;
                result += count[ageA] * count[ageB] - (ageA == ageB ? 1 : 0) * count[ageA];
            }
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def numFriendRequests(self, ages: List[int]) -> int:
        return sum(count_a * count_b - count_a * (a == b) for a, count_a in Counter(ages).items() for b, count_b in Counter(ages).items() if b > 0.5 * a + 7 and b <= a)
```
