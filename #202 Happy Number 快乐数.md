__Description__:
Write an algorithm to determine if a number is "happy".

A happy number is a number defined by the following process: Starting with any positive integer, replace the number by the sum of the squares of its digits, and repeat the process until the number equals 1 (where it will stay), or it loops endlessly in a cycle which does not include 1. Those numbers for which this process ends in 1 are happy numbers.

**Example:**

Input: 19
Output: true
Explanation:
12 + 92 = 82
82 + 22 = 68
62 + 82 = 100
12 + 02 + 02 = 1

__题目描述__:
编写一个算法来判断一个数是不是“快乐数”。

一个“快乐数”定义为：对于一个正整数，每一次将该数替换为它每个位置上的数字的平方和，然后重复这个过程直到这个数变为 1，也可能是无限循环但始终变不到 1。如果可以变为 1，那么这个数就是快乐数。

**示例：**

输入: 19
输出: true
解释:
12 + 92 = 82
82 + 22 = 68
62 + 82 = 100
12 + 02 + 02 = 1

__思路__:
快乐数定义参考[快乐数 Happy number](https://en.wikipedia.org/wiki/Happy_number)
也可以自己打印循环中的数, 验证一下
不快乐数一定会进入 4 -> 16 -> 37 -> 58 -> 89 -> 145 -> 42 -> 20 -> 4循环
1. 每次计算结果加入哈希表, 判断哈希表中是否存在, 或计算结果是否为1
2. 判断计算结果为1或4
时间复杂度O(1), 空间复杂度O(1)
因为 n最大为 2 ^ 31 - 1, 在 4~5次循环后就会收敛到 1000内, 最多再计算 1000次就一定能跳出循环
采用 set空间复杂度会增加到 n

__代码__:
__C++__:
```
class Solution {
public:
    bool isHappy(int n) {
        while (n != 1 && n != 4) {
            int temp = 0;
            while (n) {
                temp += pow(n % 10, 2);
                n /= 10;
            }
            n = temp;
        }
        return n == 1;
    }
};
```

__Java__:
```
class Solution {
    public boolean isHappy(int n) {
        Set<Integer> set = new HashSet<>();
        while (n != 1) {
            int temp = 0;
            while (n != 0) {
                temp += Math.pow(n % 10, 2);
                n /= 10;
            }
            if (set.contains(temp)) return false;
            set.add(temp);
            n = temp;
        }
        return true;
    }
}
```

__Python__:
```
class Solution:
    def isHappy(self, n: int) -> bool:
        while 1:
            if n == 4:
                return False
            if n == 1:
                return True
            n = sum([int(x) ** 2 for x in str(n)])
```
