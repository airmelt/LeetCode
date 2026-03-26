# 2951 Find the Peaks 找出峰值

__Description:__

You are given a __0-indexed__ array `mountain`. Your task is to find all the __peaks__ in the `mountain` array.

Return an array that consists of indices of peaks in the given array in any order.

Notes:

- A __peak__ is defined as an element that is __strictly greater__ than its neighboring elements.
- The first and last elements of the array are __not__ a peak.

__Example:__

Example 1:

```text
Input: mountain = [2,4,4]
Output: []
Explanation: mountain[0] and mountain[2] can not be a peak because they are first and last elements of the array.
mountain[1] also can not be a peak because it is not strictly greater than mountain[2].
So the answer is [].
```

Example 2:

```text
Input: mountain = [1,4,3,8,5]
Output: [1,3]
Explanation: mountain[0] and mountain[4] can not be a peak because they are first and last elements of the array.
mountain[2] also can not be a peak because it is not strictly greater than mountain[3] and mountain[1].
But mountain [1] and mountain[3] are strictly greater than their neighboring elements.
So the answer is [1,3].
```

__Constraints:__

- `3 <= mountain.length <= 100`
- `1 <= mountain[i] <= 100`

__题目描述:__

给你一个下标从 __0__ 开始的数组 `mountain` 。你的任务是找出数组 `mountain` 中的所有 __峰值__。

以数组形式返回给定数组中 峰值 的下标，顺序不限 。

注意：

- __峰值__ 是指一个严格大于其相邻元素的元素。
- 数组的第一个和最后一个元素 __不__ 是峰值。

__示例:__

示例 1：

```text
输入：mountain = [2,4,4]
输出：[]
解释：mountain[0] 和 mountain[2] 不可能是峰值，因为它们是数组的第一个和最后一个元素。
mountain[1] 也不可能是峰值，因为它不严格大于 mountain[2] 。
因此，答案为 [] 。
```

示例 2：

```text
输入：mountain = [1,4,3,8,5]
输出：[1,3]
解释：mountain[0] 和 mountain[4] 不可能是峰值，因为它们是数组的第一个和最后一个元素。
mountain[2] 也不可能是峰值，因为它不严格大于 mountain[3] 和 mountain[1] 。
但是 mountain[1] 和 mountain[3] 严格大于它们的相邻元素。
因此，答案是 [1,3] 。
```

__提示：__

- `3 <= mountain.length <= 100`
- `1 <= mountain[i] <= 100`

__思路:__

```text
模拟
遍历 (0, n) 里的所有元素
如果下标 i 对应的值大于两边的值
就加入结果中
这里还可以跳过已经加入的元素的下一个元素（因为不可能满足下一个元素是峰值元素）
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<int> findPeaks(vector<int>& mountain) 
    {
        vector<int> result;
        for (int i = 1, n = mountain.size(); i < n - 1; i++) if (mountain[i] > mountain[i - 1] and mountain[i] > mountain[i + 1]) result.emplace_back(i);
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public List<Integer> findPeaks(int[] mountain) {
        return IntStream.range(1, mountain.length - 1).filter(i -> mountain[i] > mountain[i - 1] && mountain[i] > mountain[i + 1]).boxed().toList();
    }
}
```

__Python__:

```Python
class Solution:
    def findPeaks(self, mountain: List[int]) -> List[int]:
        return [i for i, num in enumerate(mountain[1:-1], 1) if mountain[i - 1] < num > mountain[i + 1]]
```
