# 1850 Minimum Adjacent Swaps to Reach the Kth Smallest Number 邻位交换的最小次数

__Description:__

You are given a string `num`, representing a large integer, and an integer `k`.

We call some integer __wonderful__ if it is a __permutation__ of the digits in `num` and is __greater in value__ than `num`. There can be many wonderful integers. However, we only care about the __smallest-valued__ ones.

- For example, when `num = "5489355142"`:
  - The 1 ^ st smallest wonderful integer is `"5489355214"`.
  - The 2 ^ nd smallest wonderful integer is `"5489355241"`.
  - The 3 ^ rd smallest wonderful integer is `"5489355412"`.
  - The 4 ^ th smallest wonderful integer is `"5489355421"`.

Return _the __minimum number of adjacent digit swaps__ that needs to be applied to_ `num` _to reach the_ `k ^ th` ___smallest wonderful__ integer_.

The tests are generated in such a way that `k ^ th` smallest wonderful integer exists.

__Example:__

Example 1:

```text
Input: num = "5489355142", k = 4
Output: 2
Explanation: The 4th smallest wonderful number is "5489355421". To get this number:
```

- Swap index 7 with index 8: "5489355142" -> "5489355412"
- Swap index 8 with index 9: "5489355412" -> "5489355421"

Example 2:

```text
Input: num = "11112", k = 4
Output: 4
Explanation: The 4th smallest wonderful number is "21111". To get this number:
```

- Swap index 3 with index 4: "11112" -> "11121"
- Swap index 2 with index 3: "11121" -> "11211"
- Swap index 1 with index 2: "11211" -> "12111"
- Swap index 0 with index 1: "12111" -> "21111"

Example 3:

```text
Input: num = "00123", k = 1
Output: 1
Explanation: The 1st smallest wonderful number is "00132". To get this number:
```

- Swap index 3 with index 4: "00123" -> "00132"

__Constraints:__

- `2 <= num.length <= 1000`
- `1 <= k <= 1000`
- `num` only consists of digits.

__题目描述:__

给你一个表示大整数的字符串 `num` ，和一个整数 `k` 。

如果某个整数是 `num` 中各位数字的一个 __排列__ 且它的 __值大于__ `num` ，则称这个整数为 __妙数__ 。可能存在很多妙数，但是只需要关注 __值最小__ 的那些。

- 例如， `num = "5489355142"` :
  - 第 1 个最小妙数是 `"5489355214"`
  - 第 2 个最小妙数是 `"5489355241"`
  - 第 3 个最小妙数是 `"5489355412"`
  - 第 4 个最小妙数是 `"5489355421"`

返回要得到第 `k` 个 __最小妙数__ 需要对 `num` 执行的 __相邻位数字交换的最小次数__ 。

测试用例是按存在第 `k` 个最小妙数而生成的。

__示例:__

示例 1：

```text
输入：num = "5489355142", k = 4
输出：2
解释：第 4 个最小妙数是 "5489355421" ，要想得到这个数字：
```

- 交换下标 7 和下标 8 对应的位："5489355142" -> "5489355412"
- 交换下标 8 和下标 9 对应的位："5489355412" -> "5489355421"

示例 2：

```text
输入：num = "11112", k = 4
输出：4
解释：第 4 个最小妙数是 "21111" ，要想得到这个数字：
```

- 交换下标 3 和下标 4 对应的位："11112" -> "11121"
- 交换下标 2 和下标 3 对应的位："11121" -> "11211"
- 交换下标 1 和下标 2 对应的位："11211" -> "12111"
- 交换下标 0 和下标 1 对应的位："12111" -> "21111"

示例 3：

```text
输入：num = "00123", k = 1
输出：1
解释：第 1 个最小妙数是 "00132" ，要想得到这个数字：
```

- 交换下标 3 和下标 4 对应的位："00123" -> "00132"

__提示：__

- `2 <= num.length <= 1000`
- `1 <= k <= 1000`
- `num` 仅由数字组成

__思路:__

```text
排列 ➕ 贪心
先利用下一个排列找到第 k 个最小妙数
然后贪心地交换相邻的数字
找到第一个与原位置不同的数字, 将其交换到下一个不相等的数字的位置
时间复杂度为 O(N(N + K)), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int getMinSwaps(string num, int k) 
    {
        string nums_k = num;
        int n = num.size(), result = 0;
        while (k--) next_permutation(num.begin(), num.end());
        for (int i = 0; i < n; i++) 
        {
            if (num[i] != nums_k[i]) 
            {
                for (int j = i + 1; j < n; ++j) 
                {
                    if (num[j] == nums_k[i]) 
                    {
                        for (k = j - 1; k >= i; --k) 
                        {
                            ++result;
                            swap(num[k], num[k + 1]);
                        }
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
class Solution {
    public int getMinSwaps(String num, int k) {
        int n = num.length(), nums[] = new int[n], result = 0, numsK[] = new int[n];
        for (int i = 0; i < n; i++) {
            nums[i] = num.charAt(i) - '0';
            numsK[i] = num.charAt(i) - '0';
        }
        while (k-- > 0) nextPermutation(nums);
        for (int i = 0; i < n; i++) {
            if (nums[i] != numsK[i]) {
                for (int j = i + 1; j < n; ++j) {
                    if (nums[i] == numsK[j]) {
                        for (k = j - 1; k >= i; --k) {
                            ++result;
                            numsK[k] ^= numsK[k + 1];
                            numsK[k + 1] ^= numsK[k];
                            numsK[k] ^= numsK[k + 1];
                        }
                        break;
                    }
                }
            }
        }
        return result;
    }

    private void nextPermutation(int[] nums) {
        int i = nums.length - 1;
        while (i > 0 && nums[i] <= nums[i - 1]) --i; 
        int a = i, b = nums.length - 1;
        while (a < b) {
            nums[a] ^= nums[b];
            nums[b] ^= nums[a];
            nums[a++] ^= nums[b--];
        }
        int j = i - 1;
        if (j < 0) return;
        for (int index = i; index < nums.length; index++) {
            if (nums[index] > nums[j]) {
                nums[index] ^= nums[j];
                nums[j] ^= nums[index];
                nums[index] ^= nums[j];
                break;
            }
        }
    }
}
```

__Python__:

```Python
class Solution:
    def getMinSwaps(self, num: str, k: int) -> int:
        result, n, nums_k, nums = 0, len(num), list(num), list(num)
        for _ in range(k):
            self.nextPermutation(nums)
        for i in range(n):
            if nums_k[i] != nums[i]:
                for j in range(i + 1, n):
                    if nums_k[j] == nums[i]:
                        for k in range(j - 1, i - 1, -1):
                            result += 1
                            nums_k[k], nums_k[k + 1] = nums_k[k + 1], nums_k[k]
                        break
        return result


    def nextPermutation(self, nums: List[int]) -> None:
        """
        Do not return anything, modify nums in-place instead.
        """
        i = len(nums) - 1
        while i and nums[i] <= nums[i - 1]:
            i -= 1
        a, b = i, len(nums) - 1
        while a < b:
            nums[a], nums[b] = nums[b], nums[a]
            a += 1
            b -= 1
        j = i - 1
        for index in range(i, len(nums)):
            if nums[index] > nums[j]:
                nums[j], nums[index] = nums[index], nums[j]
                break
```
