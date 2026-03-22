# 852 Peak Index in a Mountain Array 山脉数组的峰顶索引

__Description__:
Let's call an array A a mountain if the following properties hold:

A.length >= 3
There exists some 0 < i < A.length - 1 such that A[0] < A[1] < ... A[i-1] < A[i] > A[i+1] > ... > A[A.length - 1]
Given an array that is definitely a mountain, return any i such that A[0] < A[1] < ... A[i-1] < A[i] > A[i+1] > ... > A[A.length - 1].

__Example:__

Example 1:

Input: [0,1,0]
Output: 1

Example 2:

Input: [0,2,1,0]
Output: 1

__Note:__

3 <= A.length <= 10000
0 <= A[i] <= 10^6
A is a mountain, as defined above.

__题目描述__:
我们把符合下列属性的数组 A 称作山脉：

A.length >= 3
存在 0 < i < A.length - 1 使得A[0] < A[1] < ... A[i-1] < A[i] > A[i+1] > ... > A[A.length - 1]
给定一个确定为山脉的数组，返回任何满足 A[0] < A[1] < ... A[i-1] < A[i] > A[i+1] > ... > A[A.length - 1] 的 i 的值。

__示例 :__

示例 1：

输入：[0,1,0]
输出：1

示例 2：

输入：[0,2,1,0]
输出：1

__提示：__

3 <= A.length <= 10000
0 <= A[i] <= 10^6
A 是如上定义的山脉

__思路__:

由于数组有序, 应该使用二分查找, 找到 mid, 如果 A[mid] < A[mid + 1], 则山峰一定在 mid右边
这里由于是单峰山脉, 其实找到最大值下标就是山峰
时间复杂度O(n), 空间复杂度O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int peakIndexInMountainArray(vector<int>& A) 
    {
        int low = 0, high = A.size() - 1;
        while (low < high)
        {
            int mid = low + ((high - low) >> 1);
            if (A[mid] > A[mid + 1] && A[mid] > A[mid - 1]) return mid;
            else if (A[mid] < A[mid + 1]) low = mid + 1;
            else high = mid;
        }
        return low;
    }
};
```

__Java__:

```Java
class Solution {
    public int peakIndexInMountainArray(int[] A) {
        int low = 0, high = A.length - 1;
        while (low + 1 < high) {
            int mid = (low + high) >>> 1;
            if (A[mid] < A[mid + 1]) low = mid + 1;
            else high = mid;
        }
        return (low == A.length - 1 || A[low] > A[low + 1]) ? low : high;
    }
}
```

__Python__:

```Python
class Solution:
    def peakIndexInMountainArray(self, A: List[int]) -> int:
        return A.index(max(A))
```
