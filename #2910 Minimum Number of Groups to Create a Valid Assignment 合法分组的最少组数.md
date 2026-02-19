# 2910 Minimum Number of Groups to Create a Valid Assignment 合法分组的最少组数

__Description:__

You are given a collection of numbered `balls` and instructed to sort them into boxes for a nearly balanced distribution. There are two rules you must follow:

- Balls with the same box must have the same value. But, if you have more than one ball with the same number, you can put them in different boxes.
- The biggest box can only have one more ball than the smallest box.

Return the fewest number of boxes to sort these balls following these rules.

__Example:__

Example 1:

```text
Input: balls = [3,2,3,2,3]

Output: 2

Explanation:
```

We can sort `balls` into boxes as follows:

- `[3,3,3]`
- `[2,2]`

The size difference between the two boxes doesn't exceed one.

Example 2:

```text
Input: balls = [10,10,10,3,1,1]

Output: 4

Explanation:
```

We can sort `balls` into boxes as follows:

- `[10]`
- `[10,10]`
- `[3]`
- `[1,1]`

You can't use fewer than four boxes while still following the rules. For example, putting all three balls numbered 10 in one box would break the rule about the maximum size difference between boxes.

__Constraints:__

- `1 <= nums.length <= 10 ^ 5`
- `1 <= nums[i] <= 10 ^ 9`

__题目描述:__

给你一组带编号的 `balls` 并要求将它们分类到盒子里，以便均衡地分配。你必须遵守两条规则:

- 同一个盒子里的球必须具有相同的编号。但是，如果你有多个相同编号的球，你可以把它们放在不同的盒子里。
- 最大的盒子只能比最小的盒子多一个球。

返回遵循上述规则排列这些球所需要的盒子的最小数目。

__示例:__

示例 1：

```text
输入：balls = [3,2,3,2,3]
输出：2
解释：一个得到 2 个分组的方案如下，中括号内的数字都是下标：
我们可以如下排列 balls 到盒子里：
```

- [3,3,3]
- [2,2]

两个盒子之间的大小差没有超过 1。

示例 2：

```text
输入：balls = [10,10,10,3,1,1]
输出：4
解释：我们可以如下排列 balls 到盒子里：
```

- [10]
- [10,10]
- [3]
- [1,1]

无法得到一个遵循上述规则且小于 4 盒的答案。例如，把所有三个编号为 10 的球都放在一个盒子里，就会打破盒子之间最大尺寸差异的规则。

__提示：__

- `1 <= balls.length <= 10 ^ 5`
- `1 <= balls[i] <= 10 ^ 9`

__思路:__

```text
数学
盒子容量最多只能比数字出现次数最少的次数多 1
若将每个数字的出现次数记录在哈希表 m 中
如果能分成 k + 1 的大小的组
那么最多能分成 (m[ball] + k) / (k + 1) 个组
记录每个数的出现次数 v 和 k 的除数 q 及余数 r
如果 r <= q
那么可以将 r 个塞到 q 个组中
倒着遍历只要能分组立即返回
最多可以将数组一个个分
所以总能分组
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int minGroupsForValidAssignment(vector<int>& balls) 
    {
        unordered_map<int, int> m;
        for (const auto& ball : balls) ++m[ball];
        for (int k = min_element(m.begin(), m.end(), [](const auto& a, const auto& b) { return a.second < b.second; }) -> second, result = 0; ; k--)
        {
            for (const auto &[_, v] : m) 
            {
                if (v / k < v % k) 
                {
                    result = 0;
                    break;
                }
                result += (v + k) / (k + 1);
            }
            if (result) return result;
        }
    }
};
```

__Java__:

```Java
class Solution {
    public int minGroupsForValidAssignment(int[] balls) {
        var map = new HashMap<Integer, Integer>();
        for (int ball : balls) map.merge(ball, 1, Integer::sum);
        for (int k = map.values().stream().mapToInt(v -> v).min().getAsInt(), result = 0; ; k--) {
            for (int v : map.values()) {
                if (v / k < v % k) {
                    result = 0;
                    break;
                }
                result += (v + k) / (k + 1);
            }
            if (result > 0) return result;
        }
    }
}
```

__Python__:

```Python
class Solution:
    def minGroupsForValidAssignment(self, nums: List[int]) -> int:
        c = Counter(nums)
        for k in range(min(c.values()), 0, -1):
            result = 0
            for v in c.values():
                q, r = divmod(v, k)
                if q < r:
                    break
                result += (v + k) // (k + 1)
            else:
                return result
```
