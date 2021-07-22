# 765 Couples Holding Hands 情侣牵手

__Description__:
There are n couples sitting in 2n seats arranged in a row and want to hold hands.

The people and seats are represented by an integer array row where row[i] is the ID of the person sitting in the ith seat. The couples are numbered in order, the first couple being (0, 1), the second couple being (2, 3), and so on with the last couple being (2n - 2, 2n - 1).

Return the minimum number of swaps so that every couple is sitting side by side. A swap consists of choosing any two people, then they stand up and switch seats.

__Example:__

Example 1:

Input: row = [0,2,1,3]
Output: 1
Explanation: We only need to swap the second (row[1]) and third (row[2]) person.

Example 2:

Input: row = [3,2,0,1]
Output: 0
Explanation: All couples are already seated side by side.

__Constraints:__

2n == row.length
2 <= n <= 30
n is even.
0 <= row[i] < 2n
All the elements of row are unique.

__题目描述__:
N 对情侣坐在连续排列的 2N 个座位上，想要牵到对方的手。 计算最少交换座位的次数，以便每对情侣可以并肩坐在一起。 一次交换可选择任意两人，让他们站起来交换座位。

人和座位用 0 到 2N-1 的整数表示，情侣们按顺序编号，第一对是 (0, 1)，第二对是 (2, 3)，以此类推，最后一对是 (2N-2, 2N-1)。

这些情侣的初始座位  row[i] 是由最初始坐在第 i 个座位上的人决定的。

__示例 :__

示例 1:

输入: row = [0, 2, 1, 3]
输出: 1
解释: 我们只需要交换row[1]和row[2]的位置即可。

示例 2:

输入: row = [3, 2, 0, 1]
输出: 0
解释: 无需交换座位，所有的情侣都已经可以手牵手了。

__说明:__

len(row) 是偶数且数值在 [4, 60]范围内。
可以保证row 是序列 0...len(row)-1 的一个全排列。

__思路__:

1. 暴力法(Java)
两个情侣的编号相差 1
说明两个情侣的编号可以通过 i ^ 1 = j & j ^ 1 = i 来获得, 其中 i 是情侣中的偶数对应的下标, j 是情侣中的奇数对应的下标
每次成对的查找两个人是否是情侣, 如果不是, 从后面找到情侣并交换座位
时间复杂度为 O(n ^ 2), 空间复杂度为 O(1)
2. 贪心法(Python)
用一个索引表 cache 记录, 使 cache[row[i]] = i
交换时, 直接用索引表找到情侣进行交换并更新索引表即可
时间复杂度为 O(n), 空间复杂度为 O(n)
3. 并查集(C++)
将情侣看作结点, 牵手则表示结点已经连接成环
如果刚好牵手, 那么一共应该有 (n << 1) 个环
有 k 对情侣没有牵手, 那么要移动 k - 1 次才能使情侣牵手
找出情侣图中环个数 count, 返回 (n << 1) - count 即可
时间复杂度为 O(nlgn), 空间复杂度为 O(n), 使用路径压缩及权重可以将时间复杂度降为 O(n)

__代码__:
__C++__:

```C++
class Solution {
private:
    vector<int> parent{vector<int>(60)};
    
    void connect(int p, int q) 
    {
        parent[find(p)] = parent[find(q)];
    }
    
    int find(int p) 
    {
        if (parent[p] != p) parent[p] = find(parent[p]);
        return parent[p];
    }
public:
    int minSwapsCouples(vector<int>& row) 
    {
        int n = row.size(), m = n >> 1, count = 0;
        for (int i = 0; i < m; i++) parent[i] = i;
        for (int i = 0; i < n; i += 2) connect(row[i] >> 1, row[i + 1] >> 1);
        for (int i = 0; i < m; i++) if (i == find(i)) ++count;
        return m - count;
    }
};
```

__Java__:

```Java
class Solution {
    public int minSwapsCouples(int[] row) {
        int n = row.length, result = 0;
        for (int i = 0; i < n - 1; i += 2) {
            if (row[i] != (row[i + 1] ^ 1)) {
                for (int j = i + 2; j < n; j++) {
                    if ((row[j] ^ 1) == row[i]) {
                        row[i + 1] ^= row[j];
                        row[j] ^= row[i + 1];
                        row[i + 1] ^= row[j];
                        ++result;
                        break;
                    }
                }
            }
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def minSwapsCouples(self, row: List[int]) -> int:
        result, cache = 0, [0] * (n := len(row))
        for i in range(n):
            cache[row[i]] = i
        for i in range(0, n, 2):
            if row[i] != row[i + 1] ^ 1:
                cache[row[cache[row[i] ^ 1]]], cache[row[i + 1]], row[i + 1], row[cache[row[i + 1]]] = i + 1, cache[row[i] ^ 1], row[cache[row[i + 1]]], row[i + 1]
                result += 1
        return result
```
