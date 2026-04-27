# 1652 Defuse the Bomb 拆炸弹

__Description:__

You have a bomb to defuse, and your time is running out! Your informer will provide you with a __circular__ array `code` of length of `n` and a key `k`.

To decrypt the code, you must replace every number. All the numbers are replaced simultaneously.

- If `k > 0`, replace the `i ^ th` number with the sum of the __next__ `k` numbers.
- If `k < 0`, replace the `i ^ th` number with the sum of the __previous__ `k` numbers.
- If `k == 0`, replace the `i ^ th` number with `0`.

As `code` is circular, the next element of `code[n-1]` is `code[0]`, and the previous element of `code[0]` is `code[n-1]`.

Given the __circular__ array `code` and an integer key `k`, return _the decrypted code to defuse the bomb_!

__Example:__

Example 1:

```text
Input: code = [5,7,1,4], k = 3
Output: [12,10,16,13]
Explanation: Each number is replaced by the sum of the next 3 numbers. The decrypted code is [7+1+4, 1+4+5, 4+5+7, 5+7+1]. Notice that the numbers wrap around.
```

Example 2:

```text
Input: code = [1,2,3,4], k = 0
Output: [0,0,0,0]
Explanation: When k is zero, the numbers are replaced by 0.
```

Example 3:

```text
Input: code = [2,4,9,3], k = -2
Output: [12,5,6,13]
Explanation: The decrypted code is [3+9, 2+3, 4+2, 9+4]. Notice that the numbers wrap around again. If k is negative, the sum is of the previous numbers.
```

__Constraints:__

- `n == code.length`
- `1 <= n <= 100`
- `1 <= code[i] <= 100`
- `-(n - 1) <= k <= n - 1`

__题目描述:__

你有一个炸弹需要拆除，时间紧迫！你的情报员会给你一个长度为 `n` 的 __循环__ 数组 `code` 以及一个密钥 `k` 。

为了获得正确的密码，你需要替换掉每一个数字。所有数字会 同时 被替换。

- 如果 `k > 0` ，将第 `i` 个数字用 __接下来__ `k` 个数字之和替换。
- 如果 `k < 0` ，将第 `i` 个数字用 __之前__ `k` 个数字之和替换。
- 如果 `k == 0` ，将第 `i` 个数字用 `0` 替换。

由于 `code` 是循环的， `code[n-1]` 下一个元素是 `code[0]` ，且 `code[0]` 前一个元素是 `code[n-1]` 。

给你 __循环__ 数组 `code` 和整数密钥 `k` ，请你返回解密后的结果来拆除炸弹！

__示例:__

示例 1：

```text
输入：code = [5,7,1,4], k = 3
输出：[12,10,16,13]
解释：每个数字都被接下来 3 个数字之和替换。解密后的密码为 [7+1+4, 1+4+5, 4+5+7, 5+7+1]。注意到数组是循环连接的。
```

示例 2：

```text
输入：code = [1,2,3,4], k = 0
输出：[0,0,0,0]
解释：当 k 为 0 时，所有数字都被 0 替换。
```

示例 3：

```text
输入：code = [2,4,9,3], k = -2
输出：[12,5,6,13]
解释：解密后的密码为 [3+9, 2+3, 4+2, 9+4] 。注意到数组是循环连接的。如果 k 是负数，那么和为 之前 的数字。
```

__提示：__

- `n == code.length`
- `1 <= n <= 100`
- `1 <= code[i] <= 100`
- `-(n - 1) <= k <= n - 1`

__思路:__

```text
模拟
通过位运算可以判断 k 的正负, 直接判断也可以
按照方向将和的取值求出来
也可以使用前缀和加快求和
可以用取模保证不超出数组范围
时间复杂度为 O(NK), 空间复杂度为 O(1), 使用前缀和需要额外空间 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<int> decrypt(vector<int>& code, int k) 
    {
        int n = code.size(), type = (k >> 31) | (~((~k + 1) >> 31) + 1);
        vector<int> result(n);
        for (int i = 0; i < n; i++) 
        {
            int cur = 0;
            for (int j = type; j != type + k; j += type) cur += code[(i + j + n) % n];
            result[i] = cur;
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int[] decrypt(int[] code, int k) {
        int n = code.length, result[] = new int[n], type = (k >> 31) | (~((~k + 1) >> 31) + 1);
        for (int i = 0; i < n; i++) {
            int cur = 0;
            for (int j = type; j != type + k; j += type) cur += code[(i + j + n) % n];
            result[i] = cur;
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def decrypt(self, code: List[int], k: int) -> List[int]:
        return [sum(code[i + 1:i + k + 1]) for i in range(len(code) >> 1)] if (code := code + code) and k > 0 else [0] * (len(code) >> 1) if not k else [sum(code[i + k:i]) for i in range(len(code) >> 1, len(code))]
```
