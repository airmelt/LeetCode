# 1560 Most Visited Sector in  a Circular Track 圆形赛道上经过次数最多的扇区

__Description:__

Given an integer `n` and an integer array `rounds`. We have a circular track which consists of `n` sectors labeled from `1` to `n`. A marathon will be held on this track, the marathon consists of `m` rounds. The `i ^ th` round starts at sector `rounds[i - 1]` and ends at sector `rounds[i]`. For example, round 1 starts at sector `rounds[0]` and ends at sector `rounds[1]`

Return an array of the most visited sectors sorted in ascending order.

Notice that you circulate the track in ascending order of sector numbers in the counter-clockwise direction (See the first example).

__Example:__

Example 1:

![1560-1](https://assets.leetcode.com/uploads/2020/08/14/tmp.jpg)

```text
Input: n = 4, rounds = [1,3,1,2]
Output: [1,2]
Explanation: The marathon starts at sector 1. The order of the visited sectors is as follows:
1 --> 2 --> 3 (end of round 1) --> 4 --> 1 (end of round 2) --> 2 (end of round 3 and the marathon)
We can see that both sectors 1 and 2 are visited twice and they are the most visited sectors. Sectors 3 and 4 are visited only once.
```

Example 2:

```text
Input: n = 2, rounds = [2,1,2,1,2,1,2,1,2]
Output: [2]
```

Example 3:

```text
Input: n = 7, rounds = [1,3,5,7]
Output: [1,2,3,4,5,6,7]
```

__Constraints:__

- `2 <= n <= 100`
- `1 <= m <= 100`
- `rounds.length == m + 1`
- `1 <= rounds[i] <= n`
- `rounds[i] != rounds[i + 1]` for `0 <= i < m`

__题目描述:__

给你一个整数 `n` 和一个整数数组 `rounds` 。有一条圆形赛道由 `n` 个扇区组成，扇区编号从 `1` 到 `n` 。现将在这条赛道上举办一场马拉松比赛，该马拉松全程由 `m` 个阶段组成。其中，第 `i` 个阶段将会从扇区 `rounds[i - 1]` 开始，到扇区 `rounds[i]` 结束。举例来说，第 `1` 阶段从 `rounds[0]` 开始，到 `rounds[1]` 结束。

请你以数组形式返回经过次数最多的那几个扇区，按扇区编号 升序 排列。

__示例:__

注意，赛道按扇区编号升序逆时针形成一个圆（请参见第一个示例）。

示例 1：

![1560-2](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/08/22/3rd45e.jpg)

```text
输入：n = 4, rounds = [1,3,1,2]
输出：[1,2]
解释：本场马拉松比赛从扇区 1 开始。经过各个扇区的次序如下所示：
1 --> 2 --> 3（阶段 1 结束）--> 4 --> 1（阶段 2 结束）--> 2（阶段 3 结束，即本场马拉松结束）
其中，扇区 1 和 2 都经过了两次，它们是经过次数最多的两个扇区。扇区 3 和 4 都只经过了一次。
```

示例 2：

```text
输入：n = 2, rounds = [2,1,2,1,2,1,2,1,2]
输出：[2]
```

示例 3：

```text
输入：n = 7, rounds = [1,3,5,7]
输出：[1,2,3,4,5,6,7]
```

__提示：__

- `2 <= n <= 100`
- `1 <= m <= 100`
- `rounds.length == m + 1`
- `1 <= rounds[i] <= n`
- `rounds[i] != rounds[i + 1]` ，其中 `0 <= i < m`

__思路:__

```text
贪心
中间过程不影响最后的结果
只用比较开始的一轮和最后的一轮
如果数组的第一个元素比最后一个元素小, 那么从第一个元素开始到最后一个元素都要加进结果
否则, 其实相当于反向跑了一圈, 将数字反向加入即可
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<int> mostVisited(int n, vector<int>& rounds) 
    {
        vector<int> result;
        if (rounds.front() > rounds.back()) 
        {
            for (int i = 1; i <= rounds.back(); i++) result.emplace_back(i);
            for (int i = rounds.front(); i <= n; i++) result.emplace_back(i);
        } 
        else for (int i = rounds.front(); i <= rounds.back(); i++) result.emplace_back(i);
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public List<Integer> mostVisited(int n, int[] rounds) {
        List<Integer> result = new ArrayList<>();
        if (rounds[0] > rounds[rounds.length - 1]) {
            for (int i = 1; i <= rounds[rounds.length - 1]; i++) result.add(i);
            for (int i = rounds[0]; i <= n; i++) result.add(i);
        } else for (int i = rounds[0]; i <= rounds[rounds.length - 1]; i++) result.add(i);
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def mostVisited(self, n: int, rounds: List[int]) -> List[int]:
        return list(range(rounds[0], rounds[-1] + 1)) or list(range(1, rounds[-1] + 1)) + list(range(rounds[0], n + 1))
```
