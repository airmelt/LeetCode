# 768 Max Chunks To Make Sorted II 最多能完成排序的块 II

__Description__:
You are given an integer array arr.

We split arr into some number of chunks (i.e., partitions), and individually sort each chunk. After concatenating them, the result should equal the sorted array.

Return the largest number of chunks we can make to sort the array.

__Example:__

Example 1:

Input: arr = [5,4,3,2,1]
Output: 1
Explanation:
Splitting into two or more chunks will not return the required result.
For example, splitting into [5, 4], [3, 2, 1] will result in [4, 5, 1, 2, 3], which isn't sorted.

Example 2:

Input: arr = [2,1,3,4,4]
Output: 4
Explanation:
We can split into two chunks, such as [2, 1], [3, 4, 4].
However, splitting into [2, 1], [3], [4], [4] is the highest number of chunks possible.

__Constraints:__

1 <= arr.length <= 2000
0 <= arr[i] <= 10^8

__题目描述__:
这个问题和“最多能完成排序的块”相似，但给定数组中的元素可以重复，输入数组最大长度为2000，其中的元素最大为10**8。

arr是一个可能包含重复元素的整数数组，我们将这个数组分割成几个“块”，并将这些块分别进行排序。之后再连接起来，使得连接的结果和按升序排序后的原数组相同。

我们最多能将数组分成多少块？

__示例 :__

示例 1:

输入: arr = [5,4,3,2,1]
输出: 1
解释:
将数组分成2块或者更多块，都无法得到所需的结果。
例如，分成 [5, 4], [3, 2, 1] 的结果是 [4, 5, 1, 2, 3]，这不是有序的数组。

示例 2:

输入: arr = [2,1,3,4,4]
输出: 4
解释:
我们可以把它分成两块，例如 [2, 1], [3, 4, 4]。
然而，分成 [2, 1], [3], [4], [4] 可以得到最多的块数。

__注意:__

arr的长度在[1, 2000]之间。
arr[i]的大小在[0, 10**8]之间。

__思路__:

单调栈
栈中只存放每个块中的最大值
遍历数组, 比较当前值和栈中的值的大小
若栈非空且栈顶小于当前值, 弹出栈中元素, 然后合并之后再将当前块最大值放入栈
否则新建一块, 放入当前值入栈
时间复杂度为 O(n), 空间复杂度为 O(n)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int maxChunksToSorted(vector<int>& arr) 
    {
        stack<int> s;
        for (int i = 0, n = arr.size(); i < n; i++) 
        {
            if (!s.empty() and s.top() > arr[i]) 
            {
                int cur = s.top();
                while (!s.empty() and s.top() > arr[i]) s.pop();
                s.push(cur);
            }
            else s.push(arr[i]);
        }
        return s.size();
    }
};
```

__Java__:

```Java
class Solution {
    public int maxChunksToSorted(int[] arr) {
        Stack<Integer> stack = new Stack<>();
        for (int i = 0, n = arr.length; i < n; i++) {
            if (!stack.isEmpty() && stack.peek() > arr[i]) {
                int cur = stack.pop();
                while (!stack.isEmpty() && stack.peek() > arr[i]) stack.pop();
                stack.push(cur);
            }
            else stack.push(arr[i]);
        }
        return stack.size();
    }
}
```

__Python__:

```Python
class Solution:
    def maxChunksToSorted(self, arr: List[int]) -> int:
        stack, n = [], len(arr)
        for i in range(n):
            if stack and stack[-1] > arr[i]:
                cur = stack[-1]
                while stack and stack[-1] > arr[i]:
                    stack.pop()
                stack.append(cur)
            else:
                stack.append(arr[i])
        return len(stack)
```
