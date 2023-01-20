# 1471 The k Strongest Values in an Array 数组中的k个最强值

__Description:__

Given an array of integers  `arr` and an integer  `k`.

A value  `arr[i]` is said to be stronger than a value  `arr[j]` if  `|arr[i] - m| > |arr[j] - m|` where  `m` is the __median__ of the array.

If  `|arr[i] - m| == |arr[j] - m|`, then  `arr[i]` is said to be stronger than  `arr[j]` if  `arr[i] > arr[j]`.

Return _a list of the strongest  `k`_ values in the array. return the answer __in any arbitrary order__.

__Median__ is the middle value in an ordered integer list. More formally, if the length of the list is n, the median is the element in position  `((n - 1) / 2)` in the sorted list __(0-indexed)__.

- For  `arr = [6, -3, 7, 2, 11]`,  `n = 5` and the median is obtained by sorting the array  `arr = [-3, 2, 6, 7, 11]` and the median is  `arr[m]` where  `m = ((5 - 1) / 2) = 2`. The median is  `6`.
- For  `arr = [-7, 22, 17, 3]`,  `n = 4` and the median is obtained by sorting the array  `arr = [-7, 3, 17, 22]` and the median is  `arr[m]` where  `m = ((4 - 1) / 2) = 1`. The median is  `3`.

__Example:__

Example 1:

```text
Input: arr = [1,2,3,4,5], k = 2
Output: [5,1]
Explanation: Median is 3, the elements of the array sorted by the strongest are [5,1,4,2,3]. The strongest 2 elements are [5, 1]. [1, 5] is also accepted answer.
Please note that although |5 - 3| == |1 - 3| but 5 is stronger than 1 because 5 > 1.
```

Example 2:

```text
Input: arr = [1,1,3,5,5], k = 2
Output: [5,5]
Explanation: Median is 3, the elements of the array sorted by the strongest are [5,5,1,1,3]. The strongest 2 elements are [5, 5].
```

Example 3:

```text
Input: arr = [6,7,11,7,6,8], k = 5
Output: [11,8,6,6,7]
Explanation: Median is 7, the elements of the array sorted by the strongest are [11,8,6,6,7,7].
Any permutation of [11,8,6,6,7] is accepted.
```

__Constraints:__

- `1 <= arr.length <= 10 ^ 5`
- `-10 ^ 5 <= arr[i] <= 10 ^ 5`
- `1 <= k <= arr.length`

__题目描述:__

给你一个整数数组  `arr` 和一个整数  `k` 。

设  `m` 为数组的中位数，只要满足下述两个前提之一，就可以判定  `arr[i]` 的值比  `arr[j]` 的值更强：

- `|arr[i] - m| > |arr[j] - m|`
- `|arr[i] - m| == |arr[j] - m|`，且  `arr[i] > arr[j]`

请返回由数组中最强的  `k` 个值组成的列表。答案可以以 __任意顺序__ 返回。

__中位数__ 是一个有序整数列表中处于中间位置的值。形式上，如果列表的长度为  `n` ，那么中位数就是该有序列表（下标从 0 开始）中位于  `((n - 1) / 2)` 的元素。

- 例如  `arr = [6, -3, 7, 2, 11]`， `n = 5`：数组排序后得到  `arr = [-3, 2, 6, 7, 11]` ，数组的中间位置为  `m = ((5 - 1) / 2) = 2` ，中位数  `arr[m]` 的值为  `6` 。
- 例如  `arr = [-7, 22, 17, 3]`， `n = 4`：数组排序后得到  `arr = [-7, 3, 17, 22]` ，数组的中间位置为  `m = ((4 - 1) / 2) = 1` ，中位数  `arr[m]` 的值为  `3` 。

__示例:__

示例 1：

```text
输入：arr = [1,2,3,4,5], k = 2
输出：[5,1]
解释：中位数为 3，按从强到弱顺序排序后，数组变为 [5,1,4,2,3]。最强的两个元素是 [5, 1]。[1, 5] 也是正确答案。
注意，尽管 |5 - 3| == |1 - 3| ，但是 5 比 1 更强，因为 5 > 1 。
```

示例 2：

```text
输入：arr = [1,1,3,5,5], k = 2
输出：[5,5]
解释：中位数为 3, 按从强到弱顺序排序后，数组变为 [5,5,1,1,3]。最强的两个元素是 [5, 5]。
```

示例 3：

```text
输入：arr = [6,7,11,7,6,8], k = 5
输出：[11,8,6,6,7]
解释：中位数为 7, 按从强到弱顺序排序后，数组变为 [11,8,6,6,7,7]。
[11,8,6,6,7] 的任何排列都是正确答案。
```

示例 4：

```text
输入：arr = [6,-3,7,2,11], k = 3
输出：[-3,11,2]
```

示例 5：

```text
输入：arr = [-7,22,17,3], k = 2
输出：[22,17]
```

__提示：__

- `1 <= arr.length <= 10 ^ 5`
- `-10 ^ 5 <= arr[i] <= 10 ^ 5`
- `1 <= k <= arr.length`

__思路:__

```text
排序 ➕ 双指针
先将数组排序, 得到中位数 mid
然后用双指针选择当前更强的数字加入结果中
时间复杂度为 O(NlogN), 空间复杂度为 O(lgN), 主要时间复杂度受排序影响
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<int> getStrongest(vector<int>& arr, int k) 
    {
        sort(arr.begin(), arr.end());
        int n = arr.size(), mid = arr[(n - 1) >> 1];
        sort(arr.begin(), arr.end(), [&](int a, int b) {return abs(a - mid) == abs(b - mid) ? a > b : abs(a - mid) > abs(b - mid);});
        return vector<int>(arr.begin(),arr.begin() + k);
    }
};
```

__Java__:

```Java
class Solution {
    public int[] getStrongest(int[] arr, int k) {
        Arrays.sort(arr);
        int n = arr.length, mid = arr[(n - 1) >> 1], result[] = new int[k], left = 0, right = n - 1;
        for (int i = 0; i < k; i++) {
            if (helper(arr[left], arr[right], mid) > 0) result[i] = arr[left++];
            else result[i] = arr[right--];
        }
        return result;
    }
    
    private int helper(int a, int b, int mid) {
        int x = Math.abs(a - mid), y = Math.abs(b - mid);
        return x == y ? a - b : x - y;
    }
}
```

__Python__:

```Python
class Solution:
    def getStrongest(self, arr: List[int], k: int) -> List[int]:
        arr.sort()
        mid, result, left, right = arr[(n := len(arr)) - 1 >> 1], [0] * k, 0, n - 1
        for i in range(k):
            if (x := abs(arr[left] - mid)) == (y := abs(arr[right] - mid)) and arr[left] > arr[right] or (x > y):
                result[i] = arr[left]
                left += 1
            else:
                result[i] = arr[right]
                right -= 1
        return result
```
