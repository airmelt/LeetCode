# 1803 Count Pairs With XOR in a Range 统计异或值在范围内的数对有多少

__Description:__

Given a __(0-indexed)__ integer array `nums` and two integers `low` and `high`, return _the number of __nice pairs___.

A __nice pair__ is a pair `(i, j)` where `0 <= i < j < nums.length` and `low <= (nums[i] XOR nums[j]) <= high`.

__Example:__

Example 1:

```text
Input: nums = [1,4,2,7], low = 2, high = 6
Output: 6
Explanation: All nice pairs (i, j) are as follows:
    - (0, 1): nums[0] XOR nums[1] = 5 
    - (0, 2): nums[0] XOR nums[2] = 3
    - (0, 3): nums[0] XOR nums[3] = 6
    - (1, 2): nums[1] XOR nums[2] = 6
    - (1, 3): nums[1] XOR nums[3] = 3
    - (2, 3): nums[2] XOR nums[3] = 5
```

Example 2:

```text
Input: nums = [9,8,4,2,1], low = 5, high = 14
Output: 8
Explanation: All nice pairs (i, j) are as follows:
    - (0, 2): nums[0] XOR nums[2] = 13
    - (0, 3): nums[0] XOR nums[3] = 11
    - (0, 4): nums[0] XOR nums[4] = 8
    - (1, 2): nums[1] XOR nums[2] = 12
    - (1, 3): nums[1] XOR nums[3] = 10
    - (1, 4): nums[1] XOR nums[4] = 9
    - (2, 3): nums[2] XOR nums[3] = 6
    - (2, 4): nums[2] XOR nums[4] = 5
```

__Constraints:__

- `1 <= nums.length <= 2 * 10 ^ 4`
- `1 <= nums[i] <= 2 * 10 ^ 4`
- `1 <= low <= high <= 2 * 10 ^ 4`

__题目描述:__

给你一个整数数组 `nums` （下标 __从 0 开始__ 计数）以及两个整数: `low` 和 `high` ，请返回 __漂亮数对__ 的数目。

__漂亮数对__ 是一个形如 `(i, j)` 的数对，其中 `0 <= i < j < nums.length` 且 `low <= (nums[i] XOR nums[j]) <= high` 。

__示例:__

示例 1：

```text
输入：nums = [1,4,2,7], low = 2, high = 6
输出：6
解释：所有漂亮数对 (i, j) 列出如下：
    - (0, 1): nums[0] XOR nums[1] = 5 
    - (0, 2): nums[0] XOR nums[2] = 3
    - (0, 3): nums[0] XOR nums[3] = 6
    - (1, 2): nums[1] XOR nums[2] = 6
    - (1, 3): nums[1] XOR nums[3] = 3
    - (2, 3): nums[2] XOR nums[3] = 5
```

示例 2：

```text
输入：nums = [9,8,4,2,1], low = 5, high = 14
输出：8
解释：所有漂亮数对 (i, j) 列出如下：
    - (0, 2): nums[0] XOR nums[2] = 13
    - (0, 3): nums[0] XOR nums[3] = 11
    - (0, 4): nums[0] XOR nums[4] = 8
    - (1, 2): nums[1] XOR nums[2] = 12
    - (1, 3): nums[1] XOR nums[3] = 10
    - (1, 4): nums[1] XOR nums[4] = 9
    - (2, 3): nums[2] XOR nums[3] = 6
    - (2, 4): nums[2] XOR nums[4] = 5
```

__提示：__

- `1 <= nums.length <= 2 * 10 ^ 4`
- `1 <= nums[i] <= 2 * 10 ^ 4`
- `1 <= low <= high <= 2 * 10 ^ 4`

__思路:__

```text
前缀树
统计在 [low, high] 中的数对转化为统计 f(x) 为 [0, x] 中的数对, 然后再用 f(high + 1) - f(low) 即可
由于数组中的元素范围为 [1, 20000], 最多用 15 位二进制就可以存下
将数字转化为二进制插入到前缀树中, 并给每一个结点记录计数值
从高位向低位遍历, 如果 nums[i] ^ nums[j] <= x:
对每一位的结果 nums[i] ^ nums[j] = k
1. k_i < x_i, 那么低位肯定更加满足, 结果加上该位的计数值
2. k_i == x_i, 需要继续遍历下一位才能确定
3. k_i > x_i, 低位一定不满足 
也就是说如果 x 当前位是 1, 直接加上 k 与当前位相同的值然后移动到另一边继续遍历, 否则移动到与 x 当前位相同的下一位
时间复杂度为 O(NlogM), 空间复杂度为 O(NlogM), 其中 M 为数组元素值的范围
```

__代码:__

__C++__:

```C++
struct Trie
{
    int count;
    Trie* child[2];
};

class Solution {
public:
    int countPairs(vector<int>& nums, int low, int high) {
        root = new Trie;
        init(root);
        int result = 0;
        for (const auto& num : nums) 
        {
            result += helper(root, num, high + 1) - helper(root, num, low);
            insert(root, num);
        }
        return result;
    }
private:
    Trie* root;
    void init(Trie* root)
    {
        root -> count = 0;
        root -> child[0] = root -> child[1] = nullptr;
    }

    void insert(Trie* root, int n) 
    {
        for (int i = 15; i > -1; i--) 
        {
            int x = bool(n & (1 << i));
            if (!root -> child[x]) 
            {
                root -> child[x] = new Trie;
                init(root -> child[x]);
            }
            ++root -> child[x] -> count;
            root = root -> child[x];
        }
    }

    int helper(Trie* root, int cur, int x) 
    {
        int result = 0;
        for (int i = 15; i > -1; i--) 
        {
            if (!root) break;
            int k = bool(cur & (1 << i));
            if (x & (1 << i)) 
            {
                if (root -> child[k]) result += root -> child[k] -> count;
                root = root -> child[1 - k];
            } 
            else root = root -> child[k];
        }
        return result;
    }
};
```

__Java__:

```Java
class Trie {
    public int count = 0;
    public Trie[] child = new Trie[2];
}

class Solution {
    public int countPairs(int[] nums, int low, int high) {
        Trie root = new Trie();
        int result = 0;
        for (int num : nums) {
            result += helper(root, num, high + 1) - helper(root, num, low);
            insert(root, num);
        }
        return result;
    }

    private void insert(Trie root, int n) {
        for (int i = 15; i > -1; i--) {
            int x = (n & (1 << i)) == 0 ? 0 : 1;
            if (root.child[x] == null) root.child[x] = new Trie();
            ++root.child[x].count;
            root = root.child[x];
        }
    }

    private int helper(Trie root, int cur, int x) {
        int result = 0;
        for (int i = 15; i > -1; i--) {
            if (root == null) break;
            int k = (cur & (1 << i)) == 0 ? 0 : 1;
            if ((x & (1 << i)) != 0) {
                if (root.child[k] != null) result += root.child[k].count;
                root = root.child[1 - k];
            } else root = root.child[k];
        }
        return result;
    }
}
```

__Python__:

```Python
class Trie:
    def __init__(self):        
        self.child = [None, None]
        self.count = 0
    
class Solution:
    def countPairs(self, nums: List[int], low: int, high: int) -> int:
        def insert(root: Optional[Trie], n: int) -> NoReturn:
            for i in range(15, -1, -1):
                x = bool((n) & (1 << i))
                if not root.child[x]:
                    root.child[x] = Trie()
                root.child[x].count += 1
                root = root.child[x]

        def helper(root: Optional[Trie], cur: int, x: int) -> int:
            result = 0
            for i in range(15, -1, -1):       
                if not root:
                    break                       
                k = bool(cur & (1 << i))
                if x & (1 << i):
                    if root.child[k]:
                        result += root.child[k].count
                    root = root.child[1 - k]
                else:
                    root = root.child[k]
            return result
        root = Trie()
        result = 0
        for num in nums:
            result += helper(root, num, high + 1) - helper(root, num, low)
            insert(root, num)
        return result
```
