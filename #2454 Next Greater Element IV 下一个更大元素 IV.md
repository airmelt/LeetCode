# 2454 Next Greater Element IV 下一个更大元素 IV

__Description:__

You are given a __0-indexed__ array of non-negative integers `nums`. For each integer in `nums`, you must find its respective __second greater__ integer.

The __second greater__ integer of `nums[i]` is `nums[j]` such that:

- `j > i`
- `nums[j] > nums[i]`
- There exists __exactly one__ index `k` such that `nums[k] > nums[i]` and `i < k < j`.

If there is no such `nums[j]`, the second greater integer is considered to be `-1`.

- For example, in the array `[1, 2, 4, 3]`, the second greater integer of `1` is `4`, `2` is `3`, and that of `3` and `4` is `-1`.

Return _an integer array_ `answer`_, where_ `answer[i]` _is the second greater integer of_ `nums[i]`_._

__Example:__

Example 1:

```text
Input: nums = [2,4,0,9,6]
Output: [9,6,6,-1,-1]
Explanation:
0th index: 4 is the first integer greater than 2, and 9 is the second integer greater than 2, to the right of 2.
1st index: 9 is the first, and 6 is the second integer greater than 4, to the right of 4.
2nd index: 9 is the first, and 6 is the second integer greater than 0, to the right of 0.
3rd index: There is no integer greater than 9 to its right, so the second greater integer is considered to be -1.
4th index: There is no integer greater than 6 to its right, so the second greater integer is considered to be -1.
Thus, we return [9,6,6,-1,-1].
```

Example 2:

```text
Input: nums = [3,3]
Output: [-1,-1]
Explanation:
We return [-1,-1] since neither integer has any integer greater than it.
```

__Constraints:__

- `1 <= nums.length <= 10 ^ 5`
- `0 <= nums[i] <= 10 ^ 9`

__题目描述:__

给你一个下标从 __0__ 开始的非负整数数组 `nums` 。对于 `nums` 中每一个整数，你必须找到对应元素的 __第二大__ 整数。

如果 `nums[j]` 满足以下条件，那么我们称它为 `nums[i]` 的 __第二大__ 整数:

- `j > i`
- `nums[j] > nums[i]`
- 恰好存在 __一个__ `k` 满足 `i < k < j` 且 `nums[k] > nums[i]` 。

如果不存在 `nums[j]` ，那么第二大整数为 `-1` 。

- 比方说，数组 `[1, 2, 4, 3]` 中， `1` 的第二大整数是 `4` ， `2` 的第二大整数是 `3` ， `3` 和 `4` 的第二大整数是 `-1` 。

请你返回一个整数数组 `answer` ，其中 `answer[i]`是 `nums[i]` 的第二大整数。

__示例:__

示例 1：

```text
输入：nums = [2,4,0,9,6]
输出：[9,6,6,-1,-1]
解释：
下标为 0 处：2 的右边，4 是大于 2 的第一个整数，9 是第二个大于 2 的整数。
下标为 1 处：4 的右边，9 是大于 4 的第一个整数，6 是第二个大于 4 的整数。
下标为 2 处：0 的右边，9 是大于 0 的第一个整数，6 是第二个大于 0 的整数。
下标为 3 处：右边不存在大于 9 的整数，所以第二大整数为 -1 。
下标为 4 处：右边不存在大于 6 的整数，所以第二大整数为 -1 。
所以我们返回 [9,6,6,-1,-1] 。
```

示例 2：

```text
输入：nums = [3,3]
输出：[-1,-1]
解释：
由于每个数右边都没有更大的数，所以我们返回 [-1,-1] 。
```

__提示：__

- `1 <= nums.length <= 10 ^ 5`
- `0 <= nums[i] <= 10 ^ 9`

__思路:__

```text
单调栈
如果我们只需要找到第一个更大的数
使用一个递减的单调栈记录下标
当栈不为空且栈顶元素小于当前元素时
弹出栈顶元素即为第一个更大的数
如果我们需要找到第二个更大的数
使用两个栈 s, t
当第一大的数不需要的时候
不将其直接丢弃而是移动到第二大的栈 t 中
那么当第二大的找 t 不为空时
我们就可以直接弹出 t 的栈顶元素作为结果
移动可以使用数组模拟也可以使用第三个栈中转
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<int> secondGreaterElement(vector<int>& nums) 
    {
        vector<int> result(nums.size(), -1), s, t;
        for (int i = 0, n = nums.size(), j = 0; i < n; i++)
        {
            while (!t.empty() and nums[t.back()] < nums[i])
            {
                result[t.back()] = nums[i];
                t.pop_back();
            }
            j = s.size();
            while (j and nums[s[j - 1]] < nums[i]) --j;
            t.insert(t.end(), s.begin() + j, s.end());
            s.resize(j);
            s.emplace_back(i);
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int[] secondGreaterElement(int[] nums) {
        int n = nums.length, result[] = new int[n];
        Arrays.fill(result, -1);
        Stack<Integer> s = new Stack<>(), t = new Stack<>(), v = new Stack<>();
        for (int i = 0, j = 0; i < n; i++) {
            while (!t.isEmpty() && nums[t.peek()] < nums[i]) result[t.pop()] = nums[i];
            while (!s.isEmpty() && nums[s.peek()] < nums[i]) v.push(s.pop());
            while (!v.isEmpty()) t.push(v.pop());
            s.push(i);
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def secondGreaterElement(self, nums: List[int]) -> List[int]:
        result, s, t = [-1] * (n := len(nums)), [], []
        for i, x in enumerate(nums):
            while t and nums[t[-1]] < x:
                result[t.pop()] = x
            j = len(s) - 1
            while j >= 0 and nums[s[j]] < x:
                j -= 1
            t += s[j + 1:]
            del s[j + 1:]
            s.append(i)
        return result
```
