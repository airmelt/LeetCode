# 1640 Check Array Formation Through Concatenation 能否连接形成数组

__Description:__

You are given an array of __distinct__ integers `arr` and an array of integer arrays `pieces`, where the integers in `pieces` are __distinct__. Your goal is to form `arr` by concatenating the arrays in `pieces` __in any order__. However, you are __not__ allowed to reorder the integers in each array `pieces[i]`.

Return `true` _if it is possible_ _to form the array_ `arr` _from_ `pieces`. Otherwise, return `false`.

__Example:__

Example 1:

```text
Input: arr = [15,88], pieces = [[88],[15]]
Output: true
Explanation: Concatenate [15] then [88]
```

Example 2:

```text
Input: arr = [49,18,16], pieces = [[16,18,49]]
Output: false
Explanation: Even though the numbers match, we cannot reorder pieces[0].
```

Example 3:

```text
Input: arr = [91,4,64,78], pieces = [[78],[4,64],[91]]
Output: true
Explanation: Concatenate [91] then [4,64] then [78]
```

__Constraints:__

- `1 <= pieces.length <= arr.length <= 100`
- `sum(pieces[i].length) == arr.length`
- `1 <= pieces[i].length <= arr.length`
- `1 <= arr[i], pieces[i][j] <= 100`
- The integers in `arr` are __distinct__.
- The integers in `pieces` are __distinct__ (i.e., If we flatten pieces in a 1D array, all the integers in this array are distinct).

__题目描述:__

给你一个整数数组 `arr` ，数组中的每个整数 __互不相同__ 。另有一个由整数数组构成的数组 `pieces`，其中的整数也 __互不相同__ 。请你以 __任意顺序__ 连接 `pieces` 中的数组以形成 `arr` 。但是，__不允许__ 对每个数组 `pieces[i]` 中的整数重新排序。

如果可以连接 `pieces` 中的数组形成 `arr` ，返回 `true` ；否则，返回 `false` 。

__示例:__

示例 1：

```text
输入: arr = [15,88], pieces = [[88],[15]]
输出: true
解释: 依次连接 `[15]` 和 `[88]`
```

示例 2：

```text
输入：arr = [49,18,16], pieces = [[16,18,49]]
输出：false
解释：即便数字相符，也不能重新排列 pieces[0]
```

示例 3：

```text
输入: arr = [91,4,64,78], pieces = [[78],[4,64],[91]]
输出: true
解释: 依次连接 `[91]`、 `[4,64]` 和 `[78]`
```

__提示：__

- `1 <= pieces.length <= arr.length <= 100`
- `sum(pieces[i].length) == arr.length`
- `1 <= pieces[i].length <= arr.length`
- `1 <= arr[i], pieces[i][j] <= 100`
- `arr` 中的整数 __互不相同__
- `pieces` 中的整数 __互不相同__（也就是说，如果将 `pieces` 扁平化成一维数组，数组中的所有整数互不相同）

__思路:__

```text
哈希表
注意到数组中的数字互不相同
将 pieces 的每个数组开头元素和对应数组存入哈希表中
检查 arr 数组中每个元素是否都在哈希表中
如果在哈希表中, 检查哈希表对应元素的数组是否按顺序包含所有元素
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    bool canFormArray(vector<int>& arr, vector<vector<int>>& pieces) 
    {
        unordered_map<int, vector<int>> m;
        for (const auto& piece : pieces) m[piece.front()] = piece;
        for (int i = 0, n = arr.size(); i < n;) 
        {
            if (!m.count(arr[i])) return false;
            for (const auto& value : m[arr[i]]) if (arr[i++] != value) return false;
        }
        return true;
    }
};
```

__Java__:

```Java
class Solution {
    public boolean canFormArray(int[] arr, int[][] pieces) {
        Map<Integer, int[]> map = new HashMap<>();
        for (int[] piece : pieces) map.put(piece[0], piece);
        for (int i = 0, n = arr.length; i < n;) {
            if (!map.containsKey(arr[i])) return false;
            for (int value : map.get(arr[i])) if (arr[i++] != value) return false;
        }
        return true;
    }
}
```

__Python__:

```Python
class Solution:
    def canFormArray(self, arr: List[int], pieces: List[List[int]]) -> bool:
        return all([0 for i in pieces if ((j := i[0]) not in arr) or (i != arr[(k := arr.index(j)):k + len(i)])])
```
