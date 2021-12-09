# 946 Validate Stack Sequences 验证栈序列

__Description__:
Given two integer arrays pushed and popped each with distinct values, return true if this could have been the result of a sequence of push and pop operations on an initially empty stack, or false otherwise.

__Example:__

Example 1:

Input: pushed = [1,2,3,4,5], popped = [4,5,3,2,1]
Output: true
Explanation: We might do the following sequence:
push(1), push(2), push(3), push(4),
pop() -> 4,
push(5),
pop() -> 5, pop() -> 3, pop() -> 2, pop() -> 1

Example 2:

Input: pushed = [1,2,3,4,5], popped = [4,3,5,1,2]
Output: false
Explanation: 1 cannot be popped before 2.

__Constraints:__

1 <= pushed.length <= 1000
0 <= pushed[i] <= 1000
All the elements of pushed are unique.
popped.length == pushed.length
popped is a permutation of pushed.

__题目描述__:
给定 pushed 和 popped 两个序列，每个序列中的 值都不重复，只有当它们可能是在最初空栈上进行的推入 push 和弹出 pop 操作序列的结果时，返回 true；否则，返回 false 。

__示例 :__

示例 1：

输入：pushed = [1,2,3,4,5], popped = [4,5,3,2,1]
输出：true
解释：我们可以按以下顺序执行：
push(1), push(2), push(3), push(4), pop() -> 4,
push(5), pop() -> 5, pop() -> 3, pop() -> 2, pop() -> 1

示例 2：

输入：pushed = [1,2,3,4,5], popped = [4,3,5,1,2]
输出：false
解释：1 不能在 2 之前弹出。

__提示:__

1 <= pushed.length <= 1000
0 <= pushed[i] <= 1000
pushed 的所有元素 互不相同
popped.length == pushed.length
popped 是 pushed 的一个排列

__思路__:

模拟
按照栈出入进行模拟即可
时间复杂度为 O(n), 空间复杂度为 O(n)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    bool validateStackSequences(vector<int>& pushed, vector<int>& popped) 
    {
        int n = pushed.size(), top = -1, i = 0, j = 0;
        vector<int> stack(n);
        while (i < n) for (stack[++top] = pushed[i++]; top > -1 and stack[top] == popped[j]; --top, ++j) ;
        return top == -1;
    }
};
```

__Java__:

```Java
class Solution {
    public boolean validateStackSequences(int[] pushed, int[] popped) {
        int n = pushed.length, top = -1, i = 0, j = 0, stack[] = new int[n];
        while (i < n) for (stack[++top] = pushed[i++]; top > -1 && stack[top] == popped[j]; --top, ++j) ;
        return top == -1;
    }
}
```

__Python__:

```Python
class Solution:
    def validateStackSequences(self, pushed: List[int], popped: List[int]) -> bool:
        stack, i, n = [], 0, len(pushed)
        for num in pushed:
            stack.append(num)
            while stack and i < n and stack[-1] == popped[i]:
                stack.pop()
                i += 1
        return i == n
```
