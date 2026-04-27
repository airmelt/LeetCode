# 1713 Minimum Operations to Make a Subsequence 得到子序列的最少操作次数

__Description:__

You are given an array `target` that consists of __distinct__ integers and another integer array `arr` that __can__ have duplicates.

In one operation, you can insert any integer at any position in `arr`. For example, if `arr = [1,4,1,2]`, you can add `3` in the middle and make it `[1,4,3,1,2]`. Note that you can insert the integer at the very beginning or end of the array.

Return _the __minimum__ number of operations needed to make_ `target` _a __subsequence__ of_ `arr`_._

A __subsequence__ of an array is a new array generated from the original array by deleting some elements (possibly none) without changing the remaining elements' relative order. For example, `[2,7,4]` is a subsequence of `[4,2,3,7,2,1,4]` (the underlined elements), while `[2,4,2]` is not.

__Example:__

Example 1:

```text
Input:  target = [5,1,3], `arr` = [9,4,2,3,4]
Output:  2
Explanation:  You can add 5 and 1 in such a way that makes `arr` = [5,9,4,1,2,3,4], then target will be a subsequence of `arr`.
```

Example 2:

```text
Input:  target = [6,4,8,1,3,2], `arr` = [4,7,6,2,3,8,6,1]
Output:  3
```

__Constraints:__

- `1 <= target.length, arr.length <= 10 ^ 5`
- `1 <= target[i], arr[i] <= 10 ^ 9`
- `target` contains no duplicates.

__题目描述:__

给你一个数组 `target` ，包含若干 __互不相同__ 的整数，以及另一个整数数组 `arr` ， `arr` __可能__ 包含重复元素。

每一次操作中，你可以在 `arr` 的任意位置插入任一整数。比方说，如果 `arr = [1,4,1,2]` ，那么你可以在中间添加 `3` 得到 [1,4,__3__,1,2] 。你可以在数组最开始或最后面添加整数。

请你返回 __最少__ 操作次数，使得 `target` 成为 `arr` 的一个子序列。

一个数组的 __子序列__ 指的是删除原数组的某些元素（可能一个元素都不删除），同时不改变其余元素的相对顺序得到的数组。比方说， `[2,7,4]` 是 [4,__2__,3,__7__,2,1,__4__] 的子序列（加粗元素），但 `[2,4,2]` 不是子序列。

__示例:__

示例 1：

```text
_输入:_ target = [5,1,3], `arr` = [9,4,2,3,4]
 _输出:_ 2
 _解释:_ 你可以添加 5 和 1 ，使得 arr 变为 [5,9,4,1,2,3,4] ，target 为 arr 的子序列。
```

示例 2：

```text
_输入:_ target = [6,4,8,1,3,2], `arr` = [4,7,6,2,3,8,6,1]
 _输出:_ 3
```

__提示：__

- `1 <= target.length, arr.length <= 10 ^ 5`
- `1 <= target[i], arr[i] <= 10 ^ 9`
- `target` 不包含任何重复元素。

__思路:__

```text
贪心 ➕ 二分查找
题目需要找到 target 数组和 arr 数组的最长公共子序列，然后用 target 数组的长度减去最长公共子序列的长度即为答案
但是朴素的最长公共子序列算法时间复杂度为 O(MN), 会超时
注意到 target 数组中的元素各不相同
记录每个元素出现的位置
然后将 arr 数组中的元素按照 target 数组中的顺序进行映射
这样就将原问题转化为了求 arr 数组中的最长递增子序列的长度
比如 [6, 4, 8, 1, 3, 2] -> {6: 0, 4: 1, 8: 2, 1: 3, 3: 4, 2: 5}
[4, 7, 6, 2, 3, 8, 6, 1] -> [1, 0, 5, 4, 2, 0, 3]
在 target 中不存在的元素可以忽略
实际上只需要求 [1, 0, 5, 4, 2, 0, 3] 数组中的最长递增子序列的长度即可
时间复杂度为 O(N + MlogM), 空间复杂度为 O(N + M)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int minOperations(vector<int>& target, vector<int>& arr) {
        vector<int> record, result;
        for (int i = 0; i < target.size(); i++) pos[target[i]] = i; 
        for (int i = 0; i < arr.size(); i++) if (pos.count(arr[i])) record.push_back(pos[arr[i]]);
        for (int i = 0; i < record.size(); i++)
        {
            if (result.empty() or record[i] > result.back()) result.push_back(record[i]);
            else result[lower_bound(result.begin(), result.end(), record[i]) - result.begin()] = record[i];
        }
        return target.size() - result.size();     
    }
private:
    unordered_map<int, int> pos;
};
```

__Java__:

```Java
class Solution {
    public int minOperations(int[] target, int[] arr) {
        int n = target.length;
        Map<Integer, Integer> pos = new HashMap<Integer, Integer>();
        for (int i = 0; i < n; ++i) pos.put(target[i], i);
        List<Integer> q = new ArrayList<Integer>();
        for (int val : arr) {
            if (pos.containsKey(val)) {
                int it = Collections.binarySearch(q, pos.get(val));
                if (it < 0) it = -it - 1;
                if (it != q.size()) q.set(it, pos.get(val));
                else q.add(pos.get(val));
            }
        }
        return n - q.size();
    }
}
```

__Python__:

```Python
class Solution:
    def minOperations(self, target: List[int], arr: List[int]) -> int:
        q, d = [], {x: i for i, x in enumerate(target)}
        for x in arr:
            if x in d:
                q[(i := bisect_left(q, d[x])):i + 1] = d[x],
        return len(target) - len(q)
```
