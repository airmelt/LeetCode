# 1376 Time Needed to Inform All Employees 通知所有员工所需的时间

__Description:__

A company has n employees with a unique ID for each employee from 0 to n - 1. The head of the company is the one with headID.

Each employee has one direct manager given in the manager array where manager[i] is the direct manager of the i-th employee, manager[headID] = -1. Also, it is guaranteed that the subordination relationships have a tree structure.

The head of the company wants to inform all the company employees of an urgent piece of news. He will inform his direct subordinates, and they will inform their subordinates, and so on until all employees know about the urgent news.

The i-th employee needs informTime[i] minutes to inform all of his direct subordinates (i.e., After informTime[i] minutes, all his direct subordinates can start spreading the news).

Return the number of minutes needed to inform all the employees about the urgent news.

__Example:__

Example 1:

Input: n = 1, headID = 0, manager = [-1], informTime = [0]
Output: 0
Explanation: The head of the company is the only employee in the company.

Example 2:

![1376-1](https://assets.leetcode.com/uploads/2020/02/27/graph.png)

Input: n = 6, headID = 2, manager = [2,2,-1,2,2,2], informTime = [0,0,1,0,0,0]
Output: 1
Explanation: The head of the company with id = 2 is the direct manager of all the employees in the company and needs 1 minute to inform them all.
The tree structure of the employees in the company is shown.

__Constraints:__

1 <= n <= 10^5
0 <= headID < n
manager.length == n
0 <= manager[i] < n
manager[headID] == -1
informTime.length == n
0 <= informTime[i] <= 1000
informTime[i] == 0 if employee i has no subordinates.
It is guaranteed that all the employees can be informed.

__题目描述：__

公司里有 n 名员工，每个员工的 ID 都是独一无二的，编号从 0 到 n - 1。公司的总负责人通过 headID 进行标识。

在 manager 数组中，每个员工都有一个直属负责人，其中 manager[i] 是第 i 名员工的直属负责人。对于总负责人，manager[headID] = -1。题目保证从属关系可以用树结构显示。

公司总负责人想要向公司所有员工通告一条紧急消息。他将会首先通知他的直属下属们，然后由这些下属通知他们的下属，直到所有的员工都得知这条紧急消息。

第 i 名员工需要 informTime[i] 分钟来通知它的所有直属下属（也就是说在 informTime[i] 分钟后，他的所有直属下属都可以开始传播这一消息）。

返回通知所有员工这一紧急消息所需要的 分钟数 。

__示例：__

示例 1：

输入：n = 1, headID = 0, manager = [-1], informTime = [0]
输出：0
解释：公司总负责人是该公司的唯一一名员工。

示例 2：

![1376-2](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/03/08/graph.png)

输入：n = 6, headID = 2, manager = [2,2,-1,2,2,2], informTime = [0,0,1,0,0,0]
输出：1
解释：id = 2 的员工是公司的总负责人，也是其他所有员工的直属负责人，他需要 1 分钟来通知所有员工。
上图显示了公司员工的树结构。

__提示：__

1 <= n <= 10^5
0 <= headID < n
manager.length == n
0 <= manager[i] < n
manager[headID] == -1
informTime.length == n
0 <= informTime[i] <= 1000
如果员工 i 没有下属，informTime[i] == 0 。
题目 保证 所有员工都可以收到通知。

__思路：__

DFS
从叶子结点开始回溯到经理
加上中途所需要的全部时间
取这段时间的最大值
时间复杂度为 O(n), 空间复杂度为 O(n)

__代码__:

__C++__:

```C++
class Solution 
{
public:
    int numOfMinutes(int n, int headID, vector<int>& manager, vector<int>& informTime) 
    {
        int result = 0;
        for (int i = 0; i < n; i++) 
        {
            if (!informTime[i]) 
            {
                int sum = 0, cur = i;
                while (manager[cur] != -1) 
                {
                    sum += informTime[manager[cur]];
                    cur = manager[cur];
                }
                result = max(result, sum);
            }
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int numOfMinutes(int n, int headID, int[] manager, int[] informTime) {
        int result = 0;
        for (int i = 0; i < n; i++) {
            if (informTime[i] == 0) {
                int sum = 0, cur = i;
                while (manager[cur] != -1) 
                {
                    sum += informTime[manager[cur]];
                    cur = manager[cur];
                }
                result = Math.max(result, sum);
            }
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def numOfMinutes(self, n: int, headID: int, manager: List[int], informTime: List[int]) -> int:
        result = 0
        for i in range(n):
            if not informTime[i]:
                s, cur = 0, i
                while manager[cur] != -1:
                    s += informTime[cur := manager[cur]]
                result = max(result, s)
        return result
```
