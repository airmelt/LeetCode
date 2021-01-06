# 84 Largest Rectangle in Histogram 柱状图中最大的矩形

__Description__:
Given n non-negative integers representing the histogram's bar height where the width of each bar is 1, find the area of largest rectangle in the histogram.

![Above is a histogram where width of each bar is 1, given height = [2,1,5,6,2,3].](https://upload-images.jianshu.io/upload_images/16639143-2817cc2b0a94cca8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![The largest rectangle is shown in the shaded area, which has area = 10 unit.](https://upload-images.jianshu.io/upload_images/16639143-a70e79fada9bed24.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

__Example:__

Input: [2,1,5,6,2,3]
Output: 10

__题目描述__:
给定 n 个非负整数，用来表示柱状图中各个柱子的高度。每个柱子彼此相邻，且宽度为 1 。

求在该柱状图中，能够勾勒出来的矩形的最大面积。

 ![以上是柱状图的示例，其中每个柱子的宽度为 1，给定的高度为 [2,1,5,6,2,3]。](https://upload-images.jianshu.io/upload_images/16639143-d0d9c3510542f26b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![图中阴影部分为所能勾勒出的最大矩形面积，其面积为 10 个单位。](https://upload-images.jianshu.io/upload_images/16639143-0da82a6d87430360.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

__示例 :__

输入: [2,1,5,6,2,3]
输出: 10

__思路__:

1. 对每一个柱子, 寻找左边小于它的第一个柱子 left_index, 右边小于它的第一个柱子 right_index, result = max(height[i] \* (right_index - left_index - 1)
时间复杂度O(n ^ 2), 空间复杂度O(1)
2. 单调栈

- 单调栈是一种特殊的栈, 栈中的元素依次递增(递减), 这就要求在入栈的时候, 如果该元素比栈顶元素大, 直接入栈, 否则弹出元素直到栈顶元素小于入栈元素
这样如果是需要入栈的元素比栈顶小, 说明它是栈顶的第一个右边小于它的元素, 其下标就是右边柱子 right_index, 左边的下标 left_index就是栈顶元素的下标, 单调栈里记录下标, 保证下标对应的 height值递增
时间复杂度O(n), 空间复杂度O(n)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int largestRectangleArea(vector<int>& heights) 
    {
         heights.push_back(0);
         stack<int> s;
         int result = 0;
         for (int i = 0; i < heights.size(); i++)
         {
             while (!s.empty() and heights[i] < heights[s.top()])
             {
                 int cur = s.top();
                 s.pop();
                 result = max(result, heights[cur] * (s.empty() ? i : (i - s.top() - 1)));
             }
             s.push(i);
         }
         return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int largestRectangleArea(int[] heights) {
        int result = 0;
        Stack<Integer> stack = new Stack<>();
        int temp[] = new int[heights.length + 1];
        for (int i = 0; i < heights.length; i++) temp[i] = heights[i];
        for (int i = 0; i < temp.length; i++) {
            while (!stack.empty() && temp[stack.peek()] > temp[i]) {
                int cur = stack.pop();
                result = Math.max(result, (stack.empty() ? i : (i - stack.peek() - 1)) * temp[cur]);
            }
            stack.push(i);
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def largestRectangleArea(self, heights: List[int]) -> int:
        heights += [0]
        stack, result = [], 0
        for i in range(len(heights)):
            while stack and heights[stack[-1]] > heights[i]:
                cur = stack.pop()
                result = max(result, (i - stack[-1] - 1 if stack else i) * heights[cur])
            stack.append(i)
        return result
```
