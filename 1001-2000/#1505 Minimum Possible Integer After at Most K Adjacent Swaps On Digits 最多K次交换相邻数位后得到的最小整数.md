# 1505 Minimum Possible Integer After at Most K Adjacent Swaps On Digits 最多K次交换相邻数位后得到的最小整数

__Description:__

You are given a string `num` representing __the digits__ of a very large integer and an integer `k`. You are allowed to swap any two adjacent digits of the integer __at most__ `k` times.

Return the minimum integer you can obtain also as a string.

__Example:__

Example 1:

![1505-1](https://assets.leetcode.com/uploads/2020/06/17/q4_1.jpg)

```text
Input: num = "4321", k = 4
Output: "1342"
Explanation: The steps to obtain the minimum integer from 4321 with 4 adjacent swaps are shown.
```

Example 2:

```text
Input: num = "100", k = 1
Output: "010"
Explanation: It's ok for the output to have leading zeros, but the input is guaranteed not to have any leading zeros.
```

Example 3:

```text
Input: num = "36789", k = 1000
Output: "36789"
Explanation: We can keep the number without any swaps.
```

__Constraints:__

- `1 <= num.length <= 3 * 10 ^ 4`
- `num` consists of only __digits__ and does not contain __leading zeros__.
- `1 <= k <= 10 ^ 9`

__题目描述:__

给你一个字符串 `num` 和一个整数 `k` 。其中， `num` 表示一个很大的整数，字符串中的每个字符依次对应整数上的各个 __数位__ 。

你可以交换这个整数相邻数位的数字 __最多__ `k` 次。

请你返回你能得到的最小整数，并以字符串形式返回。

__示例:__

示例 1：

![1505-2](https://assets.leetcode.com/uploads/2020/06/17/q4_1.jpg)

```text
输入：num = "4321", k = 4
输出："1342"
解释：4321 通过 4 次交换相邻数位得到最小整数的步骤如上图所示。
```

示例 2：

```text
输入：num = "100", k = 1
输出："010"
解释：输出可以包含前导 0 ，但输入保证不会有前导 0 。
```

示例 3：

```text
输入：num = "36789", k = 1000
输出："36789"
解释：不需要做任何交换。
```

示例 4：

```text
输入：num = "22", k = 22
输出："22"
```

示例 5：

```text
输入：num = "9438957234785635408", k = 23
输出："0345989723478563548"
```

__提示：__

- `1 <= num.length <= 30000`
- `num` 只包含 __数字__ 且不含有 __前导 0__ 。
- `1 <= k <= 10 ^ 9`

__思路:__

```text
贪心 ➕ 线段树
每次选择当前最小的数字将其移动到开头
记录下每个数字的位置, 每次选择最近的移动
由于每次交换会导致后面的数字的位置发生变化
用线段树记录移动的位置
时间复杂度为 O(NlogN), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class BIT 
{
private:
    vector<int> tree;
    int n;
public:
    BIT(int _n): n(_n), tree(_n + 1) {}

    static int lowbit(int x) 
    {
        return x & (-x);
    }
    
    void update(int x) 
    {
        while (x <= n) 
        {
            ++tree[x];
            x += lowbit(x);
        }
    }

    int query(int x) const 
    {
        int result = 0;
        while (x) 
        {
            result += tree[x];
            x -= lowbit(x);
        }
        return result;
    }

    int query(int x, int y) const 
    {
        return query(y) - query(x - 1);
    }
};

class Solution 
{
public:
    string minInteger(string num, int k) 
    {
        int n = num.size();
        vector<queue<int>> pos(10);
        for (int i = 0; i < n; i++) pos[num[i] - '0'].push(i + 1);
        string result;
        BIT bit(n);
        for (int i = 1; i <= n; i++) 
        {
            for (int j = 0; j < 10; j++) 
            {
                if (!pos[j].empty()) 
                {
                    int behind = bit.query(pos[j].front() + 1, n), dist = pos[j].front() + behind - i;
                    if (dist <= k) 
                    {
                        bit.update(pos[j].front());
                        pos[j].pop();
                        result += (j + '0');
                        k -= dist;
                        break;
                    }
                }
            }
        }
        return result;
    }
};
```

__Java__:

```Java
class BIT {
    private int[] tree;
    private int n;
    
    public BIT(int n) {
        this.n = n;
        this.tree = new int[n + 1];
    }
    
    public static int lowbit(int x) {
        return x & (-x);
    }
    
    public void update(int x) {
        while (x <= n) {
            ++tree[x];
            x += lowbit(x);
        }
    }
    
    public int query(int x) {
        int result = 0;
        while (x > 0) {
            result += tree[x];
            x -= lowbit(x);
        }
        return result;
    }
    
    public int query(int x, int y) {
        return query(y) - query(x - 1);
    }
}

class Solution {
    public String minInteger(String num, int k) {
        int n = num.length();
        Queue<Integer> pos[] = new Queue[10];
        for (int i = 0; i < 10; i++) pos[i] = new LinkedList<>();
        for (int i = 0; i < n; i++) pos[num.charAt(i) - '0'].add(i + 1);
        StringBuilder result = new StringBuilder();
        BIT bit = new BIT(n);
        for (int i = 1; i <= n; i++) {
            for (int j = 0; j < 10; j++) {
                if (!pos[j].isEmpty()) {
                    int behind = bit.query(pos[j].peek() + 1, n), dist = pos[j].peek() + behind - i;
                    if (dist <= k) {
                        bit.update(pos[j].peek());
                        pos[j].poll();
                        result.append((char)(j + 48));
                        k -= dist;
                        break;
                    }
                }
            }
        }
        return result.toString();
    }
}
```

__Python__:

```Python
class BIT:
    def __init__(self, n: int) -> None:
        """
        初始化树状数组
        :param n: 树状数组容量
        """
        self.n = n
        self.tree = [0] * (n + 1)

    @staticmethod
    def lowbit(x: int) -> int:
        """
        得到一个数的最低位对应的 2的幂
        如 2(0b10) -> 2, 7(0b111) -> 1
        :param x: 输入一个整数
        :return: 得到其最低位对应的 2的幂
        """
        return x & (-x)

    def update(self, x: int, k: int) -> None:
        """
        单点更新 给 a[x] + k
        :param x: 下标
        :param k: 更新值
        :return:
        """
        while x <= self.n:
            self.tree[x] += k
            x += self.lowbit(x)

    def query(self, x: int) -> int:
        """
        区间查询 sum(a[:x + 1])
        :param x: 查询下标
        :return:
        """
        result = 0
        while x:
            result += self.tree[x]
            x -= self.lowbit(x)
        return result
class Solution:
    def minInteger(self, num: str, k: int) -> str:
        bit, pos, result = BIT(n := len(num)), [deque() for _ in range(10)], deque()
        for i in range(n):
            pos[int(num[i])].append(i + 1)
        for i in range(1, n + 1):
            for j in range(10):
                if pos[j]:
                    dist = pos[j][0] + (behind := bit.query(n) - bit.query(pos[j][0])) - i
                    if dist <= k:
                        bit.update(pos[j][0], 1)
                        pos[j].popleft()
                        result.append(str(j))
                        k -= dist
                        break
        return ''.join(result)
```
