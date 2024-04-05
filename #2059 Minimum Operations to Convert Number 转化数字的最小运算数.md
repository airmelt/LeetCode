# 2059 Minimum Operations to Convert Number 转化数字的最小运算数

__Description:__

You are given a __0-indexed__ integer array `nums` containing __distinct__ numbers, an integer `start`, and an integer `goal`. There is an integer `x` that is initially set to `start`, and you want to perform operations on `x` such that it is converted to `goal`. You can perform the following operation repeatedly on the number `x`:

If `0 <= x <= 1000`, then for any index `i` in the array ( `0 <= i < nums.length`), you can set `x` to any of the following:

- `x + nums[i]`
- `x - nums[i]`
- `x ^ nums[i]` (bitwise-XOR)

Note that you can use each `nums[i]` any number of times in any order. Operations that set `x` to be out of the range `0 <= x <= 1000` are valid, but no more operations can be done afterward.

Return _the __minimum__ number of operations needed to convert_ `x = start` _into_ `goal`_, and_ `-1` _if it is not possible_.

__Example:__

Example 1:

```text
Input: nums = [2,4,12], start = 2, goal = 12
Output: 2
Explanation: We can go from 2 → 14 → 12 with the following 2 operations.
```

- 2 + 12 = 14
- 14 - 2 = 12

Example 2:

```text
Input: nums = [3,5,7], start = 0, goal = -4
Output: 2
Explanation: We can go from 0 → 3 → -4 with the following 2 operations. 
```

- 0 + 3 = 3
- 3 - 7 = -4

Note that the last operation sets x out of the range 0 <= x <= 1000, which is valid.

Example 3:

```text
Input: nums = [2,8,16], start = 0, goal = 1
Output: -1
Explanation: There is no way to convert 0 into 1.
```

__Constraints:__

- `1 <= nums.length <= 1000`
- `-10 ^ 9 <= nums[i], goal <= 10 ^ 9`
- `0 <= start <= 1000`
- `start != goal`
- All the integers in `nums` are distinct.

__题目描述:__

给你一个下标从 __0__ 开始的整数数组 `nums` ，该数组由 __互不相同__ 的数字组成。另给你两个整数 `start` 和 `goal` 。

整数 `x` 的值最开始设为 `start` ，你打算执行一些运算使 `x` 转化为 `goal` 。你可以对数字 `x` 重复执行下述运算:

如果 `0 <= x <= 1000` ，那么，对于数组中的任一下标 `i`（ `0 <= i < nums.length`），可以将 `x` 设为下述任一值:

- `x + nums[i]`
- `x - nums[i]`
- `x ^ nums[i]`（按位异或 XOR）

注意，你可以按任意顺序使用每个 `nums[i]` 任意次。使 `x` 越过 `0 <= x <= 1000` 范围的运算同样可以生效，但该该运算执行后将不能执行其他运算。

返回将 `x = start` 转化为 `goal` 的最小操作数；如果无法完成转化，则返回 `-1` 。

__示例:__

示例 1：

```text
输入：nums = [2,4,12], start = 2, goal = 12
输出：2
解释：
可以按 2 → 14 → 12 的转化路径进行，只需执行下述 2 次运算：
```

- 2 + 12 = 14
- 14 - 2 = 12

示例 2：

```text
输入：nums = [3,5,7], start = 0, goal = -4
输出：2
解释：
可以按 0 → 3 → -4 的转化路径进行，只需执行下述 2 次运算：
```

- 0 + 3 = 3
- 3 - 7 = -4

注意，最后一步运算使 x 超过范围 0 <= x <= 1000 ，但该运算仍然可以生效。

示例 3：

```text
输入：nums = [2,8,16], start = 0, goal = 1
输出：-1
解释：
无法将 0 转化为 1
```

__提示：__

- `1 <= nums.length <= 1000`
- `-10 ^ 9 <= nums[i], goal <= 10 ^ 9`
- `0 <= start <= 1000`
- `start != goal`
- `nums` 中的所有整数互不相同

__思路:__

```text
BFS
将 start 加入队列 q 中
用哈希表记录到达 j 的步数 step
遍历 nums 中的每个数 num
对于每个数 num, 计算 cur + num, cur - num, cur ^ num
如果其中有一个等于 goal, 则返回 step + 1
如果其中有一个超出范围 [0, 1000], 或者已经访问过, 则跳过
否则, 将 next 加入队列 q 中, 并记录到达 next 的步数 step + 1
直到队列为空, 返回 -1
时间复杂度为 O(MN), 空间复杂度为 O(M), 其中 M 为计算数值的大小, 本题为 1000, N 为 nums 的长度
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int minimumOperations(vector<int>& nums, int start, int goal) 
    {
        queue<int> q;
        unordered_map<int, int> m;
        q.emplace(start);
        m[start] = 0;
        while (!q.empty()) 
        {
            int cur = q.front(), step = m[cur];
            q.pop();
            for (const auto& num : nums) 
            {
                vector<int> ops{cur + num, cur - num, cur ^ num};
                for (const auto& next : ops) 
                {
                    if (next == goal) return step + 1;
                    if (next < 0 or next > 1000 or m.count(next)) continue;
                    m[next] = step + 1;
                    q.emplace(next);
                }
            }
        }
        return -1;
    }
};
```

__Java__:

```Java
class Solution {
    public int minimumOperations(int[] nums, int start, int goal) {
        Deque<Integer> d = new ArrayDeque<>();
        Map<Integer, Integer> map = new HashMap<>();
        d.addLast(start);
        map.put(start, 0);
        while (!d.isEmpty()) {
            int cur = d.pollFirst(), step = map.get(cur);
            for (int num : nums) {
                int[] result = new int[]{cur + num, cur - num, cur ^ num};
                for (int next : result) 
                {
                    if (next == goal) return step + 1;
                    if (next < 0 || next > 1000 || map.containsKey(next)) continue;
                    map.put(next, step + 1);
                    d.addLast(next);
                }
            }
        }
        return -1;
    }
}
```

__Python__:

```Python
class Solution:
    def minimumOperations(self, nums: List[int], start: int, goal: int) -> int:
        q, m = deque([start]), {start: 0}
        while q:
            step = m[cur := q.popleft()]
            for num in nums:
                for next in (cur + num, cur - num, cur ^ num):
                    if next == goal:
                        return step + 1
                    if next < 0 or next > 1000 or next in m:
                        continue
                    m[next] = step + 1
                    q.append(next)
        return -1
```
