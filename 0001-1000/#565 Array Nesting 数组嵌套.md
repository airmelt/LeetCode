# 565 Array Nesting 数组嵌套

__Description__:
A zero-indexed array A of length N contains all integers from 0 to N-1. Find and return the longest length of set S, where S[i] = {A[i], A[A[i]], A[A[A[i]]], ... } subjected to the rule below.

Suppose the first element in S starts with the selection of element A[i] of index = i, the next element in S should be A[A[i]], and then A[A[A[i]]]… By that analogy, we stop adding right before a duplicate element occurs in S.

__Example:__

Example 1:

Input: A = [5,4,0,3,1,6,2]
Output: 4
Explanation:
A[0] = 5, A[1] = 4, A[2] = 0, A[3] = 3, A[4] = 1, A[5] = 6, A[6] = 2.

One of the longest S[K]:
S[0] = {A[0], A[5], A[6], A[2]} = {5, 6, 2, 0}

__Note:__

N is an integer within the range [1, 20,000].
The elements of A are all distinct.
Each element of A is an integer within the range [0, N-1].

__题目描述__:
索引从0开始长度为N的数组A，包含0到N - 1的所有整数。找到最大的集合S并返回其大小，其中 S[i] = {A[i], A[A[i]], A[A[A[i]]], ... }且遵守以下的规则。

假设选择索引为i的元素A[i]为S的第一个元素，S的下一个元素应该是A[A[i]]，之后是A[A[A[i]]]... 以此类推，不断添加直到S出现重复的元素。

__示例 :__

示例 1:

输入: A = [5,4,0,3,1,6,2]
输出: 4
解释:
A[0] = 5, A[1] = 4, A[2] = 0, A[3] = 3, A[4] = 1, A[5] = 6, A[6] = 2.

其中一种最长的 S[K]:
S[0] = {A[0], A[5], A[6], A[2]} = {5, 6, 2, 0}

__提示:__

N是[1, 20,000]之间的整数。
A中不含有重复的元素。
A中的元素大小在[0, N-1]之间。

__思路__:

在数组中找到环的最大长度
使用一个标记数组记录已经访问过的元素
当环的长度大于数组长度的一半可以直接返回
时间复杂度 O(n), 空间复杂度 O(n)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int arrayNesting(vector<int>& nums) 
    {
        int result = 0, n = nums.size();
        vector<bool> visited(n);
        for (int i = 0; i < n; i++) 
        {
            if (result > (n >> 1)) return result;
            if (visited[nums[i]]) continue;
            visited[nums[i]] = 1;
            int temp = 1, cur = nums[nums[i]];
            while (nums[i] != cur) 
            {
                visited[cur] = 1;
                cur = nums[cur];
                ++temp;
            }
            result = max(result, temp);
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int arrayNesting(int[] nums) {
        int result = 0, visited[] = new int[nums.length], n = nums.length;
        for (int i = 0; i < n; i++) {
            if (result > (n >> 1)) return result;
            if (visited[nums[i]] != 0) continue;
            visited[nums[i]] = 1;
            int temp = 1, cur = nums[nums[i]];
            while (nums[i] != cur) {
                visited[cur] = 1;
                cur = nums[cur];
                ++temp;
            }
            result = Math.max(result, temp);
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def arrayNesting(self, nums: List[int]) -> int:
        result, visited = 0, [False] * (n := len(nums))
        for i in range(n):
            if result > (n >> 1):
                return result
            if visited[nums[i]]:
                continue
            visited[nums[i]], temp, cur = True, 1, nums[nums[i]]
            while nums[i] != cur:
                visited[cur], cur = True, nums[cur]
                temp += 1
            result = max(temp, result)
        return result
```
