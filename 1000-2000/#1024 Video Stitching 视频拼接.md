# 1024 Video Stitching 视频拼接

__Description__:
You are given a series of video clips from a sporting event that lasted time seconds. These video clips can be overlapping with each other and have varying lengths.

Each video clip is described by an array clips where clips[i] = [starti, endi] indicates that the ith clip started at starti and ended at endi.

We can cut these clips into segments freely.

For example, a clip [0, 7] can be cut into segments [0, 1] + [1, 3] + [3, 7].
Return the minimum number of clips needed so that we can cut the clips into segments that cover the entire sporting event [0, time]. If the task is impossible, return -1.

__Example:__

Example 1:

Input: clips = [[0,2],[4,6],[8,10],[1,9],[1,5],[5,9]], time = 10
Output: 3
Explanation: We take the clips [0,2], [8,10], [1,9]; a total of 3 clips.
Then, we can reconstruct the sporting event as follows:
We cut [1,9] into segments [1,2] + [2,8] + [8,9].
Now we have segments [0,2] + [2,8] + [8,10] which cover the sporting event [0, 10].

Example 2:

Input: clips = [[0,1],[1,2]], time = 5
Output: -1
Explanation: We cannot cover [0,5] with only [0,1] and [1,2].

Example 3:

Input: clips = [[0,1],[6,8],[0,2],[5,6],[0,4],[0,3],[6,7],[1,3],[4,7],[1,4],[2,5],[2,6],[3,4],[4,5],[5,7],[6,9]], time = 9
Output: 3
Explanation: We can take clips [0,4], [4,7], and [6,9].

__Constraints:__

1 <= clips.length <= 100
0 <= starti <= endi <= 100
1 <= time <= 100

__题目描述__:
你将会获得一系列视频片段，这些片段来自于一项持续时长为 time 秒的体育赛事。这些片段可能有所重叠，也可能长度不一。

使用数组 clips 描述所有的视频片段，其中 clips[i] = [starti, endi] 表示：某个视频片段开始于 starti 并于 endi 结束。

甚至可以对这些片段自由地再剪辑：

例如，片段 [0, 7] 可以剪切成 [0, 1] + [1, 3] + [3, 7] 三部分。
我们需要将这些片段进行再剪辑，并将剪辑后的内容拼接成覆盖整个运动过程的片段（[0, time]）。返回所需片段的最小数目，如果无法完成该任务，则返回 -1 。

__示例 :__

示例 1：

输入：clips = [[0,2],[4,6],[8,10],[1,9],[1,5],[5,9]], time = 10
输出：3
解释：
选中 [0,2], [8,10], [1,9] 这三个片段。
然后，按下面的方案重制比赛片段：
将 [1,9] 再剪辑为 [1,2] + [2,8] + [8,9] 。
现在手上的片段为 [0,2] + [2,8] + [8,10]，而这些覆盖了整场比赛 [0, 10]。

示例 2：

输入：clips = [[0,1],[1,2]], time = 5
输出：-1
解释：
无法只用 [0,1] 和 [1,2] 覆盖 [0,5] 的整个过程。

示例 3：

输入：clips = [[0,1],[6,8],[0,2],[5,6],[0,4],[0,3],[6,7],[1,3],[4,7],[1,4],[2,5],[2,6],[3,4],[4,5],[5,7],[6,9]], time = 9
输出：3
解释：
选取片段 [0,4], [4,7] 和 [6,9] 。

示例 4：

输入：clips = [[0,4],[2,8]], time = 5
输出：2
解释：
注意，你可能录制超过比赛结束时间的视频。

__提示:__

1 <= clips.length <= 100
0 <= starti <= endi <= 100
1 <= time <= 100

__思路__:

1. 动态规划
设 dp[i] 表示到 i 时间需要的片段数
dp[i] = min(dp[i], dp[c[0]] + 1), if c[0] <= i <= c[1]
初始化 dp[0] = 0, dp[i] = time + 1
时间复杂度为 O(n * time), 空间复杂度为 O(time), 其中 n 表示 clips 的大小
2. 贪心
用一个 reach 数组记录下当前时间能到达的最远处
reach[c[0]] = max(reach[c[0]], c[1]) for c in clips
用 pre 和 last 记录上一个区间能到达的最远的位置和当前区间能到达的最远位置
每次尽可能的往后找一个能够到达的位置
如果 i == last, 说明已经不能往后走, 返回 -1
如果 i == pre, 说明需要增加区间才能往后走, 结果自增
时间复杂度为 O(n + time), 空间复杂度为 O(time), 其中 n 表示 clips 的大小

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int videoStitching(vector<vector<int>>& clips, int time) 
    {
        vector<int> reach(time);
        for (const auto& clip : clips) if (clip.front() < time) reach[clip.front()] = max(reach[clip.front()], clip.back());
        int result = 0, pre = 0, last = 0;
        for (int i = 0; i < time; i++) 
        {
            last = max(reach[i], last);
            if (i == last) return -1;
            if (i == pre) 
            {
                ++result;
                pre = last;
            }
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int videoStitching(int[][] clips, int time) {
        int reach[] = new int[time], result = 0, pre = 0, last = 0;
        for (int[] clip : clips) if (clip[0] < time) reach[clip[0]] = Math.max(reach[clip[0]], clip[1]);
        for (int i = 0; i < time; i++) 
        {
            last = Math.max(reach[i], last);
            if (i == last) return -1;
            if (i == pre) {
                ++result;
                pre = last;
            }
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def videoStitching(self, clips: List[List[int]], time: int) -> int:
        dp = [0] + [time + 1] * time
        for i in range(time + 1):
            for c in clips:
                if c[0] <= i <= c[1]:
                    dp[i] = min(dp[i], dp[c[0]] + 1)
        return -1 if dp[-1] == time + 1 else dp[-1]
```
