# 3086 Minimum Moves to Pick K Ones 拾起 K 个 1 需要的最少行动次数

__Description:__

You are given a binary array `nums` of length `n`, a __positive__ integer `k` and a __non-negative__ integer `maxChanges`.

Alice plays a game, where the goal is for Alice to pick up `k` ones from `nums` using the __minimum__ number of __moves__. When the game starts, Alice picks up any index `aliceIndex` in the range [0, n - 1] and stands there. If `nums[aliceIndex] == 1` , Alice picks up the one and `nums[aliceIndex]` becomes `0`(this __does not__ count as a move). After this, Alice can make __any__ number of __moves__ (__including__ __zero__) where in each move Alice must perform __exactly__ one of the following actions:

- Select any index `j != aliceIndex` such that `nums[j] == 0` and set `nums[j] = 1`. This action can be performed __at__ __most__ `maxChanges` times.
- Select any two adjacent indices `x` and `y` ( `|x - y| == 1`) such that `nums[x] == 1`, `nums[y] == 0`, then swap their values (set `nums[y] = 1` and `nums[x] = 0`). If `y == aliceIndex`, Alice picks up the one after this move and `nums[y]` becomes `0`.

Return _the __minimum__ number of moves required by Alice to pick __exactly___ `k` _ones_.

__Example:__

Example 1:

```text
Input: nums = [1,1,0,0,0,1,1,0,0,1], k = 3, maxChanges = 1

Output: 3
```

__Explanation:__ Alice can pick up `3` ones in `3` moves, if Alice performs the following actions in each move when standing at `aliceIndex == 1`:

- At the start of the game Alice picks up the one and `nums[1]` becomes `0`. `nums` becomes [1,__0__,0,0,0,1,1,0,0,1].
- Select `j == 2` and perform an action of the first type. `nums` becomes [1,__0__,1,0,0,1,1,0,0,1]
- Select `x == 2` and `y == 1`, and perform an action of the second type. `nums` becomes [1,__1__,0,0,0,1,1,0,0,1]. As `y == aliceIndex`, Alice picks up the one and `nums` becomes [1,__0__,0,0,0,1,1,0,0,1].
- Select `x == 0` and `y == 1`, and perform an action of the second type. `nums` becomes [0,__1__,0,0,0,1,1,0,0,1]. As `y == aliceIndex`, Alice picks up the one and `nums` becomes [0,__0__,0,0,0,1,1,0,0,1].

Note that it may be possible for Alice to pick up `3` ones using some other sequence of `3` moves.

Example 2:

```text
Input: nums = [0,0,0,0], k = 2, maxChanges = 3

Output: 4
```

__Explanation:__ Alice can pick up `2` ones in `4` moves, if Alice performs the following actions in each move when standing at `aliceIndex == 0`:

- Select `j == 1` and perform an action of the first type. `nums` becomes [__0__,1,0,0].
- Select `x == 1` and `y == 0`, and perform an action of the second type. `nums` becomes [__1__,0,0,0]. As `y == aliceIndex`, Alice picks up the one and `nums` becomes [__0__,0,0,0].
- Select `j == 1` again and perform an action of the first type. `nums` becomes [__0__,1,0,0].
- Select `x == 1` and `y == 0` again, and perform an action of the second type. `nums` becomes [__1__,0,0,0]. As `y == aliceIndex`, Alice picks up the one and `nums` becomes [__0__,0,0,0].

__Constraints:__

- `2 <= n <= 10 ^ 5`
- `0 <= nums[i] <= 1`
- `1 <= k <= 10 ^ 5`
- `0 <= maxChanges <= 10 ^ 5`
- `maxChanges + sum(nums) >= k`

__题目描述:__

给你一个下标从 __0__ 开始的二进制数组 `nums`，其长度为 `n` ；另给你一个 __正整数__ `k` 以及一个 __非负整数__ `maxChanges` 。

Alice 在玩一个游戏，游戏的目标是让 Alice 使用 __最少__ 数量的 __行动__ 次数从 `nums` 中拾起 `k` 个 1 。游戏开始时，Alice 可以选择数组 [0, n - 1] 范围内的任何索引 `aliceIndex` 站立。如果 `nums[aliceIndex] == 1` ，Alice 会拾起一个 1 ，并且 `nums[aliceIndex]` 变成 `0`（这 __不算__ 作一次行动）。之后，Alice 可以执行 __任意数量__ 的 __行动__（__包括__ __零次__），在每次行动中 Alice 必须 __恰好__ 执行以下动作之一:

- 选择任意一个下标 `j != aliceIndex` 且满足 `nums[j] == 0` ，然后将 `nums[j]` 设置为 `1` 。这个动作最多可以执行 `maxChanges` 次。
- 选择任意两个相邻的下标 `x` 和 `y`（ `|x - y| == 1`）且满足 `nums[x] == 1`, `nums[y] == 0` ，然后交换它们的值（将 `nums[y] = 1` 和 `nums[x] = 0`）。如果 `y == aliceIndex`，在这次行动后 Alice 拾起一个 1 ，并且 `nums[y]` 变成 `0` 。

返回 Alice 拾起 __恰好__ `k` 个 1 所需的 __最少__ 行动次数。

__示例:__

示例 1：

```text
输入：nums = [1,1,0,0,0,1,1,0,0,1], k = 3, maxChanges = 1

输出：3
```

__解释:__
如果游戏开始时 Alice 在 `aliceIndex == 1` 的位置上，按照以下步骤执行每个动作，他可以利用 `3` 次行动拾取 `3` 个 1 :

- 游戏开始时 Alice 拾取了一个 1 ， `nums[1]` 变成了 `0`。此时 `nums` 变为 [1,__0__,0,0,0,1,1,0,0,1] 。
- 选择 `j == 2` 并执行第一种类型的动作。 `nums` 变为 [1,__0__,1,0,0,1,1,0,0,1]
- 选择 `x == 2` 和 `y == 1` ，并执行第二种类型的动作。 `nums` 变为 [1,__1__,0,0,0,1,1,0,0,1] 。由于 `y == aliceIndex`，Alice 拾取了一个 1 ， `nums` 变为  [1,__0__,0,0,0,1,1,0,0,1] 。
- 选择 `x == 0` 和 `y == 1` ，并执行第二种类型的动作。 `nums` 变为 [0,__1__,0,0,0,1,1,0,0,1] 。由于 `y == aliceIndex`，Alice 拾取了一个 1 ， `nums` 变为  [0,__0__,0,0,0,1,1,0,0,1] 。

请注意，Alice 也可能执行其他的 `3` 次行动序列达成拾取 `3` 个 1 。

示例 2：

```text
输入：nums = [0,0,0,0], k = 2, maxChanges = 3

输出：4
```

__解释:__
如果游戏开始时 Alice 在 `aliceIndex == 0` 的位置上，按照以下步骤执行每个动作，他可以利用 `4` 次行动拾取 `2` 个 1 :

- 选择 `j == 1` 并执行第一种类型的动作。 `nums` 变为 [__0__,1,0,0] 。
- 选择 `x == 1` 和 `y == 0` ，并执行第二种类型的动作。 `nums` 变为 [__1__,0,0,0] 。由于 `y == aliceIndex`，Alice 拾起了一个 1 ， `nums` 变为 [__0__,0,0,0] 。
- 再次选择 `j == 1` 并执行第一种类型的动作。 `nums` 变为 [__0__,1,0,0] 。
- 再次选择 `x == 1` 和 `y == 0` ，并执行第二种类型的动作。 `nums` 变为 [__1__,0,0,0] 。由于 `y == aliceIndex`，Alice 拾起了一个 1 ， `nums` 变为 [__0__,0,0,0] 。

__提示：__

- `2 <= n <= 10 ^ 5`
- `0 <= nums[i] <= 1`
- `1 <= k <= 10 ^ 5`
- `0 <= maxChanges <= 10 ^ 5`
- `maxChanges + sum(nums) >= k`

__思路:__

```text
贪心
如果 maxChanges 足够多
找到一组长为 3 的全 1 的子数组, 选择正中间的为 aliceIndex
然后按照例 1 的操作可以在 2 * k 步完成所有操作
如果没找到长为 3 的数组取长为 2 的
如果没找到长为 2 的数组取长为 1 的
否则随便选一个
那么取这个长度为 c, 可以先用 max(c - 1, 0) 步将全 1 数组获取 1
然后使用例 1 的方式需要 (k - c) * 2 步
如果 maxChanges 不够多, 那就只能一步步移动 1
其中 k - maxChanges 需要移动完成
记录所有 1 出现的位置 pos
求 pos 的前缀和为 pre
设 [left, right] 中的位置需要移动, [left, right] 总长度为 (k - maxChanges)
那么 right 的范围为 k - maxChanges 到 pos 的长度 n
left = right - (k - maxChanges)
中位数 mid = left + (k - maxChanges) / 2
则移动的距离为 pos[mid] * (left - mid) - pre[mid] + pre[left] + pre[right] - pre[mid] - pos[mid] * (right - mid)
最后再加上 maxChanges * 2
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    long long minimumMoves(vector<int>& nums, int k, int maxChanges) 
    {
        vector<int> pos;
        int c = 0;
        for (int i = 0, m = nums.size(); i < m; i++)
        {
            if (nums[i])
            {
                pos.emplace_back(i);
                c = i > 1 and nums[i - 1] and nums[i - 2] ? 3 : (i and nums[i - 1] ? max(c, 2) : max(c, 1));
            }
        }
        if (maxChanges >= k - (c = min(k, c))) return max(c - 1, 0) + ((k - c) << 1);
        int n = pos.size(), remain = k - maxChanges;
        long long result = LLONG_MAX;
        vector<long long> pre(n + 1);
        for (int i = 0; i < n; i++) pre[i + 1] = pre[i] + pos[i];
        for (int right = remain, left = 0, mid = 0; right <= n; right++) result = min(result, pos[mid = ((left = right - remain) + (remain >> 1))] * (long long)(mid - left) - (pre[mid] - pre[left]) + pre[right] - pre[mid] - pos[mid] * (long long)(right - mid));
        return result + (maxChanges << 1LL);
    }
};
```

__Java__:

```Java
class Solution {
    public long minimumMoves(int[] nums, int k, int maxChanges) {
        var pos = new ArrayList<Integer>();
        int m = nums.length, c = 0;
        for (int i = 0; i < m; i++) {
            if (nums[i] == 1) {
                pos.add(i);
                c = i > 1 && nums[i - 2] == 1 && nums[i - 1] == 1 ? 3 : (i > 0 && nums[i - 1] == 1 ? Math.max(c, 2) : Math.max(c, 1));
            }
        }
        if (maxChanges >= k - (c = Math.min(k, c))) return Math.max(c - 1, 0) + ((k - c) << 1);
        int n = pos.size(), remain = k - maxChanges;
        long result = Long.MAX_VALUE, pre[] = new long[n + 1];
        for (int i = 0; i < n; i++) pre[i + 1] = pre[i] + pos.get(i);
        for (int right = remain, mid = 0, left = 0; right <= n; right++) result = Math.min(pos.get(mid = (left = right - remain) + (remain >>> 1)) * (long)(mid - left) - (pre[mid] - pre[left]) + pre[right] - pre[mid] - pos.get(mid) * (long)(right - mid), result);
        return result + (maxChanges << 1);
    }
}
```

__Python__:

```Python
class Solution:
    def minimumMoves(self, nums: List[int], k: int, maxChanges: int) -> int:
        pos, c = [], 0
        for i, num in enumerate(nums):
            if num:
                pos.append(i)
                c = 3 if i > 1 and nums[i - 2] and nums[i - 1] else max(c, 2) if i and nums[i - 1] else max(c, 1)
        if maxChanges >= k - (c := min(k, c)):
            return max(c - 1, 0) + ((k - c) << 1)
        n, pre, remain = len(pos), list(accumulate(pos, initial=0)), k - maxChanges
        return min(pos[mid := (left := right - remain) + (remain >> 1)] * (mid - left) - (pre[mid] - pre[left]) + pre[right] - pre[mid] - pos[mid] * (right - mid) for right in range(remain, n + 1)) + (maxChanges << 1)
```
