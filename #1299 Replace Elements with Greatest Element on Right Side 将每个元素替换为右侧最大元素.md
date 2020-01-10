__Description__:
Given an array arr, replace every element in that array with the greatest element among the elements to its right, and replace the last element with -1.

After doing so, return the array.

__Example:__
Example 1:

Input: arr = [17,18,5,4,6,1]
Output: [18,6,6,6,1,-1]
 
__Constraints:__

1 <= arr.length <= 10^4
1 <= arr[i] <= 10^5

__题目描述__:
给你一个数组 arr ，请你将每个元素用它右边最大的元素替换，如果是最后一个元素，用 -1 替换。

完成所有替换操作后，请你返回这个数组。

__示例 :__

输入：arr = [17,18,5,4,6,1]
输出：[18,6,6,6,1,-1]
 
__提示：__

1 <= arr.length <= 10^4
1 <= arr[i] <= 10^5

__思路__:
逆序遍历, 记录最大值和当前值, 原地修改即可
时间复杂度O(n), 空间复杂度O(1)

__代码__:
__C++__:
```C++
class Solution 
{
public:
    vector<int> replaceElements(vector<int>& arr) 
    {
        int right = -1;
        for (int i = arr.size() - 1; i > -1; i--)
        {
            if (arr[i] > right) swap(arr[i], right);
            else arr[i] = right;
        }
        return arr;
    }
};
```

__Java__:
```Java
class Solution {
    public int[] replaceElements(int[] arr) {
        int right = -1;
        for (int i = arr.length - 1; i > -1; i--) {
            int temp = arr[i];
            arr[i] = right;
            if (temp > right) right = temp;
        }
        return arr;
    }
}
```

__Python__:
```Python
class Solution:
    def replaceElements(self, arr: List[int]) -> List[int]:
        right = -1
        for i in range(len(arr) - 1, -1, -1):
            temp = arr[i]
            arr[i] = right
            right = max(right, temp)
        return arr
```