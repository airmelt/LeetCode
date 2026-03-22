# 1535 Find the Winner of an Array Game 找出数组游戏的赢家

__Description:__

Given an integer array `arr` of __distinct__ integers and an integer `k`.

A game will be played between the first two elements of the array (i.e. `arr[0]` and `arr[1]`). In each round of the game, we compare `arr[0]` with `arr[1]`, the larger integer wins and remains at position `0`, and the smaller integer moves to the end of the array. The game ends when an integer wins `k` consecutive rounds.

Return the integer which will win the game.

It is guaranteed that there will be a winner of the game.

__Example:__

Example 1:

```text
Input: arr = [2,1,3,5,4,6,7], k = 2
Output: 5
Explanation: Let's see the rounds of the game:
Round |       arr       | winner | win_count
  1   | [2,1,3,5,4,6,7] | 2      | 1
  2   | [2,3,5,4,6,7,1] | 3      | 1
  3   | [3,5,4,6,7,1,2] | 5      | 1
  4   | [5,4,6,7,1,2,3] | 5      | 2
So we can see that 4 rounds will be played and 5 is the winner because it wins 2 consecutive games.
```

Example 2:

```text
Input: arr = [3,2,1], k = 10
Output: 3
Explanation: 3 will win the first 10 rounds consecutively.
```

__Constraints:__

- `2 <= arr.length <= 10 ^ 5`
- `1 <= arr[i] <= 10 ^ 6`
- `arr` contains __distinct__ integers.
- `1 <= k <= 10 ^ 9`

__题目描述:__

给你一个由 __不同__ 整数组成的整数数组 `arr` 和一个整数 `k` 。

每回合游戏都在数组的前两个元素（即 `arr[0]` 和 `arr[1]` ）之间进行。比较 `arr[0]` 与 `arr[1]` 的大小，较大的整数将会取得这一回合的胜利并保留在位置 `0` ，较小的整数移至数组的末尾。当一个整数赢得 `k` 个连续回合时，游戏结束，该整数就是比赛的 __赢家__ 。

返回赢得比赛的整数。

题目数据 保证 游戏存在赢家。

__示例:__

示例 1：

```text
输入：arr = [2,1,3,5,4,6,7], k = 2
输出：5
解释：一起看一下本场游戏每回合的情况：
```

![1535-1](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/07/30/q-example.png)

```text
因此将进行 4 回合比赛，其中 5 是赢家，因为它连胜 2 回合。
```

示例 2：

```text
输入：arr = [3,2,1], k = 10
输出：3
解释：3 将会在前 10 个回合中连续获胜。
```

示例 3：

```text
输入：arr = [1,9,8,2,3,7,6,4,5], k = 7
输出：9
```

示例 4：

```text
输入：arr = [1,11,22,33,44,55,66,77,88,99], k = 1000000000
输出：99
```

__提示：__

- `2 <= arr.length <= 10 ^ 5`
- `1 <= arr[i] <= 10 ^ 6`
- `arr` 所含的整数 __各不相同__ 。
- `1 <= k <= 10 ^ 9`

__思路:__

```text
模拟
统计每次胜利的元素
不需要将数组重新调整, 只需要比较每次胜利的元素
遍历完成之后如果还没分出胜负, 只要返回数组最大值即可
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int getWinner(vector<int>& arr, int k) 
    {
        int result = arr.front(), count = 0, n = arr.size();
        for (int i = 1; i < n and count < k; i++) 
        {
            if (result > arr[i]) ++count;
            else 
            {
                result = arr[i];
                count = 1;
            }
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int getWinner(int[] arr, int k) {
        int result = arr[0], count = 0, n = arr.length;
        for (int i = 1; i < n && count < k; i++) {
            if (result > arr[i]) ++count;
            else {
                result = arr[i];
                count = 1;
            }
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def getWinner(self, arr: List[int], k: int) -> int:
        result, count, n = arr[0], 0, len(arr)
        for i in range(1, n):
            if count >= k:
                break
            if arr[i] < result:
                count += 1
            else:
                result, count = arr[i], 1
        return result
```
