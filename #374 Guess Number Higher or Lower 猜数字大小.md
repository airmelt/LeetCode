# 374 Guess Number Higher or Lower 猜数字大小

__Description__:
We are playing the Guess Game. The game is as follows:

I pick a number from 1 to n. You have to guess which number I picked.

Every time you guess wrong, I'll tell you whether the number is higher or lower.

You call a pre-defined API guess(int num) which returns 3 possible results (-1, 1, or 0):

```text
-1 : My number is lower
 1 : My number is higher
 0 : Congrats! You got it!
```

**Example :**

Input: n = 10, pick = 6
Output: 6

__题目描述__:
我们正在玩一个猜数字游戏。 游戏规则如下：
我从 1 到 n 选择一个数字。 你需要猜我选择了哪个数字。
每次你猜错了，我会告诉你这个数字是大了还是小了。
你调用一个预先定义好的接口 guess(int num)，它会返回 3 个可能的结果（-1，1 或 0）：

```text
-1 : 我的数字比较小
 1 : 我的数字比较大
 0 : 恭喜！你猜对了！
```

**示例 :**

输入: n = 10, pick = 6
输出: 6

__思路__:

二分法
时间复杂度O(logn), 空间复杂度O(1)

__代码__:
__C++__:

```C++
// Forward declaration of guess API.
// @param num, your guess
// @return -1 if my number is lower, 1 if my number is higher, otherwise return 0
int guess(int num);

class Solution 
{
public:
    int guessNumber(int n) 
    {
        int low = 1;
        while (low <= n) 
        {
            int mid = low + ((n - low) >> 1);
            if (!guess(mid)) return mid;
            else if (guess(mid) > 0) low = mid + 1;
            else n = mid - 1;
        }
        return low;
    }
};
```

__Java__:

```Java
/* The guess API is defined in the parent class GuessGame.
   @param num, your guess
   @return -1 if my number is lower, 1 if my number is higher, otherwise return 0
      int guess(int num); */

public class Solution extends GuessGame {
    public int guessNumber(int n) {
        int low = 1;
        while (low <= n) {
            int mid = low + ((n - low) >> 1);
            if (guess(mid) == 0) return mid;
            else if (guess(mid) > 0) low = mid + 1;
            else n = mid - 1;
        }
        return low;
    }
}
```

__Python__:

```Python
# The guess API is already defined for you.
# @param num, your guess
# @return -1 if my number is lower, 1 if my number is higher, otherwise return 0
# def guess(num):

class Solution(object):
    def guessNumber(self, n: int) -> int:
        low = 1
        while low <= n:
            mid = low + ((n - low) >> 1)
            if not guess(mid):
                return mid
            elif guess(mid) > 0:
                low = mid + 1
            else:
                n = mid - 1
        return low
```
