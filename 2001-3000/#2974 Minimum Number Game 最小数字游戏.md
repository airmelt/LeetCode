# 2974 Minimum Number Game 最小数字游戏

__Description:__

You are given a __0-indexed__ integer array `nums` of __even__ length and there is also an empty array `arr`. Alice and Bob decided to play a game where in every round Alice and Bob will do one move. The rules of the game are as follows:

- Every round, first Alice will remove the __minimum__ element from `nums`, and then Bob does the same.
- Now, first Bob will append the removed element in the array `arr`, and then Alice does the same.
- The game continues until `nums` becomes empty.

Return _the resulting array_ `arr`.

__Example:__

Example 1:

```text
Input: nums = [5,4,2,3]
Output: [3,2,5,4]
Explanation: In round one, first Alice removes 2 and then Bob removes 3. Then in arr firstly Bob appends 3 and then Alice appends 2. So arr = [3,2].
At the begining of round two, nums = [5,4]. Now, first Alice removes 4 and then Bob removes 5. Then both append in arr which becomes [3,2,5,4].
```

Example 2:

```text
Input: nums = [2,5]
Output: [5,2]
Explanation: In round one, first Alice removes 2 and then Bob removes 5. Then in arr firstly Bob appends and then Alice appends. So arr = [5,2].
```

__Constraints:__

- `2 <= nums.length <= 100`
- `1 <= nums[i] <= 100`
- `nums.length % 2 == 0`

__题目描述:__

你有一个下标从 __0__ 开始、长度为 __偶数__ 的整数数组 `nums` ，同时还有一个空数组 `arr` 。Alice 和 Bob 决定玩一个游戏，游戏中每一轮 Alice 和 Bob 都会各自执行一次操作。游戏规则如下:

- 每一轮，Alice 先从 `nums` 中移除一个 __最小__ 元素，然后 Bob 执行同样的操作。
- 接着，Bob 会将移除的元素添加到数组 `arr` 中，然后 Alice 也执行同样的操作。
- 游戏持续进行，直到 `nums` 变为空。

返回结果数组 `arr` 。

__示例:__

示例 1：

```text
输入：nums = [5,4,2,3]
输出：[3,2,5,4]
解释：第一轮，Alice 先移除 2 ，然后 Bob 移除 3 。然后 Bob 先将 3 添加到 arr 中，接着 Alice 再将 2 添加到 arr 中。于是 arr = [3,2] 。
第二轮开始时，nums = [5,4] 。Alice 先移除 4 ，然后 Bob 移除 5 。接着他们都将元素添加到 arr 中，arr 变为 [3,2,5,4] 。
```

示例 2：

```text
输入：nums = [2,5]
输出：[5,2]
解释：第一轮，Alice 先移除 2 ，然后 Bob 移除 5 。然后 Bob 先将 5 添加到 arr 中，接着 Alice 再将 2 添加到 arr 中。于是 arr = [5,2] 。
```

__提示：__

- `1 <= nums.length <= 100`
- `1 <= nums[i] <= 100`
- `nums.length % 2 == 0`

__思路:__

```text
贪心
先排序
然后两两交换元素
时间复杂度为 O(NlogN), 空间复杂度为 O(logN)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<int> numberGame(vector<int>& nums) 
    {
        sort(nums.begin(), nums.end());
        for (int i = 1, n = nums.size(); i < n; i += 2) swap(nums[i - 1], nums[i]);
        return nums;
    }
};
```

__Java__:

```Java
class Solution {
    public int[] numberGame(int[] nums) {
        Arrays.sort(nums);
        for (int i = 1, n = nums.length; i < n; i += 2) {
            nums[i] ^= nums[i - 1];
            nums[i - 1] ^= nums[i];
            nums[i] ^= nums[i - 1];
        }
        return nums;
    }
}
```

__Python__:

```Python
class Solution:
    def numberGame(self, nums: List[int]) -> List[int]:
        return list(chain.from_iterable([[x, y] for x, y in zip(sorted(nums)[1::2], sorted(nums)[::2])]))
```
