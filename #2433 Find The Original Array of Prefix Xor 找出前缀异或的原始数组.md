# 2433 Find The Original Array of Prefix Xor 找出前缀异或的原始数组

__Description:__

You are given an __integer__ array `pref` of size `n`. Find and return _the array_ `arr` _of size_ `n` _that satisfies_:

- `pref[i] = arr[0] ^ arr[1] ^ ... ^ arr[i]`.

Note that `^` denotes the __bitwise-xor__ operation.

It can be proven that the answer is unique.

__Example:__

Example 1:

```text
Input: pref = [5,2,0,3,1]
Output: [5,7,2,3,2]
Explanation: From the array [5,7,2,3,2] we have the following:
```

- pref[0] = 5.
- pref[1] = 5  ^  7 = 2.
- pref[2] = 5  ^  7  ^  2 = 0.
- pref[3] = 5  ^  7  ^  2  ^  3 = 3.
- pref[4] = 5  ^  7  ^  2  ^  3  ^  2 = 1.

Example 2:

```text
Input: pref = [13]
Output: [13]
Explanation: We have pref[0] = arr[0] = 13.
```

__Constraints:__

- `1 <= pref.length <= 10 ^ 5`
- `0 <= pref[i] <= 10 ^ 6`

__题目描述:__

给你一个长度为 `n` 的 __整数__ 数组 `pref` 。找出并返回满足下述条件且长度为 `n` 的数组 `arr` :

- `pref[i] = arr[0] ^ arr[1] ^ ... ^ arr[i]`.

注意 `^` 表示 __按位异或__（bitwise-xor）运算。

可以证明答案是 唯一 的。

__示例:__

示例 1：

```text
输入：pref = [5,2,0,3,1]
输出：[5,7,2,3,2]
解释：从数组 [5,7,2,3,2] 可以得到如下结果：
```

- pref[0] = 5
- pref[1] = 5  ^  7 = 2
- pref[2] = 5  ^  7  ^  2 = 0
- pref[3] = 5  ^  7  ^  2  ^  3 = 3
- pref[4] = 5  ^  7  ^  2  ^  3  ^  2 = 1

示例 2：

```text
输入：pref = [13]
输出：[13]
解释：pref[0] = arr[0] = 13
```

__提示：__

- `1 <= pref.length <= 10 ^ 5`
- `0 <= pref[i] <= 10 ^ 6`

__思路:__

```text
位运算
由 pref[i] = result[0] ^ result[1] ^ ... ^ result[i]
又 pref[i - 1] = result[0] ^ result[1] ^ ... ^ result[i - 1]
可得 pref[i] = pref[i - 1] ^ result[i]
由于异或运算的性质，我们可以得到如下关系
a ^ b = c => a = b ^ c
result[i] = pref[i - 1] ^ result[i - 1]
用一个变量记录 total = result[0] ^ result[1] ^ ... ^ result[i]
则 result[i] = total ^ pref[i]
时间复杂度为 O(N), 空间复杂度为 O(1), 结果不计入空间复杂度
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<int> findArray(vector<int>& pref) 
    {
        int n = pref.size(), total = 0;
        vector<int> result(n);
        for (int i = 0; i < n; i++) 
        {
            result[i] = total ^ pref[i];
            total ^= result[i];
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int[] findArray(int[] pref) {
        int n = pref.length, result[] = new int[n], total = 0;
        for (int i = 0; i < n; i++) {
            result[i] = total ^ pref[i];
            total ^= result[i];
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def findArray(self, pref: List[int]) -> List[int]:
        return [x ^ y for x, y in pairwise([0] + pref)]
```
