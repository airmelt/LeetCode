# 781 Rabbits in Forest 森林中的兔子

__Description__:
There is a forest with an unknown number of rabbits. We asked n rabbits "How many rabbits have the same color as you?" and collected the answers in an integer array answers where answers[i] is the answer of the ith rabbit.

Given the array answers, return the minimum number of rabbits that could be in the forest.

__Example:__

Example 1:

Input: answers = [1,1,2]
Output: 5
Explanation:
The two rabbits that answered "1" could both be the same color, say red.
The rabbit that answered "2" can't be red or the answers would be inconsistent.
Say the rabbit that answered "2" was blue.
Then there should be 2 other blue rabbits in the forest that didn't answer into the array.
The smallest possible number of rabbits in the forest is therefore 5: 3 that answered plus 2 that didn't.

Example 2:

Input: answers = [10,10,10]
Output: 11

__Constraints:__

1 <= answers.length <= 1000
0 <= answers[i] < 1000

__题目描述__:
森林中，每个兔子都有颜色。其中一些兔子（可能是全部）告诉你还有多少其他的兔子和自己有相同的颜色。我们将这些回答放在 answers 数组里。

返回森林中兔子的最少数量。

__示例 :__

示例 1:

输入: answers = [1, 1, 2]
输出: 5
解释:
两只回答了 "1" 的兔子可能有相同的颜色，设为红色。
之后回答了 "2" 的兔子不会是红色，否则他们的回答会相互矛盾。
设回答了 "2" 的兔子为蓝色。
此外，森林中还应有另外 2 只蓝色兔子的回答没有包含在数组中。
因此森林中兔子的最少数量是 5: 3 只回答的和 2 只没有回答的。

示例 2:

输入: answers = [10, 10, 10]
输出: 11

示例 3:

输入: answers = []
输出: 0

__说明:__

answers 的长度最大为1000。
answers[i] 是在 [0, 999] 范围内的整数。

__思路__:

数学
假设有 n 只兔子报告次数 <= n + 1, 那么最多只需要 n + 1 只兔子
如果有 n 只兔子报告次数 > n + 1, 那么可以将其中 n + 1 只兔子看作一种颜色, 剩下的看作另一种颜色
所以用一个哈希表记录兔子出现的次数, m[a] = a, 并加上兔子的数目 result += (m[a] + 1), 每次碰到一样的兔子就 --m[a] 直到 m[a] = 0
时间复杂度为 O(n), 空间复杂度为 O(n)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int numRabbits(vector<int>& answers) 
    {
        map<int, int> m;
        int result = 0;
        for (const auto &a : answers)
        {
            if (m.count(a) and m[a] > 0) --m[a];
            else result += (m[a] = a) + 1;
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int numRabbits(int[] answers) {
        Map<Integer,Integer> map = new HashMap<>();
        int result = 0;
        for (int answer : answers) {
            if (map.containsKey(answer) && map.get(answer) > 0) map.put(answer, map.get(answer) - 1);
            else {
                result += answer + 1;
                map.put(answer, answer);
            }
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def numRabbits(self, answers: List[int]) -> int:
        return sum((c + r) // (r + 1) * (r + 1) for r, c in Counter(answers).items())
```
