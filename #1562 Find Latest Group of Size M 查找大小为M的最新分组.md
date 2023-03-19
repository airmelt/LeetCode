# 1562 Find Latest Group of Size M 查找大小为M的最新分组

__Description:__

Given an array `arr` that represents a permutation of numbers from `1` to `n`.

You have a binary string of size `n` that initially has all its bits set to zero. At each step `i` (assuming both the binary string and `arr` are 1-indexed) from `1` to `n`, the bit at position `arr[i]` is set to `1`.

You are also given an integer `m`. Find the latest step at which there exists a group of ones of length `m`. A group of ones is a contiguous substring of `1`'s such that it cannot be extended in either direction.

Return _the latest step at which there exists a group of ones of length __exactly___ `m`. _If no such group exists, return_ `-1`.

__Example:__

Example 1:

```text
Input: arr = [3,5,1,2,4], m = 1
Output: 4
Explanation: 
Step 1: "00100", groups: ["1"]
Step 2: "00101", groups: ["1", "1"]
Step 3: "10101", groups: ["1", "1", "1"]
Step 4: "11101", groups: ["111", "1"]
Step 5: "11111", groups: ["11111"]
The latest step at which there exists a group of size 1 is step 4.
```

Example 2:

```text
Input: arr = [3,1,5,4,2], m = 2
Output: -1
Explanation: 
Step 1: "00100", groups: ["1"]
Step 2: "10100", groups: ["1", "1"]
Step 3: "10101", groups: ["1", "1", "1"]
Step 4: "10111", groups: ["1", "111"]
Step 5: "11111", groups: ["11111"]
No group of size 2 exists during any step.
```

__Constraints:__

- `n == arr.length`
- `1 <= m <= n <= 10 ^ 5`
- `1 <= arr[i] <= n`
- All integers in `arr` are __distinct__.

__题目描述:__

给你一个数组 `arr` ，该数组表示一个从 `1` 到 `n` 的数字排列。有一个长度为 `n` 的二进制字符串，该字符串上的所有位最初都设置为 `0` 。

在从 `1` 到 `n` 的每个步骤 `i` 中（假设二进制字符串和 `arr` 都是从 `1` 开始索引的情况下），二进制字符串上位于位置 `arr[i]` 的位将会设为 `1` 。

给你一个整数 `m` ，请你找出二进制字符串上存在长度为 `m` 的一组 `1` 的最后步骤。一组 `1` 是一个连续的、由 `1` 组成的子串，且左右两边不再有可以延伸的 `1` 。

返回存在长度 __恰好__ 为 `m` 的 __一组 `1`__ 的最后步骤。如果不存在这样的步骤，请返回 `-1` 。

__示例:__

示例 1：

```text
输入：arr = [3,5,1,2,4], m = 1
输出：4
解释：
步骤 1："00100"，由 1 构成的组：["1"]
步骤 2："00101"，由 1 构成的组：["1", "1"]
步骤 3："10101"，由 1 构成的组：["1", "1", "1"]
步骤 4："11101"，由 1 构成的组：["111", "1"]
步骤 5："11111"，由 1 构成的组：["11111"]
存在长度为 1 的一组 1 的最后步骤是步骤 4 。
```

示例 2：

```text
输入：arr = [3,1,5,4,2], m = 2
输出：-1
解释：
步骤 1："00100"，由 1 构成的组：["1"]
步骤 2："10100"，由 1 构成的组：["1", "1"]
步骤 3："10101"，由 1 构成的组：["1", "1", "1"]
步骤 4："10111"，由 1 构成的组：["1", "111"]
步骤 5："11111"，由 1 构成的组：["11111"]
不管是哪一步骤都无法形成长度为 2 的一组 1 。
```

示例 3：

```text
输入：arr = [1], m = 1
输出：1
```

示例 4：

```text
输入：arr = [2,1], m = 2
输出：2
```

__提示：__

- `n == arr.length`
- `1 <= n <= 10 ^ 5`
- `1 <= arr[i] <= n`
- `arr` 中的所有整数 __互不相同__
- `1 <= m <= arr.length`

__思路:__

```text
模拟
用一个数组记录连续的 1 的起点和终点
初始化这个数组为 -1
对于一个新增的 1, 有以下三种情况:
1. 两边都没有 1, 即数组元素都是 -1, 只需要简单的将该位置赋值为本身即可
2. 左边有 1, 那么检查左边这个值对应的起点, 将该位置赋值为左边的元素的起点的值
3. 右边有 1, 那么检查右边这个值对应的终点, 将该位置赋值为右边的元素的终点的值
比如 [0, 1, 1, 1, 0, 1, 1, 1]
对应的起点和终点应该为 [-1, 3, 2, 1, -1, 7, 6, 5]
如果这时候需要把下标为 4 (对应 arr 数组中元素值为 5)赋值为 1
则起点和终点应该为 [-1, 7, 2, 1, 4, 7, 6, 1]
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int findLatestStep(vector<int>& arr, int m) 
    {
        int cur = 0, result = -1, n = arr.size(), link[100001];
        memset(link, -1, sizeof(link));
        for (int i = 0; i < n; i++) 
        {
            int pos = arr[i] - 1, left = pos, right = pos;
            link[pos] = pos;
            if (pos and link[pos - 1] != -1) 
            {
                if (pos - link[pos - 1] == m) --cur;
                left = link[pos - 1];
            }
            if (pos + 1 < n and link[pos + 1] != -1) 
            {
                if (link[pos + 1] - pos == m) --cur;
                right = link[pos + 1];
            }
            link[left] = right;
            link[right] = left;
            if (right - left + 1 == m) ++cur;
            if (cur) result = i + 1;
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int findLatestStep(int[] arr, int m) {
        int cur = 0, result = -1, n = arr.length, link[] = new int[100001];
        Arrays.fill(link, -1);
        for (int i = 0; i < n; i++) {
            int pos = arr[i] - 1, left = pos, right = pos;
            link[pos] = pos;
            if (pos > 0 && link[pos - 1] != -1) {
                if (pos - link[pos - 1] == m) --cur;
                left = link[pos - 1];
            }
            if (pos + 1 < n && link[pos + 1] != -1) {
                if (link[pos + 1] - pos == m) --cur;
                right = link[pos + 1];
            }
            link[left] = right;
            link[right] = left;
            if (right - left + 1 == m) ++cur;
            if (cur > 0) result = i + 1;
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def findLatestStep(self, arr: List[int], m: int) -> int:
        cur, result, link, n = 0, -1, [-1] * 100001, len(arr)
        for i in range(n):
            pos = left = right = arr[i] - 1
            link[pos] = pos
            if pos and link[pos - 1] != -1:
                if pos - link[pos - 1] == m:
                    cur -= 1
                left = link[pos - 1]
            if pos + 1 < n and link[pos + 1] != -1:
                if link[pos + 1] - pos == m:
                    cur -= 1
                right = link[pos + 1]
            link[left], link[right] = right, left
            if right - left + 1 == m:
                cur += 1
            if cur:
                result = i + 1
        return result
```
