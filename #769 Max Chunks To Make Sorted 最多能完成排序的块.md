# 769 Max Chunks To Make Sorted 最多能完成排序的块

__Description__:
You are given an integer array arr of length n that represents a permutation of the integers in the range [0, n - 1].

We split arr into some number of chunks (i.e., partitions), and individually sort each chunk. After concatenating them, the result should equal the sorted array.

Return the largest number of chunks we can make to sort the array.

__Example:__

Example 1:

Input: arr = [4,3,2,1,0]
Output: 1
Explanation:
Splitting into two or more chunks will not return the required result.
For example, splitting into [4, 3], [2, 1, 0] will result in [3, 4, 0, 1, 2], which isn't sorted.

Example 2:

Input: arr = [1,0,2,3,4]
Output: 4
Explanation:
We can split into two chunks, such as [1, 0], [2, 3, 4].
However, splitting into [1, 0], [2], [3], [4] is the highest number of chunks possible.

__Constraints:__

n == arr.length
1 <= n <= 10
0 <= arr[i] < n
All the elements of arr are unique.

__题目描述__:
数组arr是[0, 1, ..., arr.length - 1]的一种排列，我们将这个数组分割成几个“块”，并将这些块分别进行排序。之后再连接起来，使得连接的结果和按升序排序后的原数组相同。

我们最多能将数组分成多少块？

__示例 :__

示例 1:

输入: arr = [4,3,2,1,0]
输出: 1
解释:
将数组分成2块或者更多块，都无法得到所需的结果。
例如，分成 [4, 3], [2, 1, 0] 的结果是 [3, 4, 0, 1, 2]，这不是有序的数组。

示例 2:

输入: arr = [1,0,2,3,4]
输出: 4
解释:
我们可以把它分成两块，例如 [1, 0], [2, 3, 4]。
然而，分成 [1, 0], [2], [3], [4] 可以得到最多的块数。

__注意:__

arr 的长度在 [1, 10] 之间。
arr[i]是 [0, 1, ..., arr.length - 1]的一种排列。

__思路__:

贪心
对于 [1, 0, 2, 3, 4] 中 arr[0] = 1, 所以要找到 1 的位置, 这一块包含 0 和 1
对于 arr[2] = 2, 所以可以单独构成一块
即遍历, 同时维持一个最大值, 最大值等于下标的时候就可以分一块出来
时间复杂度为 O(n), 空间复杂度为 O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int maxChunksToSorted(vector<int>& arr) 
    {
        int result = 0, cur = 0, n = arr.size();
        for (int i = 0; i < n; i++)
        {
            cur = max(cur, arr[i]);
            result += cur == i;
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int maxChunksToSorted(int[] arr) {
        int result = 0, cur = 0, n = arr.length;
        for (int i = 0; i < n; i++) {
            cur = Math.max(cur, arr[i]);
            if (cur == i) ++result;
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def maxChunksToSorted(self, arr: List[int]) -> int:
        return sum([max(arr[:i + 1]) < min(arr[i + 1:]) for i in range(len(arr) - 1)]) + 1
```
