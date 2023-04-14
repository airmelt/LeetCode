# 1601 Maximum Number of Achievable Transfer Requests 最多可达成的换楼请求数目

__Description:__

We have `n` buildings numbered from `0` to `n - 1`. Each building has a number of employees. It's transfer season, and some employees want to change the building they reside in.

You are given an array `requests` where `requests[i] = [fromi, toi]` represents an employee's request to transfer from building `fromi` to building `toi`.

__All buildings are full__, so a list of requests is achievable only if for each building, the __net change in employee transfers is zero__. This means the number of employees __leaving__ is __equal__ to the number of employees __moving in__. For example if `n = 3` and two employees are leaving building `0`, one is leaving building `1`, and one is leaving building `2`, there should be two employees moving to building `0`, one employee moving to building `1`, and one employee moving to building `2`.

Return the maximum number of achievable requests.

__Example:__

Example 1:

![1601-1](https://assets.leetcode.com/uploads/2020/09/10/move1.jpg)

```text
Input: n = 5, requests = [[0,1],[1,0],[0,1],[1,2],[2,0],[3,4]]
Output: 5
Explantion: Let's see the requests:
From building 0 we have employees x and y and both want to move to building 1.
From building 1 we have employees a and b and they want to move to buildings 2 and 0 respectively.
From building 2 we have employee z and they want to move to building 0.
From building 3 we have employee c and they want to move to building 4.
From building 4 we don't have any requests.
We can achieve the requests of users x and b by swapping their places.
We can achieve the requests of users y, a and z by swapping the places in the 3 buildings.
```

Example 2:

![1601-2](https://assets.leetcode.com/uploads/2020/09/10/move2.jpg)

```text
Input: n = 3, requests = [[0,0],[1,2],[2,1]]
Output: 3
Explantion: Let's see the requests:
From building 0 we have employee x and they want to stay in the same building 0.
From building 1 we have employee y and they want to move to building 2.
From building 2 we have employee z and they want to move to building 1.
We can achieve all the requests.
```

Example 3:

```text
Input: n = 4, requests = [[0,3],[3,1],[1,2],[2,0]]
Output: 4
```

__Constraints:__

- `1 <= n <= 20`
- `1 <= requests.length <= 16`
- `requests[i].length == 2`
- `0 <= fromi, toi < n`

__题目描述:__

我们有 `n` 栋楼，编号从 `0` 到 `n - 1` 。每栋楼有若干员工。由于现在是换楼的季节，部分员工想要换一栋楼居住。

给你一个数组 `requests` ，其中 `requests[i] = [fromi, toi]` ，表示一个员工请求从编号为 `fromi` 的楼搬到编号为 `toi` 的楼。

一开始 __所有楼都是满的__，所以从请求列表中选出的若干个请求是可行的需要满足 __每栋楼员工净变化为 0__ 。意思是每栋楼 __离开__ 的员工数目 __等于__ 该楼 __搬入__ 的员工数数目。比方说 `n = 3` 且两个员工要离开楼 `0` ，一个员工要离开楼 `1` ，一个员工要离开楼 `2` ，如果该请求列表可行，应该要有两个员工搬入楼 `0` ，一个员工搬入楼 `1` ，一个员工搬入楼 `2` 。

请你从原请求列表中选出若干个请求，使得它们是一个可行的请求列表，并返回所有可行列表中最大请求数目。

__示例:__

示例 1：

![1601-3](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/09/26/move1.jpg)

```text
输入：n = 5, requests = [[0,1],[1,0],[0,1],[1,2],[2,0],[3,4]]
输出：5
解释：请求列表如下：
从楼 0 离开的员工为 x 和 y ，且他们都想要搬到楼 1 。
从楼 1 离开的员工为 a 和 b ，且他们分别想要搬到楼 2 和 0 。
从楼 2 离开的员工为 z ，且他想要搬到楼 0 。
从楼 3 离开的员工为 c ，且他想要搬到楼 4 。
没有员工从楼 4 离开。
我们可以让 x 和 b 交换他们的楼，以满足他们的请求。
我们可以让 y，a 和 z 三人在三栋楼间交换位置，满足他们的要求。
所以最多可以满足 5 个请求。
```

示例 2：

![1601-4](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/09/26/move2.jpg)

```text
输入：n = 3, requests = [[0,0],[1,2],[2,1]]
输出：3
解释：请求列表如下：
从楼 0 离开的员工为 x ，且他想要回到原来的楼 0 。
从楼 1 离开的员工为 y ，且他想要搬到楼 2 。
从楼 2 离开的员工为 z ，且他想要搬到楼 1 。
我们可以满足所有的请求。
```

示例 3：

```text
输入：n = 4, requests = [[0,3],[3,1],[1,2],[2,0]]
输出：4
```

__提示：__

- `1 <= n <= 20`
- `1 <= requests.length <= 16`
- `requests[i].length == 2`
- `0 <= fromi, toi < n`

__思路:__

```text
状态压缩
对于每一个交换要求都有选择或者不选择两种
遍历所有选择方式
记录一个出度入度数组
在安排完所有交换之后, 出度入度数组必须全为 0 才是一次合法的交换
时间复杂度为 O(N2 ^ M), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int maximumRequests(int n, vector<vector<int>>& requests) 
    {
        int m = requests.size(), result = 0, mask = 1 << m;
        vector<int> cur(n, 0);
        for (int state = 0, step = 0; state < mask; state++, step = 0)
        {
            fill(cur.begin(), cur.end(), step);
            for (int i = 0; i < m; i++)
            {
                if (state >> i & 1)
                {
                    ++step;
                    --cur[requests[i].front()];
                    ++cur[requests[i].back()];
                }
                    
            }
            if (count(cur.begin(), cur.end(), 0) == n) result = max(result, step);
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int maximumRequests(int n, int[][] requests) {
        int m = requests.length, result = 0, mask = 1 << m, start[] = new int[n];
        for (int state = 0, step = 0; state < mask; state++, step = 0) {
            int[] cur = new int[n];
            for (int i = 0; i < m; i++) {
                if ((state >> i & 1) == 1) {
                    ++step;
                    --cur[requests[i][0]];
                    ++cur[requests[i][1]];
                }
                    
            }
            if (Arrays.equals(start, cur)) result = Math.max(result, step);
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def maximumRequests(self, n: int, requests: List[List[int]]) -> int:
        for i in range(len(requests), 0, -1):
            for c in combinations(requests, i):
                cur = [0] * n
                for a, b in c:
                    cur[a] += 1
                    cur[b] -= 1
                if all(not c for c in cur):
                    return i
        return 0
```
