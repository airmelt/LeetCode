# 989 Add to Array-Form of Integer 数组形式的整数加法

__Description__:
For a non-negative integer X, the array-form of X is an array of its digits in left to right order.  For example, if X = 1231, then the array form is [1,2,3,1].

Given the array-form A of a non-negative integer X, return the array-form of the integer X+K.

__Example:__

Example 1:

Input: A = [1,2,0,0], K = 34
Output: [1,2,3,4]
Explanation: 1200 + 34 = 1234

Example 2:

Input: A = [2,7,4], K = 181
Output: [4,5,5]
Explanation: 274 + 181 = 455

Example 3:

Input: A = [2,1,5], K = 806
Output: [1,0,2,1]
Explanation: 215 + 806 = 1021

Example 4:

Input: A = [9,9,9,9,9,9,9,9,9,9], K = 1
Output: [1,0,0,0,0,0,0,0,0,0,0]
Explanation: 9999999999 + 1 = 10000000000

__Note:__

1 <= A.length <= 10000
0 <= A[i] <= 9
0 <= K <= 10000
If A.length > 1, then A[0] != 0

__题目描述__:
对于非负整数 X 而言，X 的数组形式是每位数字按从左到右的顺序形成的数组。例如，如果 X = 1231，那么其数组形式为 [1,2,3,1]。

给定非负整数 X 的数组形式 A，返回整数 X+K 的数组形式。

__示例 :__

示例 1：

输入：A = [1,2,0,0], K = 34
输出：[1,2,3,4]
解释：1200 + 34 = 1234

示例 2：

输入：A = [2,7,4], K = 181
输出：[4,5,5]
解释：274 + 181 = 455

示例 3：

输入：A = [2,1,5], K = 806
输出：[1,0,2,1]
解释：215 + 806 = 1021

示例 4：

输入：A = [9,9,9,9,9,9,9,9,9,9], K = 1
输出：[1,0,0,0,0,0,0,0,0,0,0]
解释：9999999999 + 1 = 10000000000

__提示：__

1 <= A.length <= 10000
0 <= A[i] <= 9
0 <= K <= 10000
如果 A.length > 1，那么 A[0] != 0

__思路__:

注意数组 A不能转化为整数, 因为数组长度可能为 10000, 远远超过 int能表示的范围

1. 先逆序, 然后从第一位加上 K, 然后将数组 A的元素对 10取余不断进位, 注意判断数组 A的大小需要存储值判断, 因为数组 A的大小会动态变化, 最后输出时逆序
2. 另开一个数组记录, 从后往前遍历, 按照加法设置进位
时间复杂度O(n), 空间复杂度O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    vector<int> addToArrayForm(vector<int>& A, int K) 
    {
        int len = A.size();
        reverse(A.begin(), A.end());
        A[0] += K;
        int i = 0;
        while (A[i] > 9) 
        {
            if (i > len - 2) A.push_back(0);
            A[i + 1] += A[i] / 10;
            A[i] %= 10;
            i++;
        }
        reverse(A.begin(), A.end());
        return A;
    }
};
```

__Java__:

```Java
class Solution {
    public List<Integer> addToArrayForm(int[] A, int K) {
        LinkedList<Integer> result = new LinkedList<>();
        int i = A.length - 1;
        while (i > -1 || K > 0) {
            if (i > -1) K += A[i];
            result.addFirst(K % 10);
            K /= 10;
            i--;
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def addToArrayForm(self, A: List[int], K: int) -> List[int]:
        return list(map(int, str(int(''.join(map(str, A))) + K)))
```
