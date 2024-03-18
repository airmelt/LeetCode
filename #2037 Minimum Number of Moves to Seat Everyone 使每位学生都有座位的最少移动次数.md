# 2037 Minimum Number of Moves to Seat Everyone 使每位学生都有座位的最少移动次数

__Description:__

There are `n` seats and `n` students in a room. You are given an array `seats` of length `n`, where `seats[i]` is the position of the `i ^ th` seat. You are also given the array `students` of length `n`, where `students[j]` is the position of the `j ^ th` student.

You may perform the following move any number of times:

- Increase or decrease the position of the `i ^ th` student by `1` (i.e., moving the `i ^ th` student from position `x` to `x + 1` or `x - 1`)

Return the minimum number of moves required to move each student to a seat such that no two students are in the same seat.

Note that there may be multiple seats or students in the same position at the beginning.

__Example:__

Example 1:

```text
Input: seats = [3,1,5], students = [2,7,4]
Output: 4
Explanation: The students are moved as follows:
```

- The first student is moved from from position 2 to position 1 using 1 move.
- The second student is moved from from position 7 to position 5 using 2 moves.
- The third student is moved from from position 4 to position 3 using 1 move.

In total, 1 + 2 + 1 = 4 moves were used.

Example 2:

```text
Input: seats = [4,1,5,9], students = [1,3,2,6]
Output: 7
Explanation: The students are moved as follows:
```

- The first student is not moved.
- The second student is moved from from position 3 to position 4 using 1 move.
- The third student is moved from from position 2 to position 5 using 3 moves.
- The fourth student is moved from from position 6 to position 9 using 3 moves.

In total, 0 + 1 + 3 + 3 = 7 moves were used.

Example 3:

```text
Input: seats = [2,2,6,6], students = [1,3,2,6]
Output: 4
Explanation: Note that there are two seats at position 2 and two seats at position 6.
The students are moved as follows:
```

- The first student is moved from from position 1 to position 2 using 1 move.
- The second student is moved from from position 3 to position 6 using 3 moves.
- The third student is not moved.
- The fourth student is not moved.

In total, 1 + 3 + 0 + 0 = 4 moves were used.

__Constraints:__

- `n == seats.length == students.length`
- `1 <= n <= 100`
- `1 <= seats[i], students[j] <= 100`

__题目描述:__

一个房间里有 `n` 个座位和 `n` 名学生，房间用一个数轴表示。给你一个长度为 `n` 的数组 `seats` ，其中 `seats[i]` 是第 `i` 个座位的位置。同时给你一个长度为 `n` 的数组 `students` ，其中 `students[j]` 是第 `j` 位学生的位置。

你可以执行以下操作任意次：

- 增加或者减少第 `i` 位学生的位置，每次变化量为 `1` （也就是将第 `i` 位学生从位置 `x` 移动到 `x + 1` 或者 `x - 1`）

请你返回使所有学生都有座位坐的 最少移动次数 ，并确保没有两位学生的座位相同。

请注意，初始时有可能有多个座位或者多位学生在 同一 位置。

__示例:__

示例 1：

```text
输入：seats = [3,1,5], students = [2,7,4]
输出：4
解释：学生移动方式如下：
```

- 第一位学生从位置 2 移动到位置 1 ，移动 1 次。
- 第二位学生从位置 7 移动到位置 5 ，移动 2 次。
- 第三位学生从位置 4 移动到位置 3 ，移动 1 次。

总共 1 + 2 + 1 = 4 次移动。

示例 2：

```text
输入：seats = [4,1,5,9], students = [1,3,2,6]
输出：7
解释：学生移动方式如下：
```

- 第一位学生不移动。
- 第二位学生从位置 3 移动到位置 4 ，移动 1 次。
- 第三位学生从位置 2 移动到位置 5 ，移动 3 次。
- 第四位学生从位置 6 移动到位置 9 ，移动 3 次。

总共 0 + 1 + 3 + 3 = 7 次移动。

示例 3：

```text
输入：seats = [2,2,6,6], students = [1,3,2,6]
输出：4
解释：学生移动方式如下：
```

- 第一位学生从位置 1 移动到位置 2 ，移动 1 次。
- 第二位学生从位置 3 移动到位置 6 ，移动 3 次。
- 第三位学生不移动。
- 第四位学生不移动。

总共 1 + 3 + 0 + 0 = 4 次移动。

__提示：__

- `n == seats.length == students.length`
- `1 <= n <= 100`
- `1 <= seats[i], students[j] <= 100`

__思路:__

```text
排序 ➕ 贪心
先将两个数组排序
一一对应放到座位上, 求出每个学生到座位的距离, 求和即可
时间复杂度为 O(NlogN), 空间复杂度为 O(logN)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int minMovesToSeat(vector<int>& seats, vector<int>& students) 
    {
        sort(seats.begin(), seats.end());
        sort(students.begin(), students.end());
        int n = seats.size(), result = 0;
        for (int i = 0; i < n; i++) result += abs(seats[i] - students[i]);
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int minMovesToSeat(int[] seats, int[] students) {
        Arrays.sort(seats);
        Arrays.sort(students);
        int n = seats.length, result = 0;
        for (int i = 0; i < n; i++) result += Math.abs(seats[i] - students[i]);
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def minMovesToSeat(self, seats: List[int], students: List[int]) -> int:
```
