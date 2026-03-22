# 1073 Adding Two Negabinary Numbers 负二进制数相加

__Description__:
Given two numbers arr1 and arr2 in base -2, return the result of adding them together.

Each number is given in array format:  as an array of 0s and 1s, from most significant bit to least significant bit.  For example, arr = [1,1,0,1] represents the number (-2)^3 + (-2)^2 + (-2)^0 = -3.  A number arr in array, format is also guaranteed to have no leading zeros: either arr == [0] or arr[0] == 1.

Return the result of adding arr1 and arr2 in the same format: as an array of 0s and 1s with no leading zeros.

__Example:__

Example 1:

Input: arr1 = [1,1,1,1,1], arr2 = [1,0,1]
Output: [1,0,0,0,0]
Explanation: arr1 represents 11, arr2 represents 5, the output represents 16.

Example 2:

Input: arr1 = [0], arr2 = [0]
Output: [0]

Example 3:

Input: arr1 = [0], arr2 = [1]
Output: [1]

__Constraints:__

1 <= arr1.length, arr2.length <= 1000
arr1[i] and arr2[i] are 0 or 1
arr1 and arr2 have no leading zeros

__题目描述__:
给出基数为 -2 的两个数 arr1 和 arr2，返回两数相加的结果。

数字以 数组形式 给出：数组由若干 0 和 1 组成，按最高有效位到最低有效位的顺序排列。例如，arr = [1,1,0,1] 表示数字 (-2)^3 + (-2)^2 + (-2)^0 = -3。数组形式 中的数字 arr 也同样不含前导零：即 arr == [0] 或 arr[0] == 1。

返回相同表示形式的 arr1 和 arr2 相加的结果。两数的表示形式为：不含前导零、由若干 0 和 1 组成的数组。

__示例 :__

示例 1：

输入：arr1 = [1,1,1,1,1], arr2 = [1,0,1]
输出：[1,0,0,0,0]
解释：arr1 表示 11，arr2 表示 5，输出表示 16 。

示例 2：

输入：arr1 = [0], arr2 = [0]
输出：[0]

示例 3：

输入：arr1 = [0], arr2 = [1]
输出：[1]

__提示:__

1 <= arr1.length, arr2.length <= 1000
arr1[i] 和 arr2[i] 都是 0 或 1
arr1 和 arr2 都没有前导0

__思路__:

模拟
与二进制加法类似
进位有负数的情况, 可以使用位运算避免
注意最后要去掉前导零
时间复杂度O(n), 空间复杂度O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    vector<int> addNegabinary(vector<int>& arr1, vector<int>& arr2) 
    {
        vector<int> result;
        int i = arr1.size() - 1, j = arr2.size() - 1, carry = 0, cur = 0, div = 0, mod = 0;
        while (i > -1 or j > -1 or carry) 
        {
            cur = (i < 0 ? 0 : arr1[i]) + (j < 0 ? 0 : arr2[j]) + carry;
            div = cur >> 1;
            mod = cur & 1;
            result.emplace_back(mod);
            carry = -div;
            --i;
            --j;
        }
        while (result.size() > 1 and !result.back()) result.pop_back();
        reverse(result.begin(), result.end());
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int[] addNegabinary(int[] arr1, int[] arr2) {
        List<Integer> list = new ArrayList<>();
        int i = arr1.length - 1, j = arr2.length - 1, carry = 0, cur = 0, div = 0, mod = 0;
        while (i > -1 || j > -1 || carry != 0) {
            cur = (i < 0 ? 0 : arr1[i]) + (j < 0 ? 0 : arr2[j]) + carry;
            div = cur >> 1;
            mod = cur & 1;
            list.add(mod);
            carry = -div;
            --i;
            --j;
        }
        while (list.size() > 1 && list.get(list.size() - 1) == 0) list.remove(list.size() - 1);
        int n = list.size(), result[] = new int[n];
        for (int k = 0; k < n; k++) result[k] = list.get(n - k - 1);
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def addNegabinary(self, arr1: List[int], arr2: List[int]) -> List[int]:
        result, carry, i, j = [], 0, len(arr1) - 1, len(arr2) - 1
        while i >= 0 or j >= 0 or carry:
            add = (0 if i < 0 else arr1[i]) + (0 if j < 0 else arr2[j]) + carry
            div, mod = divmod(add, 2)
            result.append(mod)
            carry = -div
            i, j = i - 1, j - 1
        while len(result) > 1 and not result[-1]:
            result.pop()
        return result[::-1]
```
