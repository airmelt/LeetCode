# 1846 Maximum Element After Decreasing and Rearranging 减小和重新排列数组后的最大元素

__Description:__

You are given an array of positive integers `arr`. Perform some operations (possibly none) on `arr` so that it satisfies these conditions:

- The value of the __first__ element in `arr` must be `1`.
- The absolute difference between any 2 adjacent elements must be __less than or equal to__ `1`. In other words, `abs(arr[i] - arr[i - 1]) <= 1` for each `i` where `1 <= i < arr.length` (__0-indexed__). `abs(x)` is the absolute value of `x`.

There are 2 types of operations that you can perform any number of times:

- __Decrease__ the value of any element of `arr` to a __smaller positive integer__.
- __Rearrange__ the elements of `arr` to be in any order.

Return _the __maximum__ possible value of an element in_ `arr` _after performing the operations to satisfy the conditions_.

__Example:__

Example 1:

```text
Input:  arr = [2,2,1,2,1]
Output:  2
Explanation:  
We can satisfy the conditions by rearranging `arr` so it becomes `[1,2,2,2,1]`.
The largest element in `arr` is 2.
```

Example 2:

```text
Input:  arr = [100,1,1000]
Output:  3
Explanation:  
One possible way to satisfy the conditions is by doing the following:
1. Rearrange `arr` so it becomes `[1,100,1000]`.
2. Decrease the value of the second element to 2.
3. Decrease the value of the third element to 3.
Now `arr = [1,2,3], which` satisfies the conditions.
The largest element in `arr is 3.`
```

Example 3:

```text
Input: arr = [1,2,3,4,5]
Output: 5
Explanation: The array already satisfies the conditions, and the largest element is 5.
```

__Constraints:__

- `1 <= arr.length <= 10 ^ 5`
- `1 <= arr[i] <= 10 ^ 9`

__题目描述:__

给你一个正整数数组 `arr` 。请你对 `arr` 执行一些操作（也可以不进行任何操作），使得数组满足以下条件:

- `arr` 中 __第一个__ 元素必须为 `1` 。
- 任意相邻两个元素的差的绝对值 __小于等于__ `1` ，也就是说，对于任意的 `1 <= i < arr.length` （__数组下标从 0 开始__），都满足 `abs(arr[i] - arr[i - 1]) <= 1` 。 `abs(x)` 为 `x` 的绝对值。

你可以执行以下 2 种操作任意次：

- __减小__ `arr` 中任意元素的值，使其变为一个 __更小的正整数__ 。
- __重新排列__ `arr` 中的元素，你可以以任意顺序重新排列。

请你返回执行以上操作后，在满足前文所述的条件下， `arr` 中可能的 __最大值__ 。

__示例:__

示例 1：

```text
输入: arr = [2,2,1,2,1]
输出: 2
解释: 
我们可以重新排列 arr 得到 `[1,2,2,2,1] ，该数组满足所有条件。`
arr 中最大元素为 2 。
```

示例 2：

```text
输入: arr = [100,1,1000]
输出: 3
解释: 
一个可行的方案如下:
1. 重新排列 `arr` 得到 `[1,100,1000] 。`
2. 将第二个元素减小为 2 。
3. 将第三个元素减小为 3 。
现在 `arr = [1,2,3] ，满足所有条件。`
arr 中最大元素为 3 。
```

示例 3：

```text
输入：arr = [1,2,3,4,5]
输出：5
解释：数组已经满足所有条件，最大元素为 5 。
```

__提示：__

- `1 <= arr.length <= 10 ^ 5`
- `1 <= arr[i] <= 10 ^ 9`

__思路:__

```text
贪心
先将数组排序
将第一个数设为 1
排序之后如果后一个数比前一个数大, 调整为前一个数加 1
时间复杂度为 O(NlogN), 空间复杂度为 O(logN)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int maximumElementAfterDecrementingAndRearranging(vector<int>& arr) 
    {
        sort(arr.begin(), arr.end());
        arr.front() = 1;
        for (int i = 1, n = arr.size(); i < n; i++) if (arr[i] - arr[i - 1] > 1) arr[i] = arr[i - 1] + 1;
        return arr.back();
    }
};
```

__Java__:

```Java
class Solution {
    public int maximumElementAfterDecrementingAndRearranging(int[] arr) {
        Arrays.sort(arr);
        arr[0] = 1;
        for (int i = 1, n = arr.length; i < n; i++) if (arr[i] - arr[i - 1] > 1) arr[i] = arr[i - 1] + 1;
        return arr[arr.length - 1];
    }
}
```

__Python__:

```Python
class Solution:
    def maximumElementAfterDecrementingAndRearranging(self, arr: List[int]) -> int:
        arr.sort()
        arr[0], n = 1, len(arr)
        for i in range(1, n):
            if arr[i] - arr[i - 1] > 1:
                arr[i] = arr[i - 1] + 1
        return arr[-1]
```
