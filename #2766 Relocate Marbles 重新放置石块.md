# 2766 Relocate Marbles 重新放置石块

__Description:__

You are given a __0-indexed__ integer array `nums` representing the initial positions of some marbles. You are also given two __0-indexed__ integer arrays `moveFrom` and `moveTo` of __equal__ length.

Throughout `moveFrom.length` steps, you will change the positions of the marbles. On the `i ^ th` step, you will move __all__ marbles at position `moveFrom[i]` to position `moveTo[i]`.

After completing all the steps, return the sorted list of occupied positions.

Notes:

- We call a position __occupied__ if there is at least one marble in that position.
- There may be multiple marbles in a single position.

__Example:__

Example 1:

```text
Input: nums = [1,6,7,8], moveFrom = [1,7,2], moveTo = [2,9,5]
Output: [5,6,8,9]
Explanation: Initially, the marbles are at positions 1,6,7,8.
At the i = 0th step, we move the marbles at position 1 to position 2. Then, positions 2,6,7,8 are occupied.
At the i = 1st step, we move the marbles at position 7 to position 9. Then, positions 2,6,8,9 are occupied.
At the i = 2nd step, we move the marbles at position 2 to position 5. Then, positions 5,6,8,9 are occupied.
At the end, the final positions containing at least one marbles are [5,6,8,9].
```

Example 2:

```text
Input: nums = [1,1,3,3], moveFrom = [1,3], moveTo = [2,2]
Output: [2]
Explanation: Initially, the marbles are at positions [1,1,3,3].
At the i = 0th step, we move all the marbles at position 1 to position 2. Then, the marbles are at positions [2,2,3,3].
At the i = 1st step, we move all the marbles at position 3 to position 2. Then, the marbles are at positions [2,2,2,2].
Since 2 is the only occupied position, we return [2].
```

__Constraints:__

- `1 <= nums.length <= 10 ^ 5`
- `1 <= moveFrom.length <= 10 ^ 5`
- `moveFrom.length == moveTo.length`
- `1 <= nums[i], moveFrom[i], moveTo[i] <= 10 ^ 9`
- The test cases are generated such that there is at least a marble in `moveFrom[i]` at the moment we want to apply the `i ^ th` move.

__题目描述:__

给你一个下标从 __0__ 开始的整数数组 `nums` ，表示一些石块的初始位置。再给你两个长度 __相等__ 下标从 __0__ 开始的整数数组 `moveFrom` 和 `moveTo` 。

在 `moveFrom.length` 次操作内，你将改变石块的位置。在第 `i` 次操作中，你将位置在 `moveFrom[i]` 的所有石块移到位置 `moveTo[i]` 。

完成这些操作后，请你按升序返回所有 有 石块的位置。

注意：

- 如果一个位置至少有一个石块，我们称这个位置 __有__ 石块。
- 一个位置可能会有多个石块。

__示例:__

示例 1：

```text
输入：nums = [1,6,7,8], moveFrom = [1,7,2], moveTo = [2,9,5]
输出：[5,6,8,9]
解释：一开始，石块在位置 1,6,7,8 。
第 i = 0 步操作中，我们将位置 1 处的石块移到位置 2 处，位置 2,6,7,8 有石块。
第 i = 1 步操作中，我们将位置 7 处的石块移到位置 9 处，位置 2,6,8,9 有石块。
第 i = 2 步操作中，我们将位置 2 处的石块移到位置 5 处，位置 5,6,8,9 有石块。
最后，至少有一个石块的位置为 [5,6,8,9] 。
```

示例 2：

```text
输入：nums = [1,1,3,3], moveFrom = [1,3], moveTo = [2,2]
输出：[2]
解释：一开始，石块在位置 [1,1,3,3] 。
第 i = 0 步操作中，我们将位置 1 处的石块移到位置 2 处，有石块的位置为 [2,2,3,3] 。
第 i = 1 步操作中，我们将位置 3 处的石块移到位置 2 处，有石块的位置为 [2,2,2,2] 。
由于 2 是唯一有石块的位置，我们返回 [2] 。
```

__提示：__

- `1 <= nums.length <= 10 ^ 5`
- `1 <= moveFrom.length <= 10 ^ 5`
- `moveFrom.length == moveTo.length`
- `1 <= nums[i], moveFrom[i], moveTo[i] <= 10 ^ 9`
- 测试数据保证在进行第 `i` 步操作时， `moveFrom[i]` 处至少有一个石块。

__思路:__

```text
哈希表
将 nums 转化为哈希表
每次将对应的 moveFrom 元素移除
再将 moveTo 元素加入
最后返回排序好的哈希表即可
时间复杂度为 O(NlogN + M), 空间复杂度为 O(N), 其中 M 为 moveTo 的长度
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<int> relocateMarbles(vector<int>& nums, vector<int>& moveFrom, vector<int>& moveTo) 
    {
        unordered_set<int> s(nums.begin(), nums.end());
        for (int i = 0, n = moveFrom.size(); i < n; i++) 
        {
            s.erase(moveFrom[i]);
            s.insert(moveTo[i]);
        }
        vector<int> result(s.begin(), s.end());
        sort(result.begin(), result.end());
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public List<Integer> relocateMarbles(int[] nums, int[] moveFrom, int[] moveTo) {
        var set = new HashSet<Integer>(nums.length);
        for (int num : nums) set.add(num);
        for (int i = 0, n = moveFrom.length; i < n; i++) {
            set.remove(moveFrom[i]);
            set.add(moveTo[i]);
        }
        var result = new ArrayList<Integer>(set);
        Collections.sort(result);
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def relocateMarbles(self, nums: List[int], moveFrom: List[int], moveTo: List[int]) -> List[int]:
        nums = set(nums)
        for x, y in zip(moveFrom, moveTo):
            nums.remove(x)
            nums.add(y)
        return sorted(nums)
```
