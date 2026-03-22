# 2383 Minimum Hours of Training to Win a Competition 赢得比赛需要的最少训练时长

__Description:__

You are entering a competition, and are given two __positive__ integers `initialEnergy` and `initialExperience` denoting your initial energy and initial experience respectively.

You are also given two __0-indexed__ integer arrays `energy` and `experience`, both of length `n`.

You will face `n` opponents __in order__. The energy and experience of the `i ^ th` opponent is denoted by `energy[i]` and `experience[i]` respectively. When you face an opponent, you need to have both __strictly__ greater experience and energy to defeat them and move to the next opponent if available.

Defeating the `i ^ th` opponent __increases__ your experience by `experience[i]`, but __decreases__ your energy by `energy[i]`.

Before starting the competition, you can train for some number of hours. After each hour of training, you can either choose to increase your initial experience by one, or increase your initial energy by one.

Return _the __minimum__ number of training hours required to defeat all_ `n` _opponents_.

__Example:__

Example 1:

```text
Input: initialEnergy = 5, initialExperience = 3, energy = [1,4,3,2], experience = [2,6,3,1]
Output: 8
Explanation: You can increase your energy to 11 after 6 hours of training, and your experience to 5 after 2 hours of training.
You face the opponents in the following order:
```

- You have more energy and experience than the 0th opponent so you win.
  Your energy becomes 11 - 1 = 10, and your experience becomes 5 + 2 = 7.
- You have more energy and experience than the 1st opponent so you win.
  Your energy becomes 10 - 4 = 6, and your experience becomes 7 + 6 = 13.
- You have more energy and experience than the 2nd opponent so you win.
  Your energy becomes 6 - 3 = 3, and your experience becomes 13 + 3 = 16.
- You have more energy and experience than the 3rd opponent so you win.
  Your energy becomes 3 - 2 = 1, and your experience becomes 16 + 1 = 17.

You did a total of 6 + 2 = 8 hours of training before the competition, so we return 8.

It can be proven that no smaller answer exists.

Example 2:

```text
Input: initialEnergy = 2, initialExperience = 4, energy = [1], experience = [3]
Output: 0
Explanation: You do not need any additional energy or experience to win the competition, so we return 0.
```

__Constraints:__

- `n == energy.length == experience.length`
- `1 <= n <= 100`
- `1 <= initialEnergy, initialExperience, energy[i], experience[i] <= 100`

__题目描述:__

你正在参加一场比赛，给你两个 __正__ 整数 `initialEnergy` 和 `initialExperience` 分别表示你的初始精力和初始经验。

另给你两个下标从 __0__ 开始的整数数组 `energy` 和 `experience`，长度均为 `n` 。

你将会 __依次__ 对上 `n` 个对手。第 `i` 个对手的精力和经验分别用 `energy[i]` 和 `experience[i]` 表示。当你对上对手时，需要在经验和精力上都 __严格__ 超过对手才能击败他们，然后在可能的情况下继续对上下一个对手。

击败第 `i` 个对手会使你的经验 __增加__ `experience[i]`，但会将你的精力 __减少__  `energy[i]` 。

在开始比赛前，你可以训练几个小时。每训练一个小时，你可以选择将增加经验增加 1 或者 将精力增加 1 。

返回击败全部 `n` 个对手需要训练的 __最少__ 小时数目。

__示例:__

示例 1：

```text
输入：initialEnergy = 5, initialExperience = 3, energy = [1,4,3,2], experience = [2,6,3,1]
输出：8
解释：在 6 小时训练后，你可以将精力提高到 11 ，并且再训练 2 个小时将经验提高到 5 。
按以下顺序与对手比赛：
```

- 你的精力与经验都超过第 0 个对手，所以获胜。
  精力变为：11 - 1 = 10 ，经验变为：5 + 2 = 7 。
- 你的精力与经验都超过第 1 个对手，所以获胜。
  精力变为：10 - 4 = 6 ，经验变为：7 + 6 = 13 。
- 你的精力与经验都超过第 2 个对手，所以获胜。
  精力变为：6 - 3 = 3 ，经验变为：13 + 3 = 16 。
- 你的精力与经验都超过第 3 个对手，所以获胜。
  精力变为：3 - 2 = 1 ，经验变为：16 + 1 = 17 。

在比赛前进行了 8 小时训练，所以返回 8 。

可以证明不存在更小的答案。

示例 2：

```text
输入：initialEnergy = 2, initialExperience = 4, energy = [1], experience = [3]
输出：0
解释：你不需要额外的精力和经验就可以赢得比赛，所以返回 0 。
```

__提示：__

- `n == energy.length == experience.length`
- `1 <= n <= 100`
- `1 <= initialEnergy, initialExperience, energy[i], experience[i] <= 100`

__思路:__

```text
贪心
每次比赛时，我们需要保证精力和经验都要超过对手
首先先训练到刚好超过对手的精力
如果训练了, 精力变为 energy[i] + 1, 训练时间加上 energy[i] + 1 - initialEnergy
用初始精力减去 energy[i]
然后训练到刚好超过对手的经验
如果训练了, 经验变为 experience[i] + 1, 训练时间加上 experience[i] + 1 - initialExperience
用初始经验加上 experience[i]
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int minNumberOfHours(int initialEnergy, int initialExperience, vector<int>& energy, vector<int>& experience) 
    {
        int result = 0, n = energy.size();
        for (int i = 0; i < n; i++) 
        {
            if (initialEnergy <= energy[i]) result += energy[i] + 1 - initialEnergy;
            initialEnergy = max(1, initialEnergy - energy[i]);
            if (initialExperience <= experience[i]) result += experience[i] + 1 - initialExperience;
            initialExperience = initialExperience <= experience[i] ? (experience[i] << 1) + 1 : initialExperience + experience[i];
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int minNumberOfHours(int initialEnergy, int initialExperience, int[] energy, int[] experience) {
        int result = 0, n = energy.length;
        for (int i = 0; i < n; i++) {
            if (initialEnergy <= energy[i]) result += energy[i] + 1 - initialEnergy;
            initialEnergy = Math.max(1, initialEnergy - energy[i]);
            if (initialExperience <= experience[i]) result += experience[i] + 1 - initialExperience;
            initialExperience = initialExperience <= experience[i] ? (experience[i] << 1) + 1 : initialExperience + experience[i];
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def minNumberOfHours(self, initialEnergy: int, initialExperience: int, energy: List[int], experience: List[int]) -> int:
        return max(0, max([j - i for i, j in zip(list(accumulate(experience, initial=initialExperience))[:-1], experience)]) + 1) + max(0, 1 + sum(energy) - initialEnergy)
```
