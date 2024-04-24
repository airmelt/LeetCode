# 2086 Minimum Number of Food Buckets to Feed the Hamsters 喂食仓鼠的最小食物桶数

__Description:__

You are given a __0-indexed__ string `hamsters` where `hamsters[i]` is either:

- `'H'` indicating that there is a hamster at index `i`, or
- `'.'` indicating that index `i` is empty.

You will add some number of food buckets at the empty indices in order to feed the hamsters. A hamster can be fed if there is at least one food bucket to its left or to its right. More formally, a hamster at index `i` can be fed if you place a food bucket at index `i - 1` __and/or__ at index `i + 1`.

Return _the minimum number of food buckets you should __place at empty indices__ to feed all the hamsters or_ `-1` _if it is impossible to feed all of them_.

__Example:__

Example 1:

![2086-1](https://assets.leetcode.com/uploads/2022/11/01/example1.png)

```text
Input: hamsters = "H..H"
Output: 2
Explanation: We place two food buckets at indices 1 and 2.
It can be shown that if we place only one food bucket, one of the hamsters will not be fed.
```

Example 2:

![2086-2](https://assets.leetcode.com/uploads/2022/11/01/example2.png)

```text
Input: hamsters = ".H.H."
Output: 1
Explanation: We place one food bucket at index 2.
```

Example 3:

![2086-3](https://assets.leetcode.com/uploads/2022/11/01/example3.png)

```text
Input: hamsters = ".HHH."
Output: -1
Explanation: If we place a food bucket at every empty index as shown, the hamster at index 2 will not be able to eat.
```

__Constraints:__

- `1 <= hamsters.length <= 10 ^ 5`
- `hamsters[i]` is either `'H'` or `'.'`.

__题目描述:__

给你一个下标从 __0__ 开始的字符串 `hamsters` ，其中 `hamsters[i]` 要么是:

- `'H'` 表示有一个仓鼠在下标 `i` ，或者
- `'.'` 表示下标 `i` 是空的。

你将要在空的位置上添加一定数量的食物桶来喂养仓鼠。如果仓鼠的左边或右边至少有一个食物桶，就可以喂食它。更正式地说，如果你在位置 `i - 1` __或者__ `i + 1` 放置一个食物桶，就可以喂养位置为 `i` 处的仓鼠。

在 __空的位置__ 放置食物桶以喂养所有仓鼠的前提下，请你返回需要的 __最少__ 食物桶数。如果无解请返回 `-1` 。

__示例:__

示例 1：

![2086-4](https://pic.leetcode.cn/1710141378-bfEGUX-image.png)

```text
输入：hamsters = "H..H"
输出：2
解释：
我们可以在下标为 1 和 2 处放食物桶。
可以发现如果我们只放置 1 个食物桶，其中一只仓鼠将得不到喂养。
```

示例 2：

![2086-5](https://pic.leetcode.cn/1710141384-oLAScv-image.png)

```text
输入：street = ".H.H."
输出：1
解释：
我们可以在下标为 2 处放置一个食物桶。
```

示例 3：

```text
输入：street = ".HHH."
输出：-1
解释：
如果我们如图那样在每个空位放置食物桶，下标 2 处的仓鼠将吃不到食物。
```

__提示：__

- `1 <= hamsters.length <= 10 ^ 5`
- `hamsters[i]` 要么是 `'H'` ，要么是 `'.'` 。

__思路:__

```text
模拟
从左往右依次遍历
如果当前位置是仓鼠, 尝试放置一个桶在它的右边, 由于右边的桶可以喂食更右边的仓鼠, i += 2
如果右边不能放桶, 尝试放在左边
如果都不能放, 返回 -1
最后返回放置的桶数
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int minimumBuckets(string hamsters) 
    {
        int n = hamsters.size(), result = 0;
        for (int i = 0; i < n; i++) 
        {
            if (hamsters[i] == 'H') 
            {
                if (i + 1 < n and hamsters[i + 1] == '.') i += 2;
                else if (!i or hamsters[i - 1] != '.') return -1;
                ++result;
            }
        }
        return result;  
    }
};
```

__Java__:

```Java
class Solution {
    public int minimumBuckets(String hamsters) {
        int n = hamsters.length(), result = 0;
        for (int i = 0; i < n; i++) {
            if (hamsters.charAt(i) == 'H') {
                if (i + 1 < n && hamsters.charAt(i + 1) == '.') i += 2;
                else if (i == 0 || hamsters.charAt(i - 1) != '.') return -1;
                ++result;
            }
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def minimumBuckets(self, hamsters: str) -> int:
        i, n, result = 0, len(hamsters), 0
        while i < n:
            if hamsters[i] == 'H':
                if i + 1 < n and hamsters[i + 1] == '.':
                    result += 1
                    i += 2
                elif i and hamsters[i - 1] == '.':
                    result += 1
                else:
                    return -1
            i += 1
        return result
```
