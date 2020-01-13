__题目描述__:
小A 和 小B 在玩猜数字。小B 每次从 1, 2, 3 中随机选择一个，小A 每次也从 1, 2, 3 中选择一个猜。他们一共进行三次这个游戏，请返回 小A 猜对了几次？

输入的guess数组为 小A 每次的猜测，answer数组为 小B 每次的选择。guess和answer的长度都等于3。

__示例 :__
示例 1：

输入：guess = [1,2,3], answer = [1,2,3]
输出：3
解释：小A 每次都猜对了。

示例 2：

输入：guess = [2,2,3], answer = [3,2,1]
输出：1
解释：小A 只猜对了第二次。

__限制：__

guess的长度 = 3
answer的长度 = 3
guess的元素取值为 {1, 2, 3} 之一。
answer的元素取值为 {1, 2, 3} 之一。

__思路__:
逐个判断, 相等就转化成 1否则为 0, 再全部求和即可
时间复杂度O(n), 空间复杂度O(1)

__代码__:
__C++__:
```C++
class Solution 
{
public:
    int game(vector<int>& guess, vector<int>& answer) 
    {
        int result = 0;
        for (int i = 0; i < guess.size(); i++) if (guess[i] == answer[i]) ++result;
        return result;
    }
};
```

__Java__:
```Java
class Solution {
    public int game(int[] guess, int[] answer) {
        return (guess[0] == answer[0] ? 1 : 0) + (int)(guess[1] == answer[1] ? 1 : 0) + (int)(guess[2] == answer[2] ? 1 : 0);
    }
}
```

__Python__:
```Python
class Solution:
    def game(self, guess: List[int], answer: List[int]) -> int:
        return (guess[0] == answer[0]) + (guess[1] == answer[1]) + (guess[2] == answer[2])
```