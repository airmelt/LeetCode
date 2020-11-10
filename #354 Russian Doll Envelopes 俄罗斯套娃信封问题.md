__Description__:
You have a number of envelopes with widths and heights given as a pair of integers (w, h). One envelope can fit into another if and only if both the width and height of one envelope is greater than the width and height of the other envelope.

What is the maximum number of envelopes can you Russian doll? (put one inside other)

__Note:__
Rotation is not allowed.

__Example:__

Input: [[5,4],[6,4],[6,7],[2,3]]
Output: 3 
Explanation: The maximum number of envelopes you can Russian doll is 3 ([2,3] => [5,4] => [6,7]).

__题目描述__:
给定一些标记了宽度和高度的信封，宽度和高度以整数对形式 (w, h) 出现。当另一个信封的宽度和高度都比这个信封大的时候，这个信封就可以放进另一个信封里，如同俄罗斯套娃一样。

请计算最多能有多少个信封能组成一组“俄罗斯套娃”信封（即可以把一个信封放到另一个信封里面）。

__说明:__
不允许旋转信封。

__示例 :__

输入: envelopes = [[5,4],[6,4],[6,7],[2,3]]
输出: 3 
解释: 最多信封的个数为 3, 组合为: [2,3] => [5,4] => [6,7]。

__思路__:
按照长度增序, 宽度降序排序
这时只要选择最长的宽度的子序列即为最长的套娃
因为每次选择都选的是当前长度最窄的信封
最长子序列可以参考[LeetCode #300 Longest Increasing Subsequence 最长上升子序列](https://www.jianshu.com/p/96bfca1e204f)
时间复杂度O(nlgn), 空间复杂度O(n)

__代码__:
__C++__:
```C++
class Solution 
{
public:
    int maxEnvelopes(vector<vector<int>>& envelopes) 
    {
        if (envelopes.empty()) return 0;
        sort(envelopes.begin(), envelopes.end(), [](auto &a, auto &b) { return a[0] == b[0] ? a[1] > b[1] : a[0] < b[0]; });
        vector<int> dp;
        for (auto &envelope : envelopes)
        {
            auto i = lower_bound(dp.begin(), dp.end(), envelope[1]) - dp.begin();
            if (i < dp.size()) dp[i] = envelope[1];
            else dp.emplace_back(envelope[1]);
        }
        return dp.size();
    }
};
```

__Java__:
```Java
class Solution {
    public int maxEnvelopes(int[][] envelopes) {
        if (envelopes == null || envelopes.length == 0) return 0;
        Arrays.sort(envelopes, (a, b) -> (a[0] == b[0] ? b[1] - a[1] : a[0] - b[0]));
        List<Integer> dp = new ArrayList<>();
        for (int envelope[] : envelopes) {
            int index = helper(dp, envelope[1]);
            if (index < dp.size()) dp.set(index, envelope[1]);
            else dp.add(envelope[1]);
        }
        return dp.size();
    }
    
    private int helper(List<Integer> dp, int target) {
        if (dp.isEmpty()) return 0;
        int l = 0, r = dp.size();
        while (l < r) {
            int mid = l + ((r - l) >> 1);
            if (dp.get(mid) < target) l = mid + 1;
            else r = mid;
        }
        return l;
    }
}
```

__Python__:
```Python
class Solution:
    def maxEnvelopes(self, envelopes: List[List[int]]) -> int:
        envelopes.sort(key=lambda x: (x[0], -x[1]))
        dp = []
        for envelope in envelopes:
            i = bisect.bisect_left(dp, envelope[1])
            if i >= len(dp):
                dp.append(envelope[1])
            else:
                dp[i] = envelope[1]
        return len(dp)
```