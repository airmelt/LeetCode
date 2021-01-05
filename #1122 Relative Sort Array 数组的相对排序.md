# 1122 Relative Sort Array 数组的相对排序

__Description__:
Given two arrays arr1 and arr2, the elements of arr2 are distinct, and all elements in arr2 are also in arr1.

Sort the elements of arr1 such that the relative ordering of items in arr1 are the same as in arr2.  Elements that don't appear in arr2 should be placed at the end of arr1 in ascending order.

__Example:__

Example 1:

Input: arr1 = [2,3,1,3,2,4,6,7,9,2,19], arr2 = [2,1,4,3,9,6]
Output: [2,2,2,1,4,3,3,9,6,7,19]

__Constraints:__

arr1.length, arr2.length <= 1000
0 <= arr1[i], arr2[i] <= 1000
Each arr2[i] is distinct.
Each arr2[i] is in arr1.

__题目描述__:
给你两个数组，arr1 和 arr2，

arr2 中的元素各不相同
arr2 中的每个元素都出现在 arr1 中
对 arr1 中的元素进行排序，使 arr1 中项的相对顺序和 arr2 中的相对顺序相同。未在 arr2 中出现过的元素需要按照升序放在 arr1 的末尾。

__示例 :__

输入：arr1 = [2,3,1,3,2,4,6,7,9,2,19], arr2 = [2,1,4,3,9,6]
输出：[2,2,2,1,4,3,3,9,6,7,19]

__提示：__

arr1.length, arr2.length <= 1000
0 <= arr1[i], arr2[i] <= 1000
arr2 中的元素 arr2[i] 各不相同
arr2 中的每个元素 arr2[i] 都出现在 arr1 中

__思路__:

桶排序, 先存放 arr2中的值, 再按顺序存放 arr1中的值
时间复杂度O(n), 空间复杂度O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    vector<int> relativeSortArray(vector<int>& arr1, vector<int>& arr2) 
    {
        int index = 0;
        const int ZERO = 0, BUCKET_SIZE = 1001;
        vector<int> count(BUCKET_SIZE, ZERO);
        for (auto i : arr1) ++count[i];
        for (auto i : arr2)
        {
            while (count[i])
            {
                --count[i];
                arr1[index++] = i;
            }
        }
        for (int i = 0; i < BUCKET_SIZE; ++i) while (count[i]--) arr1[index++] = i;
        return arr1;
    }
};
```

__Java__:

```Java
class Solution {
    public int[] relativeSortArray(int[] arr1, int[] arr2) {
        final int BUCKET_SIZE = 1001;
        int count[] = new int[BUCKET_SIZE], index = 0;
        for (int i : arr1) ++count[i];
        for (int i : arr2) {
            while (count[i] > 0) {
                --count[i];
                arr1[index++] = i;
            }
        }
        for (int i = 0; i < BUCKET_SIZE; ++i) while (count[i]-- > 0) arr1[index++] = i;
        return arr1;
    }
}
```

__Python__:

```Python
class Solution:
    def relativeSortArray(self, arr1: List[int], arr2: List[int]) -> List[int]:
        return sorted(arr1, key=(arr2 + sorted(set(arr1) - set(arr2))).index)
```
