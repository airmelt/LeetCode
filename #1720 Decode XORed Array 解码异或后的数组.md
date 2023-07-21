# 1720 Decode XORed Array 解码异或后的数组

__Description:__

There is a __hidden__ integer array `arr` that consists of `n` non-negative integers.

It was encoded into another integer array `encoded` of length `n - 1`, such that `encoded[i] = arr[i] XOR arr[i + 1]`. For example, if `arr = [1,0,2,1]`, then `encoded = [1,2,3]`.

You are given the `encoded` array. You are also given an integer `first`, that is the first element of `arr`, i.e. `arr[0]`.

Return _the original array_ `arr`. It can be proved that the answer exists and is unique.

__Example:__

Example 1:

```text
Input: encoded = [1,2,3], first = 1
Output: [1,0,2,1]
Explanation: If arr = [1,0,2,1], then first = 1 and encoded = [1 XOR 0, 0 XOR 2, 2 XOR 1] = [1,2,3]
```

Example 2:

```text
Input: encoded = [6,2,7,3], first = 4
Output: [4,2,0,7,4]
```

__Constraints:__

- `2 <= n <= 10 ^ 4`
- `encoded.length == n - 1`
- `0 <= encoded[i] <= 10 ^ 5`
- `0 <= first <= 10 ^ 5`

__题目描述:__

__未知__ 整数数组 `arr` 由 `n` 个非负整数组成。

经编码后变为长度为 `n - 1` 的另一个整数数组 `encoded` ，其中 `encoded[i] = arr[i] XOR arr[i + 1]` 。例如， `arr = [1,0,2,1]` 经编码后得到 `encoded = [1,2,3]` 。

给你编码后的数组 `encoded` 和原数组 `arr` 的第一个元素 `first`（ `arr[0]`）。

请解码返回原数组 `arr` 。可以证明答案存在并且是唯一的。

__示例:__

示例 1：

```text
输入：encoded = [1,2,3], first = 1
输出：[1,0,2,1]
解释：若 arr = [1,0,2,1] ，那么 first = 1 且 encoded = [1 XOR 0, 0 XOR 2, 2 XOR 1] = [1,2,3]
```

示例 2：

```text
输入：encoded = [6,2,7,3], first = 4
输出：[4,2,0,7,4]
```

__提示：__

- `2 <= n <= 10 ^ 4`
- `encoded.length == n - 1`
- `0 <= encoded[i] <= 10 ^ 5`
- `0 <= first <= 10 ^ 5`

__思路:__

```text
位运算
根据 a ^ b = c, 可以得到 a ^ c = b, b ^ c = a
因此, 可以将 first 插入到 encoded 的首位, 然后遍历 encoded, 依次计算出后面的元素
计算方法为 encoded[i + 1] = encoded[i] ^ encoded[i + 1]
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<int> decode(vector<int>& encoded, int first) 
    {
        encoded.insert(encoded.begin(), first);
        for (int i = 0, n = encoded.size(); i < n - 1; i++) encoded[i + 1] ^= encoded[i];
        return encoded;
    }
};
```

__Java__:

```Java
class Solution {
    public int[] decode(int[] encoded, int first) {
        int n = encoded.length, result[] = new int[n + 1];
        result[0] = first;
        for (int i = 0; i < n; i++) result[i + 1] = result[i] ^ encoded[i];
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def decode(self, encoded: List[int], first: int) -> List[int]:
        return [first] + [(first := first ^ i) for i in encoded]
```
