# 646 Maximum Length of Pair Chain 最长数对链

__Description__:
You are given an array of n pairs pairs where pairs[i] = [lefti, righti] and lefti < righti.

A pair p2 = [c, d] follows a pair p1 = [a, b] if b < c. A chain of pairs can be formed in this fashion.

Return the length longest chain which can be formed.

You do not need to use up all the given intervals. You can select pairs in any order.

__Example:__

Example 1:

Input: pairs = [[1,2],[2,3],[3,4]]
Output: 2
Explanation: The longest chain is [1,2] -> [3,4].

Example 2:

Input: pairs = [[1,2],[7,8],[4,5]]
Output: 3
Explanation: The longest chain is [1,2] -> [4,5] -> [7,8].

__Constraints:__

n == pairs.length
1 <= n <= 1000
-1000 <= lefti < righti < 1000

__题目描述__:
给出 n 个数对。 在每一个数对中，第一个数字总是比第二个数字小。

现在，我们定义一种跟随关系，当且仅当 b < c 时，数对(c, d) 才可以跟在 (a, b) 后面。我们用这种形式来构造一个数对链。

给定一个数对集合，找出能够形成的最长数对链的长度。你不需要用到所有的数对，你可以以任何顺序选择其中的一些数对来构造。

__示例 :__

输入：[[1,2], [2,3], [3,4]]
输出：2
解释：最长的数对链是 [1,2] -> [3,4]

__提示:__

给出数对的个数在 [1, 1000] 范围内。

__思路__:

排序 ➕ 贪心
排序之后取第一个 pair 的结尾, 依次比较元素并加入结果
时间复杂度 O(nlgn), 空间复杂度 O(n)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int findLongestChain(vector<vector<int>>& pairs) 
    {
        sort(pairs.begin(), pairs.end(), [](const auto &a, const auto &b) { return a.back() < b.back(); });
        int result = 1, cur = pairs.front().back(), n = pairs.size();
        for (int i = 1; i < n; i++) 
        {
            if (pairs[i].front() > cur) 
            {
                cur = pairs[i].back();
                ++result;
            }
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int findLongestChain(int[][] pairs) {
        Arrays.sort(pairs, (a, b) -> a[1] - b[1]);
        int result = 1, cur = pairs[0][1], n = pairs.length;
        for (int i = 1; i < n; i++) {
            if (pairs[i][0] > cur) {
                cur = pairs[i][1];
                ++result;
            }
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def findLongestChain(self, pairs: List[List[int]]) -> int:
        pairs.sort(key=lambda x: x[1])
        result, cur, n = 1, pairs[0][1], len(pairs)
        for i in range(1, n):
            if pairs[i][0] > cur:
                result += 1
                cur = pairs[i][1]
        return result
```
