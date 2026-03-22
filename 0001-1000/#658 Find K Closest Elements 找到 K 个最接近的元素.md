# 658 Find K Closest Elements 找到 K 个最接近的元素

__Description__:
Given a sorted integer array arr, two integers k and x, return the k closest integers to x in the array. The result should also be sorted in ascending order.

An integer a is closer to x than an integer b if:

|a - x| < |b - x|, or
|a - x| == |b - x| and a < b

__Example:__

Example 1:

Input: arr = [1,2,3,4,5], k = 4, x = 3
Output: [1,2,3,4]

Example 2:

Input: arr = [1,2,3,4,5], k = 4, x = -1
Output: [1,2,3,4]

__Constraints:__

1 <= k <= arr.length
1 <= arr.length <= 10^4
arr is sorted in ascending order.
-10^4 <= arr[i], x <= 10^4

__题目描述__:
给定一个排序好的数组 arr ，两个整数 k 和 x ，从数组中找到最靠近 x（两数之差最小）的 k 个数。返回的结果必须要是按升序排好的。

整数 a 比整数 b 更接近 x 需要满足：

|a - x| < |b - x| 或者
|a - x| == |b - x| 且 a < b

__示例 :__

示例 1：

输入：arr = [1,2,3,4,5], k = 4, x = 3
输出：[1,2,3,4]

示例 2：

输入：arr = [1,2,3,4,5], k = 4, x = -1
输出：[1,2,3,4]

__提示:__

1 <= k <= arr.length
1 <= arr.length <= 10^4
数组里的每个元素与 x 的绝对值不超过 10^4

__思路__:

二分查找
由题意可知需要找到 [left, left + k) 内的 k 个元素
由于数组有序转化为查找左边界
可以画出 [mid, mid + k) 与 x 的位置关系判断边界的移动
x - arr[mid] > arr[mid + k] 时, left = mid - 1
其他情况 right = mid
时间复杂度 O(lgn + k), 空间复杂度 O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    vector<int> findClosestElements(vector<int>& arr, int k, int x)
    {
        int left = 0, right = arr.size() - 1 - k;
        while (left <= right)
        {
            int mid = left + ((right - left) >> 1);
            if (x - arr[mid] > arr[mid + k] - x) left = mid + 1;
            else right = mid - 1;
        }
        return vector<int>(arr.begin() + left, arr.begin() + left + k);
    }
};
```

__Java__:

```Java
class Solution {
    public List<Integer> findClosestElements(int[] arr, int k, int x) {
        int left = 0, right = arr.length - k;
        while (left < right) {
            int mid = left + ((right - left) >>> 1);
            if (x - arr[mid] > arr[mid + k] - x) left = mid + 1;
            else right = mid;
        }
        return Arrays.stream(Arrays.copyOfRange(arr, left, k + left)).boxed().collect(Collectors.toList());
    }
}
```

__Python__:

```Python
class Solution:
    def findClosestElements(self, arr: List[int], k: int, x: int) -> List[int]:
        return sorted(sorted(arr, key=lambda a: abs(a - x))[:k])
```
