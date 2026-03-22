# 1131 Maximum of Absolute Value Expression 绝对值表达式的最大值

__Description__:
Given two arrays of integers with equal lengths, return the maximum value of:

|arr1[i] - arr1[j]| + |arr2[i] - arr2[j]| + |i - j|

where the maximum is taken over all 0 <= i, j < arr1.length.

__Example:__

Example 1:

Input: arr1 = [1,2,3,4], arr2 = [-1,4,5,6]
Output: 13

Example 2:

Input: arr1 = [1,-2,-5,0,10], arr2 = [0,-2,-1,-7,-4]
Output: 20

__Constraints:__

2 <= arr1.length == arr2.length <= 40000
-10^6 <= arr1[i], arr2[i] <= 10^6

__题目描述__:
给你两个长度相等的整数数组，返回下面表达式的最大值：

|arr1[i] - arr1[j]| + |arr2[i] - arr2[j]| + |i - j|

其中下标 i，j 满足 0 <= i, j < arr1.length。

__示例 :__

示例 1：

输入：arr1 = [1,2,3,4], arr2 = [-1,4,5,6]
输出：13

示例 2：

输入：arr1 = [1,-2,-5,0,10], arr2 = [0,-2,-1,-7,-4]
输出：20

__提示:__

2 <= arr1.length == arr2.length <= 40000
-10^6 <= arr1[i], arr2[i] <= 10^6

__思路__:

数学
将绝对值符号去掉一共有 8 种组合

```text
|arr1[i] - arr1[j]| + |arr2[i] - arr2[j]| + |i - j|
 
 =  (arr1[i] + arr2[i] + i) - (arr1[j] + arr2[j] + j)
 =  (arr1[i] + arr2[i] - i) - (arr1[j] + arr2[j] - j)
 =  (arr1[i] - arr2[i] + i) - (arr1[j] - arr2[j] + j)
 =  (arr1[i] - arr2[i] - i) - (arr1[j] - arr2[j] - j)
 = -(arr1[i] + arr2[i] + i) + (arr1[j] + arr2[j] + j)
 = -(arr1[i] + arr2[i] - i) + (arr1[j] + arr2[j] - j)
 = -(arr1[i] - arr2[i] + i) + (arr1[j] - arr2[j] + j)
 = -(arr1[i] - arr2[i] - i) + (arr1[j] - arr2[j] - j)
 
令
A = arr1[i] + arr2[i] + i
B = arr1[i] + arr2[i] - i
C = arr1[i] - arr2[i] + i
D = arr1[i] - arr2[i] - i

max(|arr1[i] - arr1[j]| + |arr2[i] - arr2[j]| + |i - j|)
= max(max(A) - min(A),
      max(B) - min(B),
      max(C) - min(C),
      max(D) - min(D))
```

时间复杂度为 O(n), 空间复杂度为 O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int maxAbsValExpr(vector<int>& arr1, vector<int>& arr2) 
    {
        int max1 = INT_MIN, max2 = INT_MIN, max3 = INT_MIN, max4 = INT_MIN, min1 = INT_MAX, min2 = INT_MAX, min3 = INT_MAX, min4 = INT_MAX, n = arr1.size();
        for (int i = 0; i < n; i++)
        {
            max1 = max(max1, arr1[i] + arr2[i] + i);
            max2 = max(max2, -arr1[i] + arr2[i] + i);
            max3 = max(max3, arr1[i] - arr2[i] + i);
            max4 = max(max4, arr1[i] + arr2[i] - i);
            min1 = min(min1, arr1[i] + arr2[i] + i);
            min2 = min(min2, -arr1[i] + arr2[i] + i);
            min3 = min(min3, arr1[i] - arr2[i] + i);
            min4 = min(min4, arr1[i] + arr2[i] - i);
        }
        return max({max1 - min1, max2 - min2, max3 - min3, max4 - min4});
    }
};
```

__Java__:

```Java
class Solution {
    public int maxAbsValExpr(int[] arr1, int[] arr2) {
        int max1 = Integer.MIN_VALUE, max2 = Integer.MIN_VALUE, max3 = Integer.MIN_VALUE, max4 = Integer.MIN_VALUE, min1 = Integer.MAX_VALUE, min2 = Integer.MAX_VALUE, min3 = Integer.MAX_VALUE, min4 = Integer.MAX_VALUE, n = arr1.length;
        for (int i = 0; i < n; i++)
        {
            max1 = Math.max(max1, arr1[i] + arr2[i] + i);
            max2 = Math.max(max2, -arr1[i] + arr2[i] + i);
            max3 = Math.max(max3, arr1[i] - arr2[i] + i);
            max4 = Math.max(max4, arr1[i] + arr2[i] - i);
            min1 = Math.min(min1, arr1[i] + arr2[i] + i);
            min2 = Math.min(min2, -arr1[i] + arr2[i] + i);
            min3 = Math.min(min3, arr1[i] - arr2[i] + i);
            min4 = Math.min(min4, arr1[i] + arr2[i] - i);
        }
        return Math.max(Math.max(max1 - min1, max2 - min2), Math.max(max3 - min3, max4 - min4));
    }
}
```

__Python__:

```Python
class Solution:
    def maxAbsValExpr(self, arr1: List[int], arr2: List[int]) -> int:
        max1, max2, max3, max4, min1, min2, min3, min4, n = -float('inf'), -float('inf'), -float('inf'), -float('inf'), float('inf'), float('inf'), float('inf'), float('inf'), len(arr1)
        for i in range(n):
            max1, max2, max3, max4, min1, min2, min3, min4 = max(max1, arr1[i] + arr2[i] + i), max(max2, -arr1[i] + arr2[i] + i), max(max3, arr1[i] - arr2[i] + i), max(max4, arr1[i] + arr2[i] - i), min(min1, arr1[i] + arr2[i] + i), min(min2, -arr1[i] + arr2[i] + i), min(min3, arr1[i] - arr2[i] + i), min(min4, arr1[i] + arr2[i] - i)
        return max((max1 - min1, max2 - min2, max3 - min3, max4 - min4))
```
