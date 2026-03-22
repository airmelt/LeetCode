# 810 Chalkboard XOR Game 黑板异或游戏

__Description__:
You are given an array of integers nums represents the numbers written on a chalkboard.

Alice and Bob take turns erasing exactly one number from the chalkboard, with Alice starting first. If erasing a number causes the bitwise XOR of all the elements of the chalkboard to become 0, then that player loses. The bitwise XOR of one element is that element itself, and the bitwise XOR of no elements is 0.

Also, if any player starts their turn with the bitwise XOR of all the elements of the chalkboard equal to 0, then that player wins.

Return true if and only if Alice wins the game, assuming both players play optimally.

__Example:__

Example 1:

Input: nums = [1,1,2]
Output: false
Explanation:
Alice has two choices: erase 1 or erase 2.
If she erases 1, the nums array becomes [1, 2]. The bitwise XOR of all the elements of the chalkboard is 1 XOR 2 = 3. Now Bob can remove any element he wants, because Alice will be the one to erase the last element and she will lose.
If Alice erases 2 first, now nums become [1, 1]. The bitwise XOR of all the elements of the chalkboard is 1 XOR 1 = 0. Alice will lose.

Example 2:

Input: nums = [0,1]
Output: true

Example 3:

Input: nums = [1,2,3]
Output: true

__Constraints:__

1 <= nums.length <= 1000
0 <= nums[i] < 2^16

__题目描述__:
黑板上写着一个非负整数数组 nums[i] 。Alice 和 Bob 轮流从黑板上擦掉一个数字，Alice 先手。如果擦除一个数字后，剩余的所有数字按位异或运算得出的结果等于 0 的话，当前玩家游戏失败。 (另外，如果只剩一个数字，按位异或运算得到它本身；如果无数字剩余，按位异或运算结果为 0。）

并且，轮到某个玩家时，如果当前黑板上所有数字按位异或运算结果等于 0，这个玩家获胜。

假设两个玩家每步都使用最优解，当且仅当 Alice 获胜时返回 true。

__示例 :__

输入: nums = [1, 1, 2]
输出: false
解释:
Alice 有两个选择: 擦掉数字 1 或 2。
如果擦掉 1, 数组变成 [1, 2]。剩余数字按位异或得到 1 XOR 2 = 3。那么 Bob 可以擦掉任意数字，因为 Alice 会成为擦掉最后一个数字的人，她总是会输。
如果 Alice 擦掉 2，那么数组变成[1, 1]。剩余数字按位异或得到 1 XOR 1 = 0。Alice 仍然会输掉游戏。

__提示:__

1 <= N <= 1000
0 <= nums[i] <= 2^16

__思路__:

异或
先判断数组个数的奇偶性
如果是偶数先手必胜, 因为要么已经获胜要么总是可以找到一个数擦去, 可以将相同的数归并到一起, 那么一定是偶数个相同的数, 或者有两个不同数, 可以擦去一个, 但是后手必须擦去另一个否则失败, 所以先手必胜
然后再把数组异或和算出来, 这时如果不是 0 先手必败
时间复杂度为 O(n), 空间复杂度为 O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    bool xorGame(vector<int>& nums) 
    {
        return !(nums.size() & 1) or !accumulate(begin(nums), end(nums), 0, bit_xor{});
    }
};
```

__Java__:

```Java
class Solution {
    public boolean xorGame(int[] nums) {
        return (nums.length & 1) != 1 || Arrays.stream(nums).reduce(0, (a, b) -> a ^ b) == 0;
    }
}
```

__Python__:

```Python
class Solution:
    def xorGame(self, nums: List[int]) -> bool:
        return not len(nums) & 1 or not functools.reduce(lambda x, y: x ^ y, nums)
```
