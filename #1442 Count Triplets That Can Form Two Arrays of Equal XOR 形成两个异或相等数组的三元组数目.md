# 1442 Count Triplets That Can Form Two Arrays of Equal XOR 形成两个异或相等数组的三元组数目

__Description:__

Given an array of integers `arr`.

We want to select three indices `i`, `j` and `k` where `(0 <= i < j <= k < arr.length)`.

Let's define `a` and `b` as follows:

- `a = arr[i]  ^  arr[i + 1]  ^  ...  ^  arr[j - 1]`
- `b = arr[j]  ^  arr[j + 1]  ^  ...  ^  arr[k]`

Note that ^ denotes the bitwise-xor operation.

Return _the number of triplets_ (`i`, `j` and `k`) Where `a == b`.

__Example:__

Example 1:

```text
Input: arr = [2,3,1,6,7]
Output: 4
Explanation: The triplets are (0,1,2), (0,2,2), (2,3,4) and (2,4,4)
```

Example 2:

```text
Input: arr = [1,1,1,1,1]
Output: 10
```

__Constraints:__

- `1 <= arr.length <= 300`
- `1 <= arr[i] <= 10 ^ 8`

__题目描述:__

给你一个整数数组 `arr` 。

现需要从数组中取三个下标 `i`、`j` 和 `k` ，其中 `(0 <= i < j <= k < arr.length)` 。

`a` 和 `b` 定义如下：

- `a = arr[i]  ^  arr[i + 1]  ^  ...  ^  arr[j - 1]`
- `b = arr[j]  ^  arr[j + 1]  ^  ...  ^  arr[k]`

注意：^ 表示 按位异或 操作。

请返回能够令 `a == b` 成立的三元组 (`i`, `j` , `k`) 的数目。

__示例:__

示例 1：

```text
输入：arr = [2,3,1,6,7]
输出：4
解释：满足题意的三元组分别是 (0,1,2), (0,2,2), (2,3,4) 以及 (2,4,4)
```

示例 2：

```text
输入：arr = [1,1,1,1,1]
输出：10
```

示例 3：

```text
输入：arr = [2,3]
输出：0
```

示例 4：

```text
输入：arr = [1,3,5,7,9]
输出：3
```

示例 5：

```text
输入：arr = [7,11,12,9,5,2,7,17,22]
输出：8
```

__提示：__

- `1 <= arr.length <= 300`
- `1 <= arr[i] <= 10 ^ 8`

__思路:__

```text
前缀和 ➕ 哈希表
首先同一个数自身异或等于 0, 即 t ^ t == 0
记录异或前缀和 dp[i + 1] = dp[i] ^ arr[i]
其中初始化 dp[0] = 0
若 i < j, dp[i] ^ dp[j] = (arr[0] ^ arr[1] ^ ... ^ arr[i - 1]) ^ (arr[0] ^ arr[1] ^ ... ^ arr[j - 1]) = arr[i] ^ arr[i + 1] ^ ... ^ arr[j - 1], 即 arr 数组 [i, j] 区间的异或和可以表示为 dp[i] ^ dp[j + 1]
a == b 可以转化为 a = dp[i] ^ dp[j], b = dp[j] ^ dp[k + 1]
a == b -> dp[i] ^ dp[j] == dp[j] ^ dp[k + 1], 两边同时异或 dp[j] -> dp[i] == dp[k + 1]
所以实际上就是求 i < k 时前缀和中 dp[i] == dp[k + 1] 的数目, 与 j 无关, 所以当 i < j < k 时取任意值都满足
那么对应 [i, k] 三元组的个数一共有 k - i 个可能取值
那么如果 i0, i1, i2, ... im < k 均满足 dp[i] == dp[k + 1], 有 (k - i0) + (k - i1) + ... + (k - im) = mk - (i0 + i1 + ... + im)
使用两个哈希表记录 dp[k + 1] 对应的下标出现的次数和对应下标之和, 可以在一次遍历同时完成前缀和和下标的计算
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int countTriplets(vector<int>& arr) 
    {
        int n = arr.size(), result = 0, s = 0;
        unordered_map<int, int> cnt, total;
        for (int i = 0; i < n; i++) 
        {
            if (cnt.count(s ^ arr[i])) result += cnt[s ^ arr[i]] * i - total[s ^ arr[i]];
            ++cnt[s];
            total[s] += i;
            s ^= arr[i];
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int countTriplets(int[] arr) {
        int result = 0, n = arr.length, dp[] = new int[n + 1];
        for (int i = 0; i < n; i++) dp[i + 1] = dp[i] ^ arr[i];
        Map<Integer, Integer> count = new HashMap<>(), total = new HashMap<>();
        for (int i = 0; i < n; i++) {
            if (count.containsKey(dp[i + 1])) result += count.get(dp[i + 1]) * i - total.get(dp[i + 1]);
            count.put(dp[i], count.getOrDefault(dp[i], 0) + 1);
            total.put(dp[i], total.getOrDefault(dp[i], 0) + i);
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def countTriplets(self, arr: List[int]) -> int:
        result, dp, count, total = 0, [0] * ((n := len(arr)) + 1), defaultdict(int), defaultdict(int)
        for i in range(n):
            dp[i + 1] = dp[i] ^ arr[i]
        for i in range(n):
            if dp[i + 1] in count:
                result += count[dp[i + 1]] * i - total[dp[i + 1]]
            count[dp[i]] += 1
            total[dp[i]] += i
        return result
```
