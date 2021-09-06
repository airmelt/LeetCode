# 822 Card Flipping Game 翻转卡片游戏

__Description__:
You are given n cards, with a positive integer printed on the front and back of each card (possibly different). You can flip any number of cards (possibly zero).

After choosing the front and the back of each card, you will pick each card, and if the integer printed on the back of this card is not printed on the front of any other card, then this integer is good.

You are given two integer array fronts and backs where fronts[i] and backs[i] are the integers printer on the front and the back of the ith card respectively.

Return the smallest good and integer after flipping the cards. If there are no good integers, return 0.

Note that a flip swaps the front and back numbers, so the value on the front is now on the back and vice versa.

__Example:__

Example 1:

Input: fronts = [1,2,4,4,7], backs = [1,3,4,1,3]
Output: 2
Explanation: If we flip the second card, the fronts are [1,3,4,4,7] and the backs are [1,2,4,1,3].
We choose the second card, which has the number 2 on the back, and it is not on the front of any card, so 2 is good.

Example 2:

Input: fronts = [1], backs = [1]
Output: 0

__Constraints:__

n == fronts.length
n == backs.length
1 <= n <= 1000
1 <= fronts[i], backs[i] <= 2000

__题目描述__:
在桌子上有 N 张卡片，每张卡片的正面和背面都写着一个正数（正面与背面上的数有可能不一样）。

我们可以先翻转任意张卡片，然后选择其中一张卡片。

如果选中的那张卡片背面的数字 X 与任意一张卡片的正面的数字都不同，那么这个数字是我们想要的数字。

哪个数是这些想要的数字中最小的数（找到这些数中的最小值）呢？如果没有一个数字符合要求的，输出 0。

其中, fronts[i] 和 backs[i] 分别代表第 i 张卡片的正面和背面的数字。

如果我们通过翻转卡片来交换正面与背面上的数，那么当初在正面的数就变成背面的数，背面的数就变成正面的数。

__示例 :__

输入：fronts = [1,2,4,4,7], backs = [1,3,4,1,3]
输出：2
解释：假设我们翻转第二张卡片，那么在正面的数变成了 [1,3,4,4,7] ， 背面的数变成了 [1,2,4,1,3]。
接着我们选择第二张卡片，因为现在该卡片的背面的数是 2，2 与任意卡片上正面的数都不同，所以 2 就是我们想要的数字。

__提示:__

1 <= fronts.length == backs.length <= 1000
1 <= fronts[i] <= 2000
1 <= backs[i] <= 2000

__思路__:

模拟
将正面和背面相同的数字加入哈希表中
分别遍历正面和背面数组, 对不在哈希表中的进行更新
时间复杂度为 O(n), 空间复杂度为 O(n)

__代码__:
__C++__:

```C++
class Solution
{
public:
    int flipgame(vector<int>& fronts, vector<int>& backs) 
    {
        unordered_set<int> s;
        int n = fronts.size(), result = 2001;
        for (int i = 0; i < n; i++) if (fronts[i] == backs[i]) s.insert(backs[i]);
        for (const auto& front : fronts) if (!s.count(front)) result = min(result, front);
        for (const auto& back : backs) if (!s.count(back)) result = min(result, back);
        return result > 2000 ? 0 : result;
    }
};
```

__Java__:

```Java
class Solution {
    public int flipgame(int[] fronts, int[] backs) {
        Set<Integer> set = new HashSet<>();
        int n = fronts.length, result = 2001;
        for (int i = 0; i < n; i++) if (fronts[i] == backs[i]) set.add(backs[i]);
        for (int front : fronts) if (!set.contains(front)) result = Math.min(result, front);
        for (int back : backs) if (!set.contains(back)) result = Math.min(result, back);
        return result > 2000 ? 0 : result;
    }
}
```

__Python__:

```Python
class Solution:
    def flipgame(self, fronts: List[int], backs: List[int]) -> int:
        s, result = { fronts[i] for i in range(len(fronts)) if fronts[i] == backs[i] }, float('inf')
        for front in fronts:
            if front not in s:
                result = min(result, front)
        for back in backs:
            if back not in s:
                result = min(result, back) 
        return result if result < float('inf') else 0
```
