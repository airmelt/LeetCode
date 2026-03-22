# 969 Pancake Sorting 煎饼排序

__Description__:
Given an array of integers arr, sort the array by performing a series of pancake flips.

In one pancake flip we do the following steps:

Choose an integer k where 1 <= k <= arr.length.
Reverse the sub-array arr[0...k-1] (0-indexed).
For example, if arr = [3,2,1,4] and we performed a pancake flip choosing k = 3, we reverse the sub-array [3,2,1], so arr = [1,2,3,4] after the pancake flip at k = 3.

Return an array of the k-values corresponding to a sequence of pancake flips that sort arr. Any valid answer that sorts the array within 10 * arr.length flips will be judged as correct.

__Example:__

Example 1:

Input: arr = [3,2,4,1]
Output: [4,2,4,3]
Explanation:
We perform 4 pancake flips, with k values 4, 2, 4, and 3.
Starting state: arr = [3, 2, 4, 1]
After 1st flip (k = 4): arr = [1, 4, 2, 3]
After 2nd flip (k = 2): arr = [4, 1, 2, 3]
After 3rd flip (k = 4): arr = [3, 2, 1, 4]
After 4th flip (k = 3): arr = [1, 2, 3, 4], which is sorted.

Example 2:

Input: arr = [1,2,3]
Output: []
Explanation: The input is already sorted, so there is no need to flip anything.
Note that other answers, such as [3, 3], would also be accepted.

__Constraints:__

1 <= arr.length <= 100
1 <= arr[i] <= arr.length
All integers in arr are unique (i.e. arr is a permutation of the integers from 1 to arr.length).

__题目描述__:
给你一个整数数组 arr ，请使用 煎饼翻转 完成对数组的排序。

一次煎饼翻转的执行过程如下：

选择一个整数 k ，1 <= k <= arr.length
反转子数组 arr[0...k-1]（下标从 0 开始）
例如，arr = [3,2,1,4] ，选择 k = 3 进行一次煎饼翻转，反转子数组 [3,2,1] ，得到 arr = [1,2,3,4] 。

以数组形式返回能使 arr 有序的煎饼翻转操作所对应的 k 值序列。任何将数组排序且翻转次数在 10 * arr.length 范围内的有效答案都将被判断为正确。

__示例 :__

示例 1：

输入：[3,2,4,1]
输出：[4,2,4,3]
解释：
我们执行 4 次煎饼翻转，k 值分别为 4，2，4，和 3。
初始状态 arr = [3, 2, 4, 1]
第一次翻转后（k = 4）：arr = [1, 4, 2, 3]
第二次翻转后（k = 2）：arr = [4, 1, 2, 3]
第三次翻转后（k = 4）：arr = [3, 2, 1, 4]
第四次翻转后（k = 3）：arr = [1, 2, 3, 4]，此时已完成排序。

示例 2：

输入：[1,2,3]
输出：[]
解释：
输入已经排序，因此不需要翻转任何内容。
请注意，其他可能的答案，如 [3，3] ，也将被判断为正确。

__提示:__

1 <= arr.length <= 100
1 <= arr[i] <= arr.length
arr 中的所有整数互不相同（即，arr 是从 1 到 arr.length 整数的一个排列）

__思路__:

模拟
按照冒泡排序的思想
每次选择最大值将最大值翻转到数组的最后
时间复杂度为 O(n ^ 2), 空间复杂度为 O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    vector<int> pancakeSort(vector<int>& arr) 
    {
        vector<int> result;
        int n = arr.size();
        while (n > 1)
        {
            int pos = find(arr, n);
            reverse(arr.begin(), arr.begin() + pos + 1);
            result.emplace_back(pos + 1);
            reverse(arr.begin(), arr.begin() + n);
            result.emplace_back(n--);
        }
        return result;
    }
private:
    int find(vector<int>& arr, int item)
    {
        for (int i = 0, n = arr.size(); i < n; i++) if (arr[i] == item) return i;
        return -1;
    }
};
```

__Java__:

```Java
class Solution {
    public List<Integer> pancakeSort(int[] arr) {
        List<Integer> result = new ArrayList();
        int n = arr.length;
        while (n > 1) {
            int pos = find(arr, n);
            reverse(arr, 0, pos);
            result.add(pos + 1);
            reverse(arr, 0, n - 1);
            result.add(n--);
        }
        return result;
    }
    
    private void reverse(int[] arr, int begin, int end) {
        while (begin < end) {
            arr[begin] ^= arr[end];
            arr[end] ^= arr[begin];
            arr[begin++] ^= arr[end--];
        }
    }
    
    private int find(int[] arr, int item) {
        for (int i = 0, n = arr.length; i < n; i++) if (arr[i] == item) return i;
        return -1;
    }
}
```

__Python__:

```Python
class Solution:
    def pancakeSort(self, arr: List[int]) -> List[int]:
        result, n = [], len(arr)
        while n:
            pos = arr.index(n)
            result.append(pos + 1)
            arr = arr[:pos + 1][::-1] + arr[pos + 1:]
            result.append(n)
            arr = arr[:n][::-1] + arr[n:]
            n -= 1
        return result
```
