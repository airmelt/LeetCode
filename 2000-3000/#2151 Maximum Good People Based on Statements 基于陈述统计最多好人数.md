# 2151 Maximum Good People Based on Statements 基于陈述统计最多好人数

__Description:__

There are two types of persons:

- The __good person__: The person who always tells the truth.
- The __bad person__: The person who might tell the truth and might lie.

You are given a __0-indexed__ 2D integer array `statements` of size `n x n` that represents the statements made by `n` people about each other. More specifically, `statements[i][j]` could be one of the following:

- `0` which represents a statement made by person `i` that person `j` is a __bad__ person.
- `1` which represents a statement made by person `i` that person `j` is a __good__ person.
- `2` represents that __no statement__ is made by person `i` about person `j`.

Additionally, no person ever makes a statement about themselves. Formally, we have that `statements[i][i] = 2` for all `0 <= i < n`.

Return _the __maximum__ number of people who can be __good__ based on the statements made by the_ `n` _people_.

__Example:__

Example 1:

![2151-1](https://assets.leetcode.com/uploads/2022/01/15/logic1.jpg)

```text
Input: statements = [[2,1,2],[1,2,2],[2,0,2]]
Output: 2
Explanation: Each person makes a single statement.
```

- Person 0 states that person 1 is good.
- Person 1 states that person 0 is good.
- Person 2 states that person 1 is bad.

Let's take person 2 as the key.

- Assuming that person 2 is a good person:
  - Based on the statement made by person 2, person 1 is a bad person.
  - Now we know for sure that person 1 is bad and person 2 is good.
  - Based on the statement made by person 1, and since person 1 is bad, they could be:
    - telling the truth. There will be a contradiction in this case and this assumption is invalid.
    - lying. In this case, person 0 is also a bad person and lied in their statement.
  - Following that person 2 is a good person, there will be only one good person in the group.
- Assuming that person 2 is a bad person:
  - Based on the statement made by person 2, and since person 2 is bad, they could be:
    - telling the truth. Following this scenario, person 0 and 1 are both bad as explained before.
      - Following that person 2 is bad but told the truth, there will be no good persons in the group.
    - lying. In this case person 1 is a good person.
      - Since person 1 is a good person, person 0 is also a good person.
      - Following that person 2 is bad and lied, there will be two good persons in the group.

We can see that at most 2 persons are good in the best case, so we return 2.
Note that there is more than one way to arrive at this conclusion.

Example 2:

![2151-2](https://assets.leetcode.com/uploads/2022/01/15/logic2.jpg)

```text
Input: statements = [[2,0],[0,2]]
Output: 1
Explanation: Each person makes a single statement.
```

- Person 0 states that person 1 is bad.
- Person 1 states that person 0 is bad.

Let's take person 0 as the key.

- Assuming that person 0 is a good person:
  - Based on the statement made by person 0, person 1 is a bad person and was lying.
  - Following that person 0 is a good person, there will be only one good person in the group.
- Assuming that person 0 is a bad person:
  - Based on the statement made by person 0, and since person 0 is bad, they could be:
    - telling the truth. Following this scenario, person 0 and 1 are both bad.
      - Following that person 0 is bad but told the truth, there will be no good persons in the group.
    - lying. In this case person 1 is a good person.
      - Following that person 0 is bad and lied, there will be only one good person in the group.

We can see that at most, one person is good in the best case, so we return 1.
Note that there is more than one way to arrive at this conclusion.

__Constraints:__

- `n == statements.length == statements[i].length`
- `2 <= n <= 15`
- `statements[i][j]` is either `0`, `1`, or `2`.
- `statements[i][i] == 2`

__题目描述:__

游戏中存在两种角色：

- __好人__:该角色只说真话。
- __坏人__:该角色可能说真话，也可能说假话。

给你一个下标从 __0__ 开始的二维整数数组 `statements` ，大小为 `n x n` ，表示 `n` 个玩家对彼此角色的陈述。具体来说， `statements[i][j]` 可以是下述值之一:

- `0` 表示 `i` 的陈述认为 `j` 是 __坏人__ 。
- `1` 表示 `i` 的陈述认为 `j` 是 __好人__ 。
- `2` 表示 `i` 没有对 `j` 作出陈述。

另外，玩家不会对自己进行陈述。形式上，对所有 `0 <= i < n` ，都有 `statements[i][i] = 2` 。

根据这 `n` 个玩家的陈述，返回可以认为是 __好人__ 的 __最大__ 数目。

__示例:__

示例 1：

![2151-3](https://assets.leetcode.com/uploads/2022/01/15/logic1.jpg)

```text
输入：statements = [[2,1,2],[1,2,2],[2,0,2]]
输出：2
解释：每个人都做一条陈述。
```

- 0 认为 1 是好人。
- 1 认为 0 是好人。
- 2 认为 1 是坏人。

以 2 为突破点。

- 假设 2 是一个好人：
  - 基于 2 的陈述，1 是坏人。
  - 那么可以确认 1 是坏人，2 是好人。
  - 基于 1 的陈述，由于 1 是坏人，那么他在陈述时可能：
    - 说真话。在这种情况下会出现矛盾，所以假设无效。
    - 说假话。在这种情况下，0 也是坏人并且在陈述时说假话。
  - 在认为 2 是好人的情况下，这组玩家中只有一个好人。
- 假设 2 是一个坏人：
  - 基于 2 的陈述，由于 2 是坏人，那么他在陈述时可能：
    - 说真话。在这种情况下，0 和 1 都是坏人。
      - 在认为 2 是坏人但说真话的情况下，这组玩家中没有一个好人。
    - 说假话。在这种情况下，1 是好人。
      - 由于 1 是好人，0 也是好人。
      - 在认为 2 是坏人且说假话的情况下，这组玩家中有两个好人。

在最佳情况下，至多有两个好人，所以返回 2 。
注意，能得到此结论的方法不止一种。

示例 2：

![2151-4](https://assets.leetcode.com/uploads/2022/01/15/logic2.jpg)

```text
输入：statements = [[2,0],[0,2]]
输出：1
解释：每个人都做一条陈述。
```

- 0 认为 1 是坏人。
- 1 认为 0 是坏人。

以 0 为突破点。

- 假设 0 是一个好人：
  - 基于与 0 的陈述，1 是坏人并说假话。
  - 在认为 0 是好人的情况下，这组玩家中只有一个好人。
- 假设 0 是一个坏人：
  - 基于 0 的陈述，由于 0 是坏人，那么他在陈述时可能：
    - 说真话。在这种情况下，0 和 1 都是坏人。
      - 在认为 0 是坏人但说真话的情况下，这组玩家中没有一个好人。
    - 说假话。在这种情况下，1 是好人。
      - 在认为 0 是坏人且说假话的情况下，这组玩家中只有一个好人。

在最佳情况下，至多有一个好人，所以返回 1 。
注意，能得到此结论的方法不止一种。

__提示：__

- `n == statements.length == statements[i].length`
- `2 <= n <= 15`
- `statements[i][j]` 的值为 `0`、 `1` 或 `2`
- `statements[i][i] == 2`

__思路:__

```text
状态压缩
用一个数记录好人的状态, 如果该位为 1 表示是好人, 0 表示是坏人
检查所有情况
如果当前遍历的人是好人
检查他的评论, 如果他评论的人和实际情况不符 
即他说的是假话, 直接返回 0
有两种情况
第一, 他认定的好人实际不是好人, 即 mask 对应位为 0
第二, 他认定的坏人实际不是坏人, 即 mask 对应位为 1
综合可以是 statements[i][j] < 2 and statements[i][j] != ((mask >> j) & 1)
mask 满足评论且 1 的个数最多的即为结果
时间复杂度为 O(2 ^ N * N ^ 2), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int maximumGood(vector<vector<int>> &statements) 
    {
        int result = 0, n = statements.size(), total = 1 << n;
        for (int mask = 1; mask < total; mask++) 
        {
            for (int i = 0; i < n; i++) if ((mask >> i) & 1) for (int j = 0; j < n; j++) if (statements[i][j] < 2 and statements[i][j] != ((mask >> j) & 1)) goto next;
            result = max(result, __builtin_popcount(mask));
            next:;
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int maximumGood(int[][] statements) {
        int result = 0, n = statements.length, total = 1 << n;
        for (int mask = 1; mask < total; mask++) result = Math.max(check(mask, statements), result);
        return result;
    }

    private int check(int mask, int[][] statements) {
        int r = 0, n = statements.length;
        for (int i = 0; i < n; i++) {
            if (((mask >> i) & 1) == 1) {
                for (int j = 0; j < n; j++) {
                    if (statements[i][j] == 2) continue;
                    boolean good = statements[i][j] == 1;
                    if (good && ((mask >> j) & 1) == 0) return 0;
                    if (!good && ((mask >> j) & 1) == 1) return 0;
                }
                ++r;
            }
        }
        return r;
    }
}
```

__Python__:

```Python
class Solution:
    def maximumGood(self, statements: List[List[int]]) -> int:
        return max(0 if any((mask >> i) & 1 and any(v < 2 and v != (mask >> j) & 1 for j, v in enumerate(row)) for i, row in enumerate(statements)) else bin(mask).count('1') for mask in range(1, 1 << len(statements)))
```
