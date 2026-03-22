# 2170 Minimum Operations to Make the Array Alternating 使数组变成交替数组的最少操作数

__Description:__

You are given a __0-indexed__ array `nums` consisting of `n` positive integers.

The array `nums` is called __alternating__ if:

- `nums[i - 2] == nums[i]`, where `2 <= i <= n - 1`.
- `nums[i - 1] != nums[i]`, where `1 <= i <= n - 1`.

In one __operation__, you can choose an index `i` and __change__ `nums[i]` into __any__ positive integer.

Return the minimum number of operations required to make the array alternating.

__Example:__

Example 1:

```text
Input: nums = [3,1,3,2,4,3]
Output: 3
Explanation:
One way to make the array alternating is by converting it to [3,1,3,1,3,1].
The number of operations required in this case is 3.
It can be proven that it is not possible to make the array alternating in less than 3 operations.
```

Example 2:

```text
Input: nums = [1,2,2,2,2]
Output: 2
Explanation:
One way to make the array alternating is by converting it to [1,2,1,2,1].
The number of operations required in this case is 2.
Note that the array cannot be converted to [2,2,2,2,2] because in this case nums[0] == nums[1] which violates the conditions of an alternating array.
```

__Constraints:__

- `1 <= nums.length <= 10 ^ 5`
- `1 <= nums[i] <= 10 ^ 5`

__题目描述:__

给你一个下标从 __0__ 开始的数组 `nums` ，该数组由 `n` 个正整数组成。

如果满足下述条件，则数组 `nums` 是一个 __交替数组__ :

- `nums[i - 2] == nums[i]` ，其中 `2 <= i <= n - 1` 。
- `nums[i - 1] != nums[i]` ，其中 `1 <= i <= n - 1` 。

在一步 __操作__ 中，你可以选择下标 `i` 并将 `nums[i]` __更改__ 为 __任一__ 正整数。

返回使数组变成交替数组的 最少操作数 。

__示例:__

示例 1：

```text
输入：nums = [3,1,3,2,4,3]
输出：3
解释：
使数组变成交替数组的方法之一是将该数组转换为 [3,1,3,1,3,1] 。
在这种情况下，操作数为 3 。
可以证明，操作数少于 3 的情况下，无法使数组变成交替数组。
```

示例 2：

```text
输入：nums = [1,2,2,2,2]
输出：2
解释：
使数组变成交替数组的方法之一是将该数组转换为 [1,2,1,2,1].
在这种情况下，操作数为 2 。
注意，数组不能转换成 [2,2,2,2,2] 。因为在这种情况下，nums[0] == nums[1]，不满足交替数组的条件。
```

__提示：__

- `1 <= nums.length <= 10 ^ 5`
- `1 <= nums[i] <= 10 ^ 5`

__思路:__

```text
贪心
按照奇偶下标分组找到最多的和次多的
如果奇偶的最多的不同, 那么答案就是 n - 最多的之和
否则选择最多的和次多的组合, 答案就是 n - max(最多的之和 + 次多的之和)
其中 n 为数组长度
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int minimumOperations(vector<int>& nums) 
    {
        int n = nums.size();
        auto best_second = [&](int start) -> tuple<int, int, int, int> {
            unordered_map<int, int> freq;
            for (int i = start; i < n; i += 2) ++freq[nums[i]];
            int fkey = 0, fval = 0, skey = 0, sval = 0;
            for (const auto& [key, val]: freq) 
            {
                if (val > fval) 
                {
                    tie(skey, sval) = tuple{fkey, fval};
                    tie(fkey, fval) = tuple{key, val};
                }
                else if (val > sval) tie(skey, sval) = tuple{key, val};
            }

            return { fkey, fval, skey, sval };
        };

        auto [e1key, e1val, e2key, e2val] = best_second(0);
        auto [o1key, o1val, o2key, o2val] = best_second(1);
        return e1key != o1key ? n - e1val - o1val : n - max(e1val + o2val, e2val + o1val);
    }
};
```

__Java__:

```Java
class Solution {
    public int minimumOperations(int[] nums) {
        int odd[] = bestSecond(1, nums), even[] = bestSecond(0, nums), n = nums.length;
        return odd[0] != even[0] ? n - odd[1] - even[1] : n - Math.max(odd[1] + even[3], odd[3] + even[1]);
    }

    private int[] bestSecond(int start, int[] nums) {
        Map<Integer, Integer> map = new HashMap<>();
        for (int i = start, n = nums.length; i < n; i += 2) map.merge(nums[i], 1, Integer::sum);
        int fkey = 0, fval = 0, skey = 0, sval = 0;
        for (Map.Entry<Integer, Integer> entry : map.entrySet()) {
            if (entry.getValue() > fval) {
                skey = fkey;
                sval = fval;
                fkey = entry.getKey();
                fval = entry.getValue();
            } else if (entry.getValue() > sval) {
                skey = entry.getKey();
                sval = entry.getValue();
            }
        }
        return new int[]{ fkey, fval, skey, sval };
    }
}
```

__Python__:

```Python
class Solution:
    def minimumOperations(self, nums: List[int]) -> int:
        def best_second(start: int) -> Tuple[int, int, int, int]:
            return (0, 0, 0, 0) if not (best := Counter(nums[start::2]).most_common(2)) else (*best[0], 0, 0) if len(best) == 1 else (*best[0], *best[1])
        (e1key, e1val, e2key, e2val), (o1key, o1val, o2key, o2val) = best_second(0), best_second(1)
        return len(nums) - (e1val + o1val) if e1key != o1key else len(nums) - max(e1val + o2val, o1val + e2val)
```
