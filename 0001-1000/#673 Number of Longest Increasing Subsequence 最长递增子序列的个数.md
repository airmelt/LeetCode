# 673 Number of Longest Increasing Subsequence 最长递增子序列的个数

__Description__:
Given an integer array nums, return the number of longest increasing subsequences.

Notice that the sequence has to be strictly increasing.

__Example:__

Example 1:

Input: nums = [1,3,5,4,7]
Output: 2
Explanation: The two longest increasing subsequences are [1, 3, 4, 7] and [1, 3, 5, 7].

Example 2:

Input: nums = [2,2,2,2,2]
Output: 5
Explanation: The length of longest continuous increasing subsequence is 1, and there are 5 subsequences' length is 1, so output 5.

__Constraints:__

1 <= nums.length <= 2000
-10^6 <= nums[i] <= 10^6

__题目描述__:
给定一个未排序的整数数组，找到最长递增子序列的个数。

__示例 :__

示例 1:

输入: [1,3,5,4,7]
输出: 2
解释: 有两个最长递增子序列，分别是 [1, 3, 4, 7] 和[1, 3, 5, 7]。

示例 2:

输入: [2,2,2,2,2]
输出: 5
解释: 最长递增子序列的长度是1，并且存在5个子序列的长度为1，因此输出5。

__注意:__
给定的数组长度不超过 2000 并且结果一定是32位有符号整数。

__思路__:

1. 动态规划
dp[i] 表示以 nums[i] 结尾的最长递增子序列的长度
count[i] 表示以 nums[i] 结尾的最长递增子序列的个数
dp[i] = dp[j] + 1, if nums[j] < nums[i] and dp[j] >= dp[i]
count[i] = count[j], if nums[j] < nums[i] and dp[j] >= dp[i]
count[i] += count[j], if nums[j] < nums[i] and dp[j] + 1 == dp[i]
其中 -1 < j < i
时间复杂度 O(n ^ 2), 空间复杂度 O(n)
2. 线段树
线段树额外记录当前结点的最长递增子序列的长度和数量
时间复杂度 O(nlgn), 空间复杂度 O(n)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int findNumberOfLIS(vector<int>& nums) 
    {
        int n = nums.size(), result = 0, max_length = 0;
        vector<int> dp(n, 0), count(n, 1);
        for (int i = 0; i < n; i++)
        {
            for (int j = 0; j < i; j++)
            {
                if (nums[j] < nums[i])
                {
                    if (dp[i] == dp[j] + 1) count[i] += count[j];
                    else if (dp[j] >= dp[i])
                    {
                        dp[i] = dp[j] + 1;
                        count[i] = count[j];
                    }
                }
            }
        }
        max_length = *max_element(dp.begin(), dp.end());
        for (int i = 0; i < n; i++) if (dp[i] == max_length) result += count[i];
        return result;
    }
};
```

__Java__:

```Java
class Node {
    int rangeLeft, rangeRight;
    Node left, right;
    Value val;
    
    public Node(int start, int end) {
        rangeLeft = start;
        rangeRight = end;
        left = null;
        right = null;
        val = new Value(0, 1);
    }
    
    public int getMid() {
        return rangeLeft + ((rangeRight - rangeLeft) >>> 1);
    }
    
    public Node getLeft() {
        if (left == null) left = new Node(rangeLeft, getMid());
        return left;
    }
    
    public Node getRight() {
        if (right == null) right = new Node(getMid() + 1, rangeRight);
        return right;
    }
}

class Value {
    int length;
    int count;
    public Value(int l, int c) {
        length = l;
        count = c;
    }
}

class Solution {
    public Value merge(Value v1, Value v2) {
        if (v1.length == v2.length) {
            if (v1.length == 0) return new Value(0, 1);
            return new Value(v1.length, v1.count + v2.count);
        }
        return v1.length > v2.length ? v1 : v2;
    }

    public void insert(Node node, int key, Value val) {
        if (node.rangeLeft == node.rangeRight) {
            node.val = merge(val, node.val);
            return;
        } else if (key <= node.getMid()) insert(node.getLeft(), key, val);
        else insert(node.getRight(), key, val);
        node.val = merge(node.getLeft().val, node.getRight().val);
    }

    public Value query(Node node, int key) {
        if (node.rangeRight <= key) return node.val;
        else if (node.rangeLeft > key) return new Value(0, 1);
        else return merge(query(node.getLeft(), key), query(node.getRight(), key));
    }

    public int findNumberOfLIS(int[] nums) {
        if (nums.length == 0) return 0;
        int min = nums[0], max = nums[0];
        for (int num: nums) {
            min = Math.min(min, num);
            max = Math.max(max, num);
        }
        Node root = new Node(min, max);
        for (int num: nums) {
            Value v = query(root, num - 1);
            insert(root, num, new Value(v.length + 1, v.count));
        }
        return root.val.count;
    }
}
```

__Python__:

```Python
class Solution:
    def findNumberOfLIS(self, nums):
        n = len(nums)
        if n <= 1: 
            return n
        dp, count = [0] * n, [1] * n
        for i, num in enumerate(nums):
            for j in range(i):
                if nums[j] < nums[i]:
                    if dp[j] >= dp[i]:
                        dp[i] = 1 + dp[j]
                        count[i] = count[j]
                    elif dp[j] + 1 == dp[i]:
                        count[i] += count[j]
        return sum(c for i, c in enumerate(count) if dp[i] == max(dp))
```
