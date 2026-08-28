# 3147 Taking Maximum Energy From the Mystic Dungeon 从魔法师身上吸取的最大能量

__Description:__

In a mystic dungeon, `n` magicians are standing in a line. Each magician has an attribute that gives you energy. Some magicians can give you negative energy, which means taking energy from you.

You have been cursed in such a way that after absorbing energy from magician `i`, you will be instantly transported to magician `(i + k)`. This process will be repeated until you reach the magician where `(i + k)` does not exist.

In other words, you will choose a starting point and then teleport with `k` jumps until you reach the end of the magicians' sequence, absorbing all the energy during the journey.

You are given an array `energy` and an integer `k`. Return the __maximum__ possible energy you can gain.

__Note__ that when you reach a magician, you _must_ take energy from them, whether it is negative or positive energy.

__Example:__

Example 1:

```text
Input: energy = [5,2,-10,-5,1], k = 3

Output: 3

Explanation: We can gain a total energy of 3 by starting from magician 1 absorbing 2 + 1 = 3.
```

Example 2:

```text
Input: energy = [-2,-3,-1], k = 2

Output: -1

Explanation: We can gain a total energy of -1 by starting from magician 2.
```

__Constraints:__

- `1 <= energy.length <= 10 ^ 5`
- `-1000 <= energy[i] <= 1000`
- `1 <= k <= energy.length - 1`

__题目描述:__

在神秘的地牢中，`n` 个魔法师站成一排。每个魔法师都拥有一个属性，这个属性可以给你提供能量。有些魔法师可能会给你负能量，即从你身上吸取能量。

你被施加了一种诅咒，当你从魔法师 `i` 处吸收能量后，你将被立即传送到魔法师 `(i + k)` 处。这一过程将重复进行，直到你到达一个不存在 `(i + k)` 的魔法师为止。

换句话说，你将选择一个起点，然后以 `k` 为间隔跳跃，直到到达魔法师序列的末端，在过程中吸收所有的能量。

给定一个数组 `energy` 和一个整数 `k`，返回你能获得的 __最大__ 能量。

__示例:__

示例 1：

```text
输入： energy = [5,2,-10,-5,1], k = 3

输出： 3

解释：可以从魔法师 1 开始，吸收能量 2 + 1 = 3。
```

示例 2：

```text
输入： energy = [-2,-3,-1], k = 2

输出： -1

解释：可以从魔法师 2 开始，吸收能量 -1。
```

__提示：__

- `1 <= energy.length <= 10 ^ 5`
- `-1000 <= energy[i] <= 1000`
- `1 <= k <= energy.length - 1`

__思路:__

```text
贪心
由于必须要一直加到数组末尾
可以直接从数组末尾开始反推
直接操作原数组
最后返回数组最大值即可
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int maximumEnergy(vector<int>& energy, int k) 
    {
        for (int i = energy.size() - k - 1; i > -1; i--) energy[i] += energy[i + k];
        return *max_element(energy.begin(), energy.end());
    }
};
```

__Java__:

```Java
class Solution {
    public int maximumEnergy(int[] energy, int k) {
        for (int i = energy.length - k - 1; i > -1; i--) energy[i] += energy[i + k];
        return Arrays.stream(energy).max().getAsInt();
    }
}
```

__Python__:

```Python
class Solution:
    def maximumEnergy(self, energy: List[int], k: int) -> int:
        for i in range(len(energy) - k - 1, -1, -1):
            energy[i] += energy[i + k]
        return max(energy)
```
