__Description__:
X is a good number if after rotating each digit individually by 180 degrees, we get a valid number that is different from X.  Each digit must be rotated - we cannot choose to leave it alone.

A number is valid if each digit remains a digit after rotation. 0, 1, and 8 rotate to themselves; 2 and 5 rotate to each other; 6 and 9 rotate to each other, and the rest of the numbers do not rotate to any other number and become invalid.

Now given a positive number N, how many numbers X from 1 to N are good?

__Example:__
Input: 10
Output: 4
Explanation: 
There are four good numbers in the range [1, 10] : 2, 5, 6, 9.
Note that 1 and 10 are not good numbers, since they remain unchanged after rotating.

__Note:__

N  will be in range [1, 10000].

__题目描述__:
我们称一个数 X 为好数, 如果它的每位数字逐个地被旋转 180 度后，我们仍可以得到一个有效的，且和 X 不同的数。要求每位数字都要被旋转。

如果一个数的每位数字被旋转以后仍然还是一个数字， 则这个数是有效的。0, 1, 和 8 被旋转后仍然是它们自己；2 和 5 可以互相旋转成对方；6 和 9 同理，除了这些以外其他的数字旋转以后都不再是有效的数字。

现在我们有一个正整数 N, 计算从 1 到 N 中有多少个数 X 是好数？

__示例 :__
输入: 10
输出: 4
解释: 
在[1, 10]中有四个好数： 2, 5, 6, 9。
注意 1 和 10 不是好数, 因为他们在旋转之后不变。

__注意：__

N 的取值范围是 [1, 10000]。

__思路__:
1. 动态规划, 一个整数可以划分为 i % 10和 i / 10, 如果这两个数中存在3, 4, 7, 则一定不是好数; 如果这两个数中有2, 5, 6, 9则一定是好数
时间复杂度O(n), 空间复杂度O(n)
2. 对每一个数计算, 只要有3, 4, 7一定不是好数, 至少要有一位数是 2, 5, 6, 9
时间复杂度O(nd), 空间复杂度O(1), 其中 d表示数字的位数
3. 转化成字符串, 其中, 字符串中不能有3, 4, 7, 至少要有一位数是 2, 5, 6, 9
时间复杂度O(nd), 空间复杂度O(n), 其中 d表示数字的位数

__代码__:
__C++__:
```
class Solution {
public:
    int rotatedDigits(int N) {
        int result = 0;
        vector<int> dp(N + 1, 0);
        for (int i = 1; i <= N; i++) {
            if (i == 3 || i == 4 || i == 7 ||
                dp[i % 10] == -1 || dp[i / 10] == -1) dp[i] = -1;
            else if (i == 2 || i == 5 || i == 6 || i == 9 ||
                dp[i % 10] == 1 || dp[i / 10] == 1) {
                dp[i] = 1;
                result++;
            }
        }
        return result;
    }
};
```

__Java__:
```
class Solution {
    public int rotatedDigits(int N) {
        int result = 0;
        for (int i = 1; i < N + 1; i++) if (isGood(i)) result++;
        return result;
    }
    
    private boolean isGood(int i) {
        int count = 0;
        while (i != 0) {
            int dig = i % 10;
            if (dig == 3 || dig == 4 || dig == 7) return false;
            if (dig == 2 || dig == 5 || dig == 6 || dig == 9) count++;
            i /= 10;
        }
        return count == 0 ? false : true;
    }
}
```

__Python__:
```
class Solution:
    def rotatedDigits(self, N: int) -> int:
        return len([i for i in range(1, N + 1) if not any([d for d in str(i) if int(d) in (3, 4, 7)]) and any([d for d in str(i) if int(d) in (2, 5, 6, 9)])])
```