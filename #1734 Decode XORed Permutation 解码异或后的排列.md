# 1734 Decode XORed Permutation 解码异或后的排列

__Description:__

There is an integer array `perm` that is a permutation of the first `n` positive integers, where `n` is always __odd__.

It was encoded into another integer array `encoded` of length `n - 1`, such that `encoded[i] = perm[i] XOR perm[i + 1]`. For example, if `perm = [1,3,2]`, then `encoded = [2,1]`.

Given the `encoded` array, return _the original array_ `perm`. It is guaranteed that the answer exists and is unique.

__Example:__

Example 1:

```text
Input: encoded = [3,1]
Output: [1,2,3]
Explanation: If perm = [1,2,3], then encoded = [1 XOR 2,2 XOR 3] = [3,1]
```

Example 2:

```text
Input: encoded = [6,5,4,6]
Output: [2,4,1,5,3]
```

__Constraints:__

- `3 <= n < 10 ^ 5`
- `n` is odd.
- `encoded.length == n - 1`

__题目描述:__

给你一个整数数组 `perm` ，它是前 `n` 个正整数的排列，且 `n` 是个 __奇数__ 。

它被加密成另一个长度为 `n - 1` 的整数数组 `encoded` ，满足 `encoded[i] = perm[i] XOR perm[i + 1]` 。比方说，如果 `perm = [1,3,2]` ，那么 `encoded = [2,1]` 。

给你 `encoded` 数组，请你返回原始数组 `perm` 。题目保证答案存在且唯一。

__示例:__

示例 1：

```text
输入：encoded = [3,1]
输出：[1,2,3]
解释：如果 perm = [1,2,3] ，那么 encoded = [1 XOR 2,2 XOR 3] = [3,1]
```

示例 2：

```text
输入：encoded = [6,5,4,6]
输出：[2,4,1,5,3]
```

__提示：__

- `3 <= n < 10 ^ 5`
- `n` 是奇数。
- `encoded.length == n - 1`

__思路:__

```text
位运算
异或运算满足:
a ^ b = c => a ^ c = b, b ^ c = a
由于 perm 是前 n 个正整数的排列，所以可以通过异或运算得到 1 到 n 的异或结果 total
注意到 encoded[i] = perm[i] XOR perm[i + 1], 如果选出所有奇数下标的 encoded[i]，那么可以得到 odd = perm[1] XOR perm[2] XOR ... XOR perm[n] = total XOR perm[0]
所以 perm[0] = total XOR odd
于是 perm[i + 1] = perm[i] XOR encoded[i]
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<int> decode(vector<int>& encoded) 
    {
        int n = encoded.size(), total = n + 1;
        for (int i = 1; i <= n; i++) total ^= i;
        for (int i = 1; i < n; i += 2) total ^= encoded[i];
        vector<int> result(n + 1, total);
        for (int i = 0; i < n; i++) result[i + 1] = result[i] ^ encoded[i];
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int[] decode(int[] encoded) {
        int n = encoded.length, total = n + 1, result[] = new int[n + 1];
        for (int i = 1; i <= n; i++) total ^= i;
        for (int i = 1; i < n; i += 2) total ^= encoded[i];
        result[0] = total;
        for (int i = 0; i < n; i++) result[i + 1] = result[i] ^ encoded[i];
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def decode(self, encoded: List[int]) -> List[int]:
        return list(accumulate([reduce(ixor, encoded[1::2] + list(range(1, len(encoded) + 2)))] + encoded, ixor))
```
