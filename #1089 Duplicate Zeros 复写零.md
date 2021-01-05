# 1089 Duplicate Zeros 复写零

__Description__:
Given a fixed length array arr of integers, duplicate each occurrence of zero, shifting the remaining elements to the right.

Note that elements beyond the length of the original array are not written.

Do the above modifications to the input array in place, do not return anything from your function.

__Example:__

Input: [1,0,2,3,0,4,5,0]
Output: null
Explanation: After calling your function, the input array is modified to: [1,0,0,2,3,0,0,4]

Example 2:

Input: [1,2,3]
Output: null
Explanation: After calling your function, the input array is modified to: [1,2,3]

__Note:__

1 <= arr.length <= 10000
0 <= arr[i] <= 9

__题目描述__:
给你一个长度固定的整数数组 arr，请你将该数组中出现的每个零都复写一遍，并将其余的元素向右平移。

注意：请不要在超过该数组长度的位置写入元素。

要求：请对输入的数组 就地 进行上述修改，不要从函数返回任何东西。

__示例 :__

示例 1：

输入：[1,0,2,3,0,4,5,0]
输出：null
解释：调用函数后，输入的数组将被修改为：[1,0,0,2,3,0,0,4]

示例 2：

输入：[1,2,3]
输出：null
解释：调用函数后，输入的数组将被修改为：[1,2,3]

__提示：__

1 <= arr.length <= 10000
0 <= arr[i] <= 9

__思路__:
先遍历数组, 记录 0的个数
从后往前遍历数组, 将 0的个数当作偏移量, 移动数组
时间复杂度O(n), 空间复杂度O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    void duplicateZeros(vector<int>& arr) 
    {
        int zero = 0, s = arr.size();
        for (auto i : arr) if (!i) ++zero;
        for (int i = s - 1; i > -1; i--)
        {
            if (!arr[i])
            {
                --zero;
                if (i + zero < s) arr[i + zero] = 0;
                if (i + zero + 1 < s) arr[i + zero + 1] = 0;
            }
            else if (i + zero < s) arr[i + zero] = arr[i]; 
        }
    }
};
```

__Java__:

```Java
class Solution {
    public void duplicateZeros(int[] arr) {
        int zero = 0, s = arr.length;
        for (int i : arr) if (i == 0) ++zero;
        for (int i = s - 1; i > -1; --i) {
            if (arr[i] == 0) {
                --zero;
                if (i + zero < s) arr[i + zero] = 0;
                if (i + zero + 1 < s) arr[i + zero + 1] = 0;
            }
            else if (i + zero < s) arr[i + zero] = arr[i]; 
        }
    }
}
```

__Python__:

```Python
class Solution:
    def duplicateZeros(self, arr: List[int]) -> None:
        """
        Do not return anything, modify arr in-place instead.
        """
        zero, s = 0, len(arr)
        for i in arr:
            if not i:
                zero += 1
        for i in range(s - 1, -1, -1):
            if not arr[i]:
                zero -= 1
                if i + zero < s:
                    arr[i + zero] = 0;
                if i + zero + 1 < s:
                    arr[i + zero + 1] = 0;
            elif i + zero < s:
                arr[i + zero] = arr[i]; 
```
