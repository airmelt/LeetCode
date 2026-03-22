# 528 Random Pick with Weight 按权重随机选择

__Description__:
You are given an array of positive integers w where w[i] describes the weight of ith index (0-indexed).

We need to call the function pickIndex() which randomly returns an integer in the range [0, w.length - 1]. pickIndex() should return the integer proportional to its weight in the w array. For example, for w = [1, 3], the probability of picking the index 0 is 1 / (1 + 3) = 0.25 (i.e 25%) while the probability of picking the index 1 is 3 / (1 + 3) = 0.75 (i.e 75%).

More formally, the probability of picking index i is w[i] / sum(w).

__Example:__

Example 1:

Input
["Solution","pickIndex"]
[[[1]],[]]
Output
[null,0]

Explanation
Solution solution = new Solution([1]);
solution.pickIndex(); // return 0. Since there is only one single element on the array the only option is to return the first element.

Example 2:

Input
["Solution","pickIndex","pickIndex","pickIndex","pickIndex","pickIndex"]
[[[1,3]],[],[],[],[],[]]
Output
[null,1,1,1,1,0]

Explanation
Solution solution = new Solution([1, 3]);
solution.pickIndex(); // return 1. It's returning the second element (index = 1) that has probability of 3/4.
solution.pickIndex(); // return 1
solution.pickIndex(); // return 1
solution.pickIndex(); // return 1
solution.pickIndex(); // return 0. It's returning the first element (index = 0) that has probability of 1/4.

Since this is a randomization problem, multiple answers are allowed so the following outputs can be considered correct :
[null,1,1,1,1,0]
[null,1,1,1,1,1]
[null,1,1,1,0,0]
[null,1,1,1,0,1]
[null,1,0,1,0,0]
......
and so on.

__Constraints:__

1 <= w.length <= 10000
1 <= w[i] <= 10^5
pickIndex will be called at most 10000 times.

__题目描述__:
给定一个正整数数组 w ，其中 w[i] 代表下标 i 的权重（下标从 0 开始），请写一个函数 pickIndex ，它可以随机地获取下标 i，选取下标 i 的概率与 w[i] 成正比。

例如，对于 w = [1, 3]，挑选下标 0 的概率为 1 / (1 + 3) = 0.25 （即，25%），而选取下标 1 的概率为 3 / (1 + 3) = 0.75（即，75%）。

也就是说，选取下标 i 的概率为 w[i] / sum(w) 。

__示例 :__

示例 1：

输入：
["Solution","pickIndex"]
[[[1]],[]]
输出：
[null,0]
解释：
Solution solution = new Solution([1]);
solution.pickIndex(); // 返回 0，因为数组中只有一个元素，所以唯一的选择是返回下标 0。

示例 2：

输入：
["Solution","pickIndex","pickIndex","pickIndex","pickIndex","pickIndex"]
[[[1,3]],[],[],[],[],[]]
输出：
[null,1,1,1,1,0]
解释：
Solution solution = new Solution([1, 3]);
solution.pickIndex(); // 返回 1，返回下标 1，返回该下标概率为 3/4 。
solution.pickIndex(); // 返回 1
solution.pickIndex(); // 返回 1
solution.pickIndex(); // 返回 1
solution.pickIndex(); // 返回 0，返回下标 0，返回该下标概率为 1/4 。

由于这是一个随机问题，允许多个答案，因此下列输出都可以被认为是正确的:
[null,1,1,1,1,0]
[null,1,1,1,1,1]
[null,1,1,1,0,0]
[null,1,1,1,0,1]
[null,1,0,1,0,0]
......
诸若此类。

__提示:__

1 <= w.length <= 10000
1 <= w[i] <= 10^5
pickIndex 将被调用不超过 10000 次

__思路__:

前缀和➕ 二分查找
用一个前缀和数组记录
生成一个 0 - sum 之间的整数 i
用二分查找找到 i 对应的前缀和数组的下标
时间复杂度 O(k), 空间复杂度 O(n)

__代码__:
__C++__:

```C++
class Solution 
{
private:
    vector<int> pre;
    int sum = 0;
    mt19937 rng{random_device{}()};
    uniform_int_distribution<int> uni;
public:
    Solution(vector<int>& w) 
    {
        for (int x : w) 
        {
            sum += x;
            pre.emplace_back(sum);
        }
        uni = uniform_int_distribution<int>{0, sum - 1};
    }
    
    int pickIndex() 
    {
        int i = uni(rng), l = 0, r = pre.size() - 1;
        while (l < r)
        {
            int mid = l + ((r - l) >> 1);
            if (pre[mid] <= i) l = mid + 1;
            else r = mid;
        }
        return l;
    }
};

/**
 * Your Solution object will be instantiated and called as such:
 * Solution* obj = new Solution(w);
 * int param_1 = obj->pickIndex();
 */
```

__Java__:

```Java
class Solution {

    private List<Integer> pre = new ArrayList<>();
    private int sum = 0;
    private Random rand = new Random();

    public Solution(int[] w) {
        for (int x : w) {
            sum += x;
            pre.add(sum);
        }
    }

    public int pickIndex() {
        int i = rand.nextInt(sum), l = 0, r = pre.size() - 1;
        while (l < r) {
            int mid = l + ((r - l) >> 1);
            if (pre.get(mid) <= i) l = mid + 1;
            else r = mid;
        }
        return l;
    }
}

/**
 * Your Solution object will be instantiated and called as such:
 * Solution obj = new Solution(w);
 * int param_1 = obj.pickIndex();
 */
```

__Python__:

```Python
class Solution:

    def __init__(self, w: List[int]):
        self.pre = [0] * (n := len(w))
        for i in range(n):
            self.pre[i] = self.pre[i - 1] + w[i]

    def pickIndex(self) -> int:
        r = int(random.random() * self.pre[-1] + 1)
        return bisect.bisect_left(self.pre, r)



# Your Solution object will be instantiated and called as such:
# obj = Solution(w)
# param_1 = obj.pickIndex()class Solution:
```
