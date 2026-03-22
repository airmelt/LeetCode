# 1835 Find XOR Sum of All Pairs Bitwise AND 所有数对按位与结果的异或和

__Description:__

The __XOR sum__ of a list is the bitwise `XOR` of all its elements. If the list only contains one element, then its __XOR sum__ will be equal to this element.

- For example, the __XOR sum__ of `[1,2,3,4]` is equal to `1 XOR 2 XOR 3 XOR 4 = 4`, and the __XOR sum__ of `[3]` is equal to `3`.

You are given two __0-indexed__ arrays `arr1` and `arr2` that consist only of non-negative integers.

Consider the list containing the result of `arr1[i] AND arr2[j]` (bitwise `AND`) for every `(i, j)` pair where `0 <= i < arr1.length` and `0 <= j < arr2.length`.

Return the XOR sum of the aforementioned list.

__Example:__

Example 1:

```text
Input: arr1 = [1,2,3], arr2 = [6,5]
Output: 0
Explanation: The list = [1 AND 6, 1 AND 5, 2 AND 6, 2 AND 5, 3 AND 6, 3 AND 5] = [0,1,2,0,2,1].
The XOR sum = 0 XOR 1 XOR 2 XOR 0 XOR 2 XOR 1 = 0.
```

Example 2:

```text
Input: arr1 = [12], arr2 = [4]
Output: 4
Explanation: The list = [12 AND 4] = [4]. The XOR sum = 4.
```

__Constraints:__

- `1 <= arr1.length, arr2.length <= 10 ^ 5`
- `0 <= arr1[i], arr2[j] <= 10 ^ 9`

__题目描述:__

列表的 __异或和__（__XOR sum__）指对所有元素进行按位 `XOR` 运算的结果。如果列表中仅有一个元素，那么其 __异或和__ 就等于该元素。

- 例如， `[1,2,3,4]` 的 __异或和__ 等于 `1 XOR 2 XOR 3 XOR 4 = 4` ，而 `[3]` 的 __异或和__ 等于 `3` 。

给你两个下标 __从 0 开始__ 计数的数组 `arr1` 和 `arr2` ，两数组均由非负整数组成。

根据每个 `(i, j)` 数对，构造一个由 `arr1[i] AND arr2[j]`（按位 `AND` 运算）结果组成的列表。其中 `0 <= i < arr1.length` 且 `0 <= j < arr2.length` 。

返回上述列表的 异或和 。

__示例:__

示例 1：

```text
输入：arr1 = [1,2,3], arr2 = [6,5]
输出：0
解释：列表 = [1 AND 6, 1 AND 5, 2 AND 6, 2 AND 5, 3 AND 6, 3 AND 5] = [0,1,2,0,2,1] ，
异或和 = 0 XOR 1 XOR 2 XOR 0 XOR 2 XOR 1 = 0 。
```

示例 2：

```text
输入：arr1 = [12], arr2 = [4]
输出：4
解释：列表 = [12 AND 4] = [4] ，异或和 = 4 。
```

__提示：__

- `1 <= arr1.length, arr2.length <= 10 ^ 5`
- `0 <= arr1[i], arr2[j] <= 10 ^ 9`

__思路:__

```text
位运算
史上最简单困难题, 仅需一行
转化为求两个数组的异或和再按位与即可
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int getXORSum(vector<int>& arr1, vector<int>& arr2) 
    {
        int a = 0, b = 0;
        for (const auto& num : arr1) a ^= num;
        for (const auto& num : arr2) b ^= num;
        return a & b;
    }
};
```

__Java__:

```Java
class Solution {
    public int getXORSum(int[] arr1, int[] arr2) {
        int a = 0, b = 0;
        for (int num : arr1) a ^= num;
        for (int num : arr2) b ^= num;
        return a & b;
    }
}
```

__Python__:

```Python
class Solution:
    def getXORSum(self, arr1: List[int], arr2: List[int]) -> int:
        return functools.reduce(lambda x, y: x ^ y, arr1) & functools.reduce(lambda x, y: x ^ y, arr2)
```
