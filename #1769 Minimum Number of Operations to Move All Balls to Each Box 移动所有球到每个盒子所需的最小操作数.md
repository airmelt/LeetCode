# 1769 Minimum Number of Operations to Move All Balls to Each Box 移动所有球到每个盒子所需的最小操作数

__Description:__

You have `n` boxes. You are given a binary string `boxes` of length `n`, where `boxes[i]` is `'0'` if the `i ^ th` box is __empty__, and `'1'` if it contains __one__ ball.

In one operation, you can move __one__ ball from a box to an adjacent box. Box `i` is adjacent to box `j` if `abs(i - j) == 1`. Note that after doing so, there may be more than one ball in some boxes.

Return an array `answer` of size `n`, where `answer[i]` is the __minimum__ number of operations needed to move all the balls to the `i ^ th` box.

Each `answer[i]` is calculated considering the __initial__ state of the boxes.

__Example:__

Example 1:

```text
Input: boxes = "110"
Output: [1,1,3]
Explanation: The answer for each box is as follows:
1) First box: you will have to move one ball from the second box to the first box in one operation.
2) Second box: you will have to move one ball from the first box to the second box in one operation.
3) Third box: you will have to move one ball from the first box to the third box in two operations, and move one ball from the second box to the third box in one operation.
```

Example 2:

```text
Input: boxes = "001011"
Output: [11,8,5,4,3,4]
```

__Constraints:__

- `n == boxes.length`
- `1 <= n <= 2000`
- `boxes[i]` is either `'0'` or `'1'`.

__题目描述:__

有 `n` 个盒子。给你一个长度为 `n` 的二进制字符串 `boxes` ，其中 `boxes[i]` 的值为 `'0'` 表示第 `i` 个盒子是 __空__ 的，而 `boxes[i]` 的值为 `'1'` 表示盒子里有 __一个__ 小球。

在一步操作中，你可以将 __一个__ 小球从某个盒子移动到一个与之相邻的盒子中。第 `i` 个盒子和第 `j` 个盒子相邻需满足 `abs(i - j) == 1` 。注意，操作执行后，某些盒子中可能会存在不止一个小球。

返回一个长度为 `n` 的数组 `answer` ，其中 `answer[i]` 是将所有小球移动到第 `i` 个盒子所需的 __最小__ 操作数。

每个 `answer[i]` 都需要根据盒子的 __初始状态__ 进行计算。

__示例:__

示例 1：

```text
输入：boxes = "110"
输出：[1,1,3]
解释：每个盒子对应的最小操作数如下：
1) 第 1 个盒子：将一个小球从第 2 个盒子移动到第 1 个盒子，需要 1 步操作。
2) 第 2 个盒子：将一个小球从第 1 个盒子移动到第 2 个盒子，需要 1 步操作。
3) 第 3 个盒子：将一个小球从第 1 个盒子移动到第 3 个盒子，需要 2 步操作。将一个小球从第 2 个盒子移动到第 3 个盒子，需要 1 步操作。共计 3 步操作。
```

示例 2：

```text
输入：boxes = "001011"
输出：[11,8,5,4,3,4]
```

__提示：__

- `n == boxes.length`
- `1 <= n <= 2000`
- `boxes[i]` 为 `'0'` 或 `'1'`

__思路:__

```text
1. 暴力法
将每个盒子遍历一遍
计算所有小球到盒子的距离之和
时间复杂度为 O(N ^ 2), 空间复杂度为 O(1)
2. 前缀和
计算每个盒子的左边小球数量和右边小球数量
初始 pre 就为 0 号盒子右边的小球下标之和, 遍历一次数组可以得到
再遍历一次数组操作数可以由 pre + left - right 得到
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<int> minOperations(string boxes) 
    {
        int left = boxes.front() - '0', right = 0, pre = 0, n = boxes.size();
        vector<int> result(n);
        for (int i = 1; i < n; i++) 
        {
            if (boxes[i] == '1') 
            {
                ++right;
                pre += i;
            }
        }
        result[0] = pre;
        for (int i = 1; i < n; i++) 
        {
            result[i] = (pre += left - right);
            if (boxes[i] == '1') 
            {
                ++left;
                --right;
            }
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int[] minOperations(String boxes) {
        int left = boxes.charAt(0) - '0', right = 0, pre = 0, n = boxes.length(), result[] = new int[n];
        for (int i = 1; i < n; i++) {
            if (boxes.charAt(i) == '1') {
                ++right;
                pre += i;
            }
        }
        result[0] = pre;
        for (int i = 1; i < n; i++) {
            result[i] = (pre += left - right);
            if (boxes.charAt(i) == '1') {
                ++left;
                --right;
            }
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def minOperations(self, boxes: str) -> List[int]:
        return [sum([abs(i - j) for j in range(len(boxes)) if boxes[j] == '1']) for i in range(len(boxes))]
```
