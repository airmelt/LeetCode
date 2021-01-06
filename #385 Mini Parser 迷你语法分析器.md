# 385 Mini Parser 迷你语法分析器

__Description__:
Given a nested list of integers represented as a string, implement a parser to deserialize it.

Each element is either an integer, or a list -- whose elements may also be integers or other lists.

__Note:__
You may assume that the string is well-formed:

String is non-empty.
String does not contain white spaces.
String contains only digits 0-9, [, - ,, ].

__Example:__

Example 1:

Given s = "324",

You should return a NestedInteger object which contains a single integer 324.

Example 2:

Given s = "[123,[456,[789]]]",

Return a NestedInteger object containing a nested list with 2 elements:

1. An integer containing value 123.
2. A nested list containing two elements:
    i.  An integer containing value 456.
    ii. A nested list with one element:
         a. An integer containing value 789.

__题目描述__:
给定一个用字符串表示的整数的嵌套列表，实现一个解析它的语法分析器。

列表中的每个元素只可能是整数或整数嵌套列表

__提示：__
你可以假定这些字符串都是格式良好的：

字符串非空
字符串不包含空格
字符串只包含数字0-9、[、-、,、]

__示例 :__

示例 1：

给定 s = "324",

你应该返回一个 NestedInteger 对象，其中只包含整数值 324。

示例 2：

给定 s = "[123,[456,[789]]]",

返回一个 NestedInteger 对象包含一个有两个元素的嵌套列表：

1. 一个 integer 包含值 123
2. 一个包含两个元素的嵌套列表：
    i.  一个 integer 包含值 456
    ii. 一个包含一个元素的嵌套列表
         a. 一个 integer 包含值 789

__思路__:

递归
终点要么是 s为空, 要么 s为数字, 要么 s长度小于 2(为 '['或者 '[]')
可以将 s看作是一棵树, 每一层 []相当于树的一层
时间复杂度O(n), 空间复杂度O(n)

__代码__:
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
public:
    NestedInteger deserialize(string s) 
    {
        if (s.empty()) return NestedInteger();
        if (s[0] != '[') return NestedInteger(stoi(s));
        if (s.size() <= 2) return NestedInteger();
        NestedInteger result;
        int start = 1, level = 0, n = s.size();
        for (int i = 1; i < n; i++) 
        {
            if (level == 0 and (s[i] == ',' or i == n - 1))
            {
                result.add(deserialize(s.substr(start, i - start)));
                start = i + 1;
            }
            else if (s[i] == '[') ++level;
            else if (s[i] == ']') --level;
        }
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
 *     // Return null if this NestedInteger holds a single integer
 *     public List<NestedInteger> getList();
 * }
 */
class Solution {
    public NestedInteger deserialize(String s) {
        if (s == null || s.isEmpty()) return new NestedInteger();
        if (s.charAt(0) != '[') return new NestedInteger(Integer.valueOf(s));
        if (s.length() <= 2) return new NestedInteger();
        NestedInteger result = new NestedInteger();
        int start = 1, level = 0, n = s.length();
        for (int i = 1; i < n; i++) {
            if (level == 0 && (s.charAt(i) == ',' || i == n - 1)) {
                result.add(deserialize(s.substring(start, i)));
                start = i + 1;
            }
            else if (s.charAt(i) == '[') ++level;
            else if (s.charAt(i) == ']') --level;
        }
        return result;
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
    def deserialize(self, s: str) -> NestedInteger:
        if not s:
            return NestedInteger()
        if s[0] != '[':
            return NestedInteger(int(s))
        if len(s) < 3:
            return NestedInteger()
        result, start, level, n = NestedInteger(), 1, 0, len(s)
        for i in range(1, n):
            if not level and (s[i] == ',' or i == n - 1):
                result.add(self.deserialize(s[start:i]))
                start = i + 1
            elif s[i] == '[':
                level += 1
            elif s[i] == ']':
                level -= 1
        return result
```
