__Description__:
You are playing the following Bulls and Cows game with your friend: You write down a number and ask your friend to guess what the number is. Each time your friend makes a guess, you provide a hint that indicates how many digits in said guess match your secret number exactly in both digit and position (called "bulls") and how many digits match the secret number but locate in the wrong position (called "cows"). Your friend will use successive guesses and hints to eventually derive the secret number.

Write a function to return a hint according to the secret number and friend's guess, use A to indicate the bulls and B to indicate the cows. 

Please note that both secret number and friend's guess may contain duplicate digits.

__Example:__
Example 1:

Input: secret = "1807", guess = "7810"

Output: "1A3B"

Explanation: 1 bull and 3 cows. The bull is 8, the cows are 0, 1 and 7.

Example 2:

Input: secret = "1123", guess = "0111"

Output: "1A1B"

__Explanation:__ 
The 1st 1 in friend's guess is a bull, the 2nd or 3rd 1 is a cow.
Note: You may assume that the secret number and your friend's guess only contain digits, and their lengths are always equal.

__题目描述__:
你正在和你的朋友玩 猜数字（Bulls and Cows）游戏：你写下一个数字让你的朋友猜。每次他猜测后，你给他一个提示，告诉他有多少位数字和确切位置都猜对了（称为“Bulls”, 公牛），有多少位数字猜对了但是位置不对（称为“Cows”, 奶牛）。你的朋友将会根据提示继续猜，直到猜出秘密数字。

请写出一个根据秘密数字和朋友的猜测数返回提示的函数，用 A 表示公牛，用 B 表示奶牛。

请注意秘密数字和朋友的猜测数都可能含有重复数字。

__示例 :__
示例 1:

输入: secret = "1807", guess = "7810"

输出: "1A3B"

解释: 1 公牛和 3 奶牛。公牛是 8，奶牛是 0, 1 和 7。

示例 2:

输入: secret = "1123", guess = "0111"

输出: "1A1B"

解释: 朋友猜测数中的第一个 1 是公牛，第二个或第三个 1 可被视为奶牛。

__说明:__
你可以假设秘密数字和朋友的猜测数都只包含数字，并且它们的长度永远相等。

__思路__:
这里只有 10个数字(0-9), 可以用一个个容量为 10的数组, 分别存储每个字符串中的各个数字出现的个数, 遍历两个字符串记录相同数字出现在同一位置的个数以及数字出现的最小次数
分别遍历字符串的时候一次用加法一次用减法, 可以减少空间的使用
A的个数为相同数字出现的个数, B的个数为同一数字出现的最小次数减去 A的个数
时间复杂度O(n), 空间复杂度O(1)

__代码__:
__C++__:
```C++
class Solution {
public:
    string getHint(string secret, string guess) {
        int bucket[10] = {0}, bull = 0, cow = 0;
        for (auto c : secret) bucket[c - '0']++;
        for (int i = 0; i < guess.size(); i++) 
        {
            if (secret[i] == guess[i])
            {
                bull++;
                if (bucket[secret[i] - '0'] < 1) cow--;
                bucket[secret[i] - '0']--;
            }
            else if (bucket[guess[i] - '0'] > 0)
            {
                bucket[guess[i] - '0']--;
                cow++;
            }
        }
        return to_string(bull) + "A" + to_string(cow) + "B";
    }
};
```

__Java__:
```Java
class Solution {
    public String getHint(String secret, String guess) {
        int bucket[] = new int[10], bull = 0, cow = 0;
        for (char c : secret.toCharArray()) bucket[c - '0']++;
        for (int i = 0; i < secret.length(); i++) {
            if (secret.charAt(i) == guess.charAt(i)) {
                bull++;
                if (bucket[secret.charAt(i) - '0'] < 1) cow--;
                bucket[secret.charAt(i) - '0']--;
            } else if (bucket[guess.charAt(i) - '0'] > 0) {
                cow++;
                bucket[guess.charAt(i) - '0']--;
            }
        }
        return String.valueOf(bull) + "A" + String.valueOf(cow) + "B";
    }
}
```

__Python__:
```Python
class Solution:
    def getHint(self, secret: str, guess: str) -> str:
        return "{}A{}B".format(sum(i == j for i, j in zip(secret, guess)), sum((collections.Counter(secret) & collections.Counter(guess)).values()) - sum(i == j for i, j in zip(secret, guess)))
```