# 339 Nested List Weight Sum 嵌套列表权重和

__Description:__

You are given a nested list of integers `nestedList`. Each element is either an integer or a list whose elements may also be integers or other lists.

The __depth__ of an integer is the number of lists that it is inside of. For example, the nested list `[1,[2,2],[[3],2],1]` has each integer's value set to its __depth__.

Return _the sum of each integer in_ `nestedList` _multiplied by its __depth___.

__Example:__

Example 1:

![339-1](https://assets.leetcode.com/uploads/2021/01/14/nestedlistweightsumex1.png)

```text
Input: nestedList = [[1,1],2,[1,1]]
Output: 10
Explanation: Four 1's at depth 2, one 2 at depth 1. 1*2 + 1*2 + 2*1 + 1*2 + 1*2 = 10.
```

Example 2:

![339-2](https://assets.leetcode.com/uploads/2021/01/14/nestedlistweightsumex2.png)

```text
Input: nestedList = [1,[4,[6]]]
Output: 27
Explanation: One 1 at depth 1, one 4 at depth 2, and one 6 at depth 3. 1*1 + 4*2 + 6*3 = 27.
```

Example 3:

```text
Input: nestedList = [0]
Output: 0
```

__Constraints:__

- `1 <= nestedList.length <= 50`
- The values of the integers in the nested list is in the range `[-100, 100]`.
- The maximum __depth__ of any integer is less than or equal to `50`.

__题目描述:__

给定一个嵌套的整数列表 `nestedList` ，每个元素要么是整数，要么是列表。同时，列表中元素同样也可以是整数或者是另一个列表。

整数的 __深度__ 是其在列表内部的嵌套层数。例如，嵌套列表 `[1,[2,2],[[3],2],1]` 中每个整数的值就是其深度。

请返回该列表按深度加权后所有整数的总和。

__示例:__

示例 1：

![339-3](https://assets.leetcode.com/uploads/2021/01/14/nestedlistweightsumex1.png)

```text
输入：nestedList = [[1,1],2,[1,1]]
输出：10 
解释：因为列表中有四个深度为 2 的 1 ，和一个深度为 1 的 2。
```

示例 2：

![339-4](https://assets.leetcode.com/uploads/2021/01/14/nestedlistweightsumex2.png)

```text
输入：nestedList = [1,[4,[6]]]
输出：27 
解释：一个深度为 1 的 1，一个深度为 2 的 4，一个深度为 3 的 6。所以，1 + 4*2 + 6*3 = 27。
```

示例 3：

```text
输入：nestedList = [0]
输出：0
```

__提示：__

- `1 <= nestedList.length <= 50`
- 嵌套列表中整数的值在范围 `[-100, 100]` 内
- 任何整数的最大 __深度__ 都小于或等于 `50`

__思路:__

```text
DFS
遇到数字直接相加
遇到列表再加一层 DFS
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
/**
 * // This is the interface that allows for creating nested lists.
 * // You should not implement it, or speculate about its implementation
 * class NestedInteger {
 *   public:
 *     // Constructor initializes an empty nested list.
 *     NestedInteger();
 *
 *     // Constructor initializes a single integer.
 *     NestedInteger(int value);
 *
 *     // Return true if this NestedInteger holds a single integer, rather than a nested list.
 *     bool isInteger() const;
 *
 *     // Return the single integer that this NestedInteger holds, if it holds a single integer
 *     // The result is undefined if this NestedInteger holds a nested list
 *     int getInteger() const;
 *
 *     // Set this NestedInteger to hold a single integer.
 *     void setInteger(int value);
 *
 *     // Set this NestedInteger to hold a nested list and adds a nested integer to it.
 *     void add(const NestedInteger &ni);
 *
 *     // Return the nested list that this NestedInteger holds, if it holds a nested list
 *     // The result is undefined if this NestedInteger holds a single integer
 *     const vector<NestedInteger> &getList() const;
 * };
 */
class Solution 
{
private:
    int helper(const NestedInteger& cur, int level) 
    {
        if (cur.isInteger()) return cur.getInteger() * level;
        int total = 0;
        for (const auto& v : cur.getList()) total += helper(v, level + 1);
        return total;
    }
public:
    int depthSum(vector<NestedInteger>& nestedList) 
    {
        int result = 0;
        for (const auto& v : nestedList) result += helper(v, 1);
        return result;
    }
};
```

__Java__:

```Java
/**
 * // This is the interface that allows for creating nested lists.
 * // You should not implement it, or speculate about its implementation
 * public interface NestedInteger {
 *     // Constructor initializes an empty nested list.
 *     public NestedInteger();
 *
 *     // Constructor initializes a single integer.
 *     public NestedInteger(int value);
 *
 *     // @return true if this NestedInteger holds a single integer, rather than a nested list.
 *     public boolean isInteger();
 *
 *     // @return the single integer that this NestedInteger holds, if it holds a single integer
 *     // Return null if this NestedInteger holds a nested list
 *     public Integer getInteger();
 *
 *     // Set this NestedInteger to hold a single integer.
 *     public void setInteger(int value);
 *
 *     // Set this NestedInteger to hold a nested list and adds a nested integer to it.
 *     public void add(NestedInteger ni);
 *
 *     // @return the nested list that this NestedInteger holds, if it holds a nested list
 *     // Return empty list if this NestedInteger holds a single integer
 *     public List<NestedInteger> getList();
 * }
 */
class Solution {
    public int depthSum(List<NestedInteger> nestedList) {
        int result = 0;
        for (var v : nestedList) result += helper(v, 1);
        return result;
    }
    
    private int helper(NestedInteger cur, int level) {
        if (cur.isInteger()) return cur.getInteger() * level;
        int total = 0;
        for (var v : cur.getList()) total += helper(v, level + 1);
        return total;
    }
}
```

__Python__:

```Python
# """
# This is the interface that allows for creating nested lists.
# You should not implement it, or speculate about its implementation
# """
#class NestedInteger:
#    def __init__(self, value=None):
#        """
#        If value is not specified, initializes an empty list.
#        Otherwise initializes a single integer equal to value.
#        """
#
#    def isInteger(self):
#        """
#        @return True if this NestedInteger holds a single integer, rather than a nested list.
#        :rtype bool
#        """
#
#    def add(self, elem):
#        """
#        Set this NestedInteger to hold a nested list and adds a nested integer elem to it.
#        :rtype void
#        """
#
#    def setInteger(self, value):
#        """
#        Set this NestedInteger to hold a single integer equal to value.
#        :rtype void
#        """
#
#    def getInteger(self):
#        """
#        @return the single integer that this NestedInteger holds, if it holds a single integer
#        Return None if this NestedInteger holds a nested list
#        :rtype int
#        """
#
#    def getList(self):
#        """
#        @return the nested list that this NestedInteger holds, if it holds a nested list
#        Return None if this NestedInteger holds a single integer
#        :rtype List[NestedInteger]
#        """
class Solution:
    def depthSum(self, nestedList: List[NestedInteger]) -> int:
        def helper(cur: NestedInteger, level: int) -> int:
            return cur.getInteger() * level if cur.isInteger() else sum(helper(v, level + 1) for v in cur.getList())
        return sum(helper(v, 1) for v in nestedList)
```
