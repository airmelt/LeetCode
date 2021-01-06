# 421 Maximum XOR of Two Numbers in an Array 数组中两个数的最大异或值

__Description__:
Given an integer array nums, return the maximum result of nums[i] XOR nums[j], where 0 ≤ i ≤ j < n.

__Follow up:__
Could you do this in O(n) runtime?

__Example:__

Example 1:

Input: nums = [3,10,5,25,2,8]
Output: 28
Explanation: The maximum result is 5 XOR 25 = 28.

Example 2:

Input: nums = [0]
Output: 0

Example 3:

Input: nums = [2,4]
Output: 6

Example 4:

Input: nums = [8,10,2]
Output: 10

Example 5:

Input: nums = [14,70,53,83,49,91,36,80,92,51,66,70]
Output: 127

__Constraints:__

1 <= nums.length <= 2 * 10^4
0 <= nums[i] <= 2^31 - 1

__题目描述__:
给定一个非空数组，数组中元素为 a0, a1, a2, … , an-1，其中 0 ≤ ai < 2^31 。

找到 ai 和aj 最大的异或 (XOR) 运算结果，其中0 ≤ i,  j < n 。

你能在O(n)的时间解决这个问题吗？

__示例 :__

输入: [3, 10, 5, 25, 2, 8]

输出: 28

解释: 最大的结果是 5 ^ 25 = 28.

__思路__:

前缀树/哈希表
异或的一个性质: a ^ b = c -> a ^ c = b & b ^ c = a
那么就转化为二进制找高位 1, 越多越好
将异或值存储在表中, 用掩码查找该位是否能为 1即可
时间复杂度O(n), 空间复杂度O(n)

__代码__:
__C++__:

```C++
struct Node
{
    Node* next[2] = { nullptr };
};
class Solution 
{
private:
    void insert(int num, Node* root) 
    {
        for (int i = 30; i >= 0; i--) 
        {
            int j = (num >> i & 1);
            if (!root -> next[j]) root -> next[j] = new Node();
            root = root -> next[j];
        }
    }
public:
    int findMaximumXOR(vector<int>& nums) 
    {
        Node* root = new Node();
        for (auto num : nums) insert(num, root);
        int result = 0, cur = 0;
        Node* p = root;
        for (auto num : nums) 
        {
            p = root; 
            cur = 0;
            for (int i = 30; i >= 0; i--) 
            {
                int j = (num >> i) & 1;
                if (p -> next[!j]) 
                {
                    p = p -> next[!j];
                    cur += (1 << i);
                }
                else p = p -> next[j];
            }
            result = max(result, cur);
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int findMaximumXOR(int[] nums) {
        int result = 0, cur = 0, mask = 0;
        for (int i = 30; i >= 0; i--) {
            mask = mask | (1 << i);
            Set<Integer> set = new HashSet<>();
            for (int num : nums) set.add(num & mask);
            cur = result | (1 << i);
            for (Integer j : set) {
                if (set.contains(j ^ cur)) {
                    result = cur;
                    break;
                }
            }
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def findMaximumXOR(self, nums: List[int]) -> int:
        l, result = len(bin(max(nums))[2:]), 0
        for i in range(l)[::-1]:
            result <<= 1
            cur = result | 1
            d = {num >> i for num in nums}
            result |= any(cur ^ j in d for j in d)     
        return result
```
