# 2155 All Divisions With the Highest Score of a Binary Array 分组得分最高的所有下标

__Description:__

You are given a __0-indexed__ binary array `nums` of length `n`. `nums` can be divided at index `i` (where `0 <= i <= n)` into two arrays (possibly empty) `numsleft` and `numsright`:

- `numsleft` has all the elements of `nums` between index `0` and `i - 1` __(inclusive)__, while `numsright` has all the elements of nums between index `i` and `n - 1` __(inclusive)__.
- If `i == 0`, `numsleft` is __empty__, while `numsright` has all the elements of `nums`.
- If `i == n`, `numsleft` has all the elements of nums, while `numsright` is __empty__.

The __division score__ of an index `i` is the __sum__ of the number of `0`'s in `numsleft` and the number of `1`'s in `numsright`.

Return all distinct indices that have the highest possible division score. You may return the answer in any order.

__Example:__

Example 1:

```text
Input: nums = [0,0,1,0]
Output: [2,4]
Explanation: Division at index
```

- 0: numsleft is []. numsright is [0,0,1,0]. The score is 0 + 1 = 1.
- 1: numsleft is [0]. numsright is [0,1,0]. The score is 1 + 1 = 2.
- 2: numsleft is [0,0]. numsright is [1,0]. The score is 2 + 1 = 3.
- 3: numsleft is [0,0,1]. numsright is [0]. The score is 2 + 0 = 2.
- 4: numsleft is [0,0,1,0]. numsright is []. The score is 3 + 0 = 3.

Indices 2 and 4 both have the highest possible division score 3.
Note the answer [4,2] would also be accepted.

Example 2:

```text
Input: nums = [0,0,0]
Output: [3]
Explanation: Division at index
```

- 0: numsleft is []. numsright is [0,0,0]. The score is 0 + 0 = 0.
- 1: numsleft is [0]. numsright is [0,0]. The score is 1 + 0 = 1.
- 2: numsleft is [0,0]. numsright is [0]. The score is 2 + 0 = 2.
- 3: numsleft is [0,0,0]. numsright is []. The score is 3 + 0 = 3.

Only index 3 has the highest possible division score 3.

Example 3:

```text
Input: nums = [1,1]
Output: [0]
Explanation: Division at index
```

- 0: numsleft is []. numsright is [1,1]. The score is 0 + 2 = 2.
- 1: numsleft is [1]. numsright is [1]. The score is 0 + 1 = 1.
- 2: numsleft is [1,1]. numsright is []. The score is 0 + 0 = 0.

Only index 0 has the highest possible division score 2.

__Constraints:__

- `n == nums.length`
- `1 <= n <= 10 ^ 5`
- `nums[i]` is either `0` or `1`.

__题目描述:__

给你一个下标从 __0__ 开始的二进制数组 `nums` ，数组长度为 `n` 。 `nums` 可以按下标 `i`（ `0 <= i <= n` ）拆分成两个数组（可能为空）: `numsleft` 和 `numsright` 。

- `numsleft` 包含 `nums` 中从下标 `0` 到 `i - 1` 的所有元素__（包括__ `0` __和__ `i - 1` __）__，而 `numsright` 包含 `nums` 中从下标 `i` 到 `n - 1` 的所有元素__（包括__ `i` __和__ `n - 1` __）。__
- 如果 `i == 0` ， `numsleft` 为 __空__ ，而 `numsright` 将包含 `nums` 中的所有元素。
- 如果 `i == n` ， `numsleft` 将包含 `nums` 中的所有元素，而 `numsright` 为 __空__ 。

下标 `i` 的 __分组得分__ 为 `numsleft` 中 `0` 的个数和 `numsright` 中 `1` 的个数之 __和__ 。

返回 分组得分 最高 的 所有不同下标 。你可以按 任意顺序 返回答案。

__示例:__

示例 1：

```text
输入：nums = [0,0,1,0]
输出：[2,4]
解释：按下标分组
```

- 0 ：numsleft 为 [] 。numsright 为 [0,0,1,0] 。得分为 0 + 1 = 1 。
- 1 ：numsleft 为 [0] 。numsright 为 [0,1,0] 。得分为 1 + 1 = 2 。
- 2 ：numsleft 为 [0,0] 。numsright 为 [1,0] 。得分为 2 + 1 = 3 。
- 3 ：numsleft 为 [0,0,1] 。numsright 为 [0] 。得分为 2 + 0 = 2 。
- 4 ：numsleft 为 [0,0,1,0] 。numsright 为 [] 。得分为 3 + 0 = 3 。

下标 2 和 4 都可以得到最高的分组得分 3 。
注意，答案 [4,2] 也被视为正确答案。

示例 2：

```text
输入：nums = [0,0,0]
输出：[3]
解释：按下标分组
```

- 0 ：numsleft 为 [] 。numsright 为 [0,0,0] 。得分为 0 + 0 = 0 。
- 1 ：numsleft 为 [0] 。numsright 为 [0,0] 。得分为 1 + 0 = 1 。
- 2 ：numsleft 为 [0,0] 。numsright 为 [0] 。得分为 2 + 0 = 2 。
- 3 ：numsleft 为 [0,0,0] 。numsright 为 [] 。得分为 3 + 0 = 3 。

只有下标 3 可以得到最高的分组得分 3 。

示例 3：

```text
输入：nums = [1,1]
输出：[0]
解释：按下标分组
```

- 0 ：numsleft 为 [] 。numsright 为 [1,1] 。得分为 0 + 2 = 2 。
- 1 ：numsleft 为 [1] 。numsright 为 [1] 。得分为 0 + 1 = 1 。
- 2 ：numsleft 为 [1,1] 。numsright 为 [] 。得分为 0 + 0 = 0 。

只有下标 0 可以得到最高的分组得分 2 。

__提示：__

- `n == nums.length`
- `1 <= n <= 10 ^ 5`
- `nums[i]` 为 `0` 或 `1`

__思路:__

```text
前缀和
遍历数组, 统计 0 的个数
遇到 0, 0 的个数加 1
遇到 1, 0 的个数减 1
如果 0 的个数大于最大值, 更新最大值, 清空结果列表
如果 0 的个数等于最大值, 将 i + 1 加入结果列表
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<int> maxScoreIndices(vector<int>& nums) 
    {
        vector<int> result(1, 0);
        int pre = 0, best = 0, n = nums.size();
        for (int i = 0; i < n; i++)
        {
            if (!nums[i])
            {
                ++pre;
                if (pre > best) 
                {
                    best = pre;
                    result.clear();
                }
                if (pre == best) result.emplace_back(i + 1);
            }
            else --pre;
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public List<Integer> maxScoreIndices(int[] nums) {
        List<Integer> result = new ArrayList<>();
        int pre = 0, best = 0, n = nums.length;
        result.add(pre);
        for (int i = 0; i < n; i++) {
            if (nums[i] == 0) {
                ++pre;
                if (pre > best) {
                    best = pre;
                    result.clear();
                }
                if (pre == best) result.add(i + 1);
            } else --pre;
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def maxScoreIndices(self, nums: List[int]) -> List[int]:
        result, pre, best = [0], 0, 0
        for i, num in enumerate(nums):
            if not num:
                pre += 1
                if pre > best:
                    best,result = pre, [i + 1]
                elif pre == best:
                    result.append(i + 1)
            else:
                pre -= 1
        return result
```
