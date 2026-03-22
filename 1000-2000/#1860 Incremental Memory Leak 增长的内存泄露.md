# 1860 Incremental Memory Leak 增长的内存泄露

__Description:__

You are given two integers `memory1` and `memory2` representing the available memory in bits on two memory sticks. There is currently a faulty program running that consumes an increasing amount of memory every second.

At the `i ^ th` second (starting from 1), `i` bits of memory are allocated to the stick with __more available memory__ (or from the first memory stick if both have the same available memory). If neither stick has at least `i` bits of available memory, the program __crashes__.

Return _an array containing_ `[crashTime, memory1crash, memory2crash]`_, where_ `crashTime` _is the time (in seconds) when the program crashed and_ `memory1crash` _and_ `memory2crash` _are the available bits of memory in the first and second sticks respectively_.

__Example:__

Example 1:

```text
Input: memory1 = 2, memory2 = 2
Output: [3,1,0]
Explanation: The memory is allocated as follows:
```

- At the 1st second, 1 bit of memory is allocated to stick 1. The first stick now has 1 bit of available memory.
- At the 2nd second, 2 bits of memory are allocated to stick 2. The second stick now has 0 bits of available memory.
- At the 3rd second, the program crashes. The sticks have 1 and 0 bits available respectively.

Example 2:

```text
Input: memory1 = 8, memory2 = 11
Output: [6,0,4]
Explanation: The memory is allocated as follows:
```

- At the 1st second, 1 bit of memory is allocated to stick 2. The second stick now has 10 bit of available memory.
- At the 2nd second, 2 bits of memory are allocated to stick 2. The second stick now has 8 bits of available memory.
- At the 3rd second, 3 bits of memory are allocated to stick 1. The first stick now has 5 bits of available memory.
- At the 4th second, 4 bits of memory are allocated to stick 2. The second stick now has 4 bits of available memory.
- At the 5th second, 5 bits of memory are allocated to stick 1. The first stick now has 0 bits of available memory.
- At the 6th second, the program crashes. The sticks have 0 and 4 bits available respectively.

__Constraints:__

- `0 <= memory1, memory2 <= 2 ^ 31 - 1`

__题目描述:__

给你两个整数 `memory1` 和 `memory2` 分别表示两个内存条剩余可用内存的位数。现在有一个程序每秒递增的速度消耗着内存。

在第 `i` 秒（秒数从 1 开始），有 `i` 位内存被分配到 __剩余内存较多__ 的内存条（如果两者一样多，则分配到第一个内存条）。如果两者剩余内存都不足 `i` 位，那么程序将 _意外退出_ 。

请你返回一个数组，包含 `[crashTime, memory1crash, memory2crash]` ，其中 `crashTime`是程序意外退出的时间（单位为秒）， `memory1crash` 和 `memory2crash` 分别是两个内存条最后剩余内存的位数。

__示例:__

示例 1：

```text
输入：memory1 = 2, memory2 = 2
输出：[3,1,0]
解释：内存分配如下：
```

- 第 1 秒，内存条 1 被占用 1 位内存。内存条 1 现在有 1 位剩余可用内存。
- 第 2 秒，内存条 2 被占用 2 位内存。内存条 2 现在有 0 位剩余可用内存。
- 第 3 秒，程序意外退出，两个内存条分别有 1 位和 0 位剩余可用内存。

示例 2：

```text
输入：memory1 = 8, memory2 = 11
输出：[6,0,4]
解释：内存分配如下：
```

- 第 1 秒，内存条 2 被占用 1 位内存，内存条 2 现在有 10 位剩余可用内存。
- 第 2 秒，内存条 2 被占用 2 位内存，内存条 2 现在有 8 位剩余可用内存。
- 第 3 秒，内存条 1 被占用 3 位内存，内存条 1 现在有 5 位剩余可用内存。
- 第 4 秒，内存条 2 被占用 4 位内存，内存条 2 现在有 4 位剩余可用内存。
- 第 5 秒，内存条 1 被占用 5 位内存，内存条 1 现在有 0 位剩余可用内存。
- 第 6 秒，程序意外退出，两个内存条分别有 0 位和 4 位剩余可用内存。

__提示：__

- `0 <= memory1, memory2 <= 2 ^ 31 - 1`

__思路:__

```text
1. 模拟
按照题意选择最大的内存减去时间即可
t * (t + 1) <= memory1 * memory2
时间复杂度为 O(MN ^ 1 / 2), 空间复杂度为 O(1)
2. 数学法
先将两个内存减少到 memory1 > memory2
这个过程可以用 k + k + 1 + ... k + a <= abs(memory1 - memory2) 计算
然后再将 memory1 和 memory2 减少到 0
可以分别用 
i + i + 3 + ... + i + a <= memory1 计算, 其中 a 为奇数
j + 2 + j + 4 + ... + j + b <= memory2 计算, 其中 b 为偶数
时间复杂度为 O(1), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<int> memLeak(int memory1, int memory2) 
    {
        int diff = abs(memory1 - memory2), k = floor(sqrt(2.0 * diff + 0.25) - 0.5);
        if (memory1 > memory2) memory1 -= (long)(k + 1) * k / 2;
        else memory2 -= (long)(k + 1) * k / 2;
        if (memory2 > memory1) memory2 -= ++k;
        int i = floor(sqrt(memory1 + k / 4.0 * k) - k / 2.0), j = floor(sqrt(memory2 + (k + 1) / 4.0 * (k + 1)) - (k + 1) / 2.0);
        memory1 -= i * i + k * i;
        memory2 -= j * j + j + k * j;
        return {i + j + k + 1, memory1, memory2};
    }
};
```

__Java__:

```Java
class Solution {
    public int[] memLeak(int memory1, int memory2) {
        int result = 1;
        while (result <= memory1 || result <= memory2) {
            if (memory1 < memory2) memory2 -= result++;
            else memory1 -= result++;
        }
        return new int[]{result, memory1, memory2};
    }
}
```

__Python__:

```Python
class Solution:
    def memLeak(self, memory1: int, memory2: int) -> List[int]:
        result = 1
        while result <= max(memory1, memory2):
            if memory1 < memory2:
                memory2 -= result
            else:
                memory1 -= result
            result += 1
        return [result, memory1, memory2]
```
