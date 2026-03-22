# 493 Reverse Pairs 翻转对

__Description__:
Given an array nums, we call (i, j) an important reverse pair if i < j and nums[i] > 2*nums[j].

You need to return the number of important reverse pairs in the given array.

__Example:__

Example1:

Input: [1,3,2,3,1]
Output: 2

Example2:

Input: [2,4,3,5,1]
Output: 3

__Note:__
The length of the given array will not exceed 50,000.
All the numbers in the input array are in the range of 32-bit integer.

__题目描述__:
给定一个数组 nums ，如果 i < j 且 nums[i] > 2*nums[j] 我们就将 (i, j) 称作一个重要翻转对。

你需要返回给定数组中的重要翻转对的数量。

__示例 :__

示例 1:

输入: [1,3,2,3,1]
输出: 2

示例 2:

输入: [2,4,3,5,1]
输出: 3

__注意:__

给定数组的长度不会超过50000。
输入数组中的所有数字都在32位整数的表示范围内。

__思路__:

采用树状数组
用一个 map记录下数组元素和对应的下标
树状数组中的 query(val)可以查找 [1, val]中的数量, 先找出 [1, nums[i] * 2]的数量, 然后与[1, max(nums) * 2]的数量做差, 即可得到当前元素的翻转对的数量
时间复杂度O(nlgn), 空间复杂度O(n)

__代码__:
__C++__:

```C++
class BIT 
{
private:
    vector<int> tree;
    int n;
public:
    BIT(int _n) : n(_n), tree(_n + 1) {}

    static constexpr int lowbit(int x) 
    {
        return x & (-x);
    }

    void update(int x, int k) 
    {
        while (x <= n) 
        {
            tree[x] += k;
            x += lowbit(x);
        }
    }

    int query(int x) const 
    {
        int result = 0;
        while (x) 
        {
            result += tree[x];
            x -= lowbit(x);
        }
        return result;
    }
};
class Solution 
{
public:
    int reversePairs(vector<int>& nums) 
    {
        set<long long> s;
        for (int num : nums) 
        {
            s.insert(num);
            s.insert((long long)num * 2);
        }
        unordered_map<long long, int> m;
        int index = 0, result = 0;
        for (long long x : s) m[x] = ++index;
        BIT bit(m.size());
        for (int num : nums) 
        {
            int left = m[(long long)num * 2], right = m.size();
            result += bit.query(right) - bit.query(left);
            bit.update(m[num], 1);
        }
        return result;
    }
};
```

__Java__:

```Java
class BIT {
    private int n, tree[];
    
    public BIT(int n) {
        this.n = n;
        this.tree = new int[n + 1];
    } 

    public int lowbit(int x) {
        return x & (-x);
    }

    public void update(int x, int k) {
        while (x <= n) {
            tree[x] += k;
            x += lowbit(x);
        }
    }

    public int query(int x) {
        int result = 0;
        while (x != 0) {
            result += tree[x];
            x -= lowbit(x);
        }
        return result;
    }
}
class Solution {
    public int reversePairs(int[] nums) {
        Set<Long> set = new TreeSet<>();
        for (int x : nums) {
            set.add((long) x);
            set.add((long) x * 2);
        }
        Map<Long, Integer> map = new HashMap<Long, Integer>();
        int index = 0, result = 0;
        for (long x : set) map.put(x, ++index);
        BIT bit = new BIT(map.size());
        for (int num : nums) {
            int left = map.get((long) num * 2), right = map.size();
            result += bit.query(right) - bit.query(left);
            bit.update(map.get((long) num), 1);
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    class BIT:
        def __init__(self, n: int) -> None:
            self.n = n
            self.tree = [0] * (n + 1)
        
        def lowbit(self, x: int) -> int:
            return x & (-x)
        
        def update(self, x: int, k: int) -> None:
            while x <= self.n:
                self.tree[x] += k
                x += self.lowbit(x)
                
        def query(self, x: int) -> int:
            result = 0
            while x:
                result += self.tree[x]
                x -= self.lowbit(x)
            return result
        
    def reversePairs(self, nums: List[int]) -> int:
        s, result = list(set(num for num in nums) | set((num << 1) for num in nums)), 0
        d = collections.defaultdict(int)
        s.sort()
        for i, x in enumerate(s):
            d[x] = i + 1
        bit = self.BIT(len(d))
        for num in nums:
            left, right = d[num << 1], len(d)
            result += bit.query(right) - bit.query(left)
            bit.update(d[num], 1)
        return result
```
