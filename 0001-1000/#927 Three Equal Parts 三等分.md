# 927 Three Equal Parts 三等分

__Description__:
You are given an array arr which consists of only zeros and ones, divide the array into three non-empty parts such that all of these parts represent the same binary value.

If it is possible, return any [i, j] with i + 1 < j, such that:

arr[0], arr[1], ..., arr[i] is the first part,
arr[i + 1], arr[i + 2], ..., arr[j - 1] is the second part, and
arr[j], arr[j + 1], ..., arr[arr.length - 1] is the third part.
All three parts have equal binary values.
If it is not possible, return [-1, -1].

Note that the entire part is used when considering what binary value it represents. For example, [1,1,0] represents 6 in decimal, not 3. Also, leading zeros are allowed, so [0,1,1] and [1,1] represent the same value.

__Example:__

Example 1:

Input: arr = [1,0,1,0,1]
Output: [0,3]
Example 2:

Input: arr = [1,1,0,1,1]
Output: [-1,-1]

Example 3:

Input: arr = [1,1,0,0,1]
Output: [0,2]

__Constraints:__

3 <= arr.length <= 3 * 10^4
arr[i] is 0 or 1

__题目描述__:
给定一个由 0 和 1 组成的数组 A，将数组分成 3 个非空的部分，使得所有这些部分表示相同的二进制值。

如果可以做到，请返回任何 [i, j]，其中 i+1 < j，这样一来：

A[0], A[1], ..., A[i] 组成第一部分；
A[i+1], A[i+2], ..., A[j-1] 作为第二部分；
A[j], A[j+1], ..., A[A.length - 1] 是第三部分。
这三个部分所表示的二进制值相等。
如果无法做到，就返回 [-1, -1]。

注意，在考虑每个部分所表示的二进制时，应当将其看作一个整体。例如，[1,1,0] 表示十进制中的 6，而不会是 3。此外，前导零也是被允许的，所以 [0,1,1] 和 [1,1] 表示相同的值。

__示例 :__

示例 1：

输入：[1,0,1,0,1]
输出：[0,3]

示例 2：

输出：[1,1,0,1,1]
输出：[-1,-1]

__提示:__

3 <= A.length <= 30000
A[i] == 0 或 A[i] == 1

__思路__:

模拟
先计算出数组的总和
如果数组总和为 0, 任意升序返回 2 个 [0, n) 的数即可
如果数组总和不为 3 的倍数, 返回 [-1, -1] 因为永远不可能等分 3 份
否则去掉前导 0, 记录 3 个子数组开始的位置
然后去掉后置 0, 遍历子数组观察子数组是否相等
时间复杂度为 O(n), 空间复杂度为 O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    vector<int> threeEqualParts(vector<int>& arr) 
    {
        int s = accumulate(arr.begin(), arr.end(), 0), start = 0, mid = 0, end = 0, n = arr.size(), cur = 0;
        if (s % 3) return { -1, -1 };
        if (!s) return { 0, n - 1 };
        s /= 3;
        for (int i = 0; i < n; i++) 
        {
            if (!cur) start = i;
            if (cur == s) mid = i;
            if (cur == (s << 1)) end = i;
            cur += arr[i];
        }
        int last = n - end;
        for (int i = 0; i < last; i++) if (arr[start + i] != arr[mid + i] or arr[start + i] != arr[end + i]) return { -1, -1 };
        return { start + last - 1, mid + last };
    }
};
```

__Java__:

```Java
class Solution {
    public int[] threeEqualParts(int[] arr) {
        int s = 0, start = 0, mid = 0, end = 0, n = arr.length, cur = 0;
        for (int num : arr) s += num;
        if (s % 3 != 0) return new int[]{ -1, -1 };
        if (s == 0) return new int[]{ 0, n - 1 };
        s /= 3;
        for (int i = 0; i < n; i++) {
            if (cur == 0) start = i;
            if (cur == s) mid = i;
            if (cur == (s << 1)) end = i;
            cur += arr[i];
        }
        int last = n - end;
        for (int i = 0; i < last; i++) if (arr[start + i] != arr[mid + i] || arr[start + i] != arr[end + i]) return new int[]{ -1, -1 };
        return new int[]{ start + last - 1, mid + last };
    }
}
```

__Python__:

```Python
class Solution:
    def threeEqualParts(self, arr: List[int]) -> List[int]:
        s, start, mid, end, n, cur = sum(arr), 0, 0, 0, len(arr), 0
        if s % 3:
            return [-1, -1]
        if not s:
            return [0, n - 1]
        s //= 3
        for i in range(n):
            if not cur:
                start = i
            if cur == s:
                mid = i
            if cur == (s << 1):
                end = i
            cur += arr[i]
        last = n - end
        for i in range(last):
            if arr[start + i] != arr[mid + i] or arr[start + i] != arr[end + i]:
                return [-1, -1]
        return [start + last - 1, mid + last]
```
