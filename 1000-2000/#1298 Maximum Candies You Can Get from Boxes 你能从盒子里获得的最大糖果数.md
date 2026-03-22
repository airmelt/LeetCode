# 1298 Maximum Candies You Can Get from Boxes 你能从盒子里获得的最大糖果数

__Description:__

You have n boxes labeled from 0 to n - 1. You are given four arrays: status, candies, keys, and containedBoxes where:

status[i] is 1 if the ith box is open and 0 if the ith box is closed,
candies[i] is the number of candies in the ith box,
keys[i] is a list of the labels of the boxes you can open after opening the ith box.
containedBoxes[i] is a list of the boxes you found inside the ith box.
You are given an integer array initialBoxes that contains the labels of the boxes you initially have. You can take all the candies in any open box and you can use the keys in it to open new boxes and you also can use the boxes you find in it.

Return the maximum number of candies you can get following the rules above.

__Example:__

Example 1:

Input: status = [1,0,1,0], candies = [7,5,4,100], keys = [[],[],[1],[]], containedBoxes = [[1,2],[3],[],[]], initialBoxes = [0]
Output: 16
Explanation: You will be initially given box 0. You will find 7 candies in it and boxes 1 and 2.
Box 1 is closed and you do not have a key for it so you will open box 2. You will find 4 candies and a key to box 1 in box 2.
In box 1, you will find 5 candies and box 3 but you will not find a key to box 3 so box 3 will remain closed.
Total number of candies collected = 7 + 4 + 5 = 16 candy.

Example 2:

Input: status = [1,0,0,0,0,0], candies = [1,1,1,1,1,1], keys = [[1,2,3,4,5],[],[],[],[],[]], containedBoxes = [[1,2,3,4,5],[],[],[],[],[]], initialBoxes = [0]
Output: 6
Explanation: You have initially box 0. Opening it you can find boxes 1,2,3,4 and 5 and their keys.
The total number of candies will be 6.

__Constraints:__

n == status.length == candies.length == keys.length == containedBoxes.length
1 <= n <= 1000
status[i] is either 0 or 1.
1 <= candies[i] <= 1000
0 <= keys[i].length <= n
0 <= keys\[i][j] < n
All values of keys[i] are unique.
0 <= containedBoxes[i].length <= n
0 <= containedBoxes\[i][j] < n
All values of containedBoxes[i] are unique.
Each box is contained in one box at most.
0 <= initialBoxes.length <= n
0 <= initialBoxes[i] < n

__题目描述：__

给你 n 个盒子，每个盒子的格式为 [status, candies, keys, containedBoxes] ，其中：

状态字 status[i]：整数，如果 box[i] 是开的，那么是 1 ，否则是 0 。
糖果数 candies[i]: 整数，表示 box[i] 中糖果的数目。
钥匙 keys[i]：数组，表示你打开 box[i] 后，可以得到一些盒子的钥匙，每个元素分别为该钥匙对应盒子的下标。
内含的盒子 containedBoxes[i]：整数，表示放在 box[i] 里的盒子所对应的下标。
给你一个 initialBoxes 数组，表示你现在得到的盒子，你可以获得里面的糖果，也可以用盒子里的钥匙打开新的盒子，还可以继续探索从这个盒子里找到的其他盒子。

请你按照上述规则，返回可以获得糖果的 最大数目 。

__示例：__

示例 1：

输入：status = [1,0,1,0], candies = [7,5,4,100], keys = [[],[],[1],[]], containedBoxes = [[1,2],[3],[],[]], initialBoxes = [0]
输出：16
解释：
一开始你有盒子 0 。你将获得它里面的 7 个糖果和盒子 1 和 2。
盒子 1 目前状态是关闭的，而且你还没有对应它的钥匙。所以你将会打开盒子 2 ，并得到里面的 4 个糖果和盒子 1 的钥匙。
在盒子 1 中，你会获得 5 个糖果和盒子 3 ，但是你没法获得盒子 3 的钥匙所以盒子 3 会保持关闭状态。
你总共可以获得的糖果数目 = 7 + 4 + 5 = 16 个。

示例 2：

输入：status = [1,0,0,0,0,0], candies = [1,1,1,1,1,1], keys = [[1,2,3,4,5],[],[],[],[],[]], containedBoxes = [[1,2,3,4,5],[],[],[],[],[]], initialBoxes = [0]
输出：6
解释：
你一开始拥有盒子 0 。打开它你可以找到盒子 1,2,3,4,5 和它们对应的钥匙。
打开这些盒子，你将获得所有盒子的糖果，所以总糖果数为 6 个。

示例 3：

输入：status = [1,1,1], candies = [100,1,100], keys = [[],[0,2],[]], containedBoxes = [[],[],[]], initialBoxes = [1]
输出：1

示例 4：

输入：status = [1], candies = [100], keys = [[]], containedBoxes = [[]], initialBoxes = []
输出：0

示例 5：

输入：status = [1,1,1], candies = [2,3,2], keys = [[],[],[]], containedBoxes = [[],[],[]], initialBoxes = [2,1,0]
输出：7

__提示：__

1 <= status.length <= 1000
status.length == candies.length == keys.length == containedBoxes.length == n
status[i] 要么是 0 要么是 1 。
1 <= candies[i] <= 1000
0 <= keys[i].length <= status.length
0 <= keys\[i][j] < status.length
keys[i] 中的值都是互不相同的。
0 <= containedBoxes[i].length <= status.length
0 <= containedBoxes\[i][j] < status.length
containedBoxes[i] 中的值都是互不相同的。
每个盒子最多被一个盒子包含。
0 <= initialBoxes.length <= status.length
0 <= initialBoxes[i] < status.length

__思路：__

```text
DFS
用 status = 1 表示有钥匙, 即可以打开盒子
再用另一个数组 has_box 来表示是否有盒子
初始化的时候将 initialBoxes 中的盒子标记为有
从 initialBoxes 中的 box 出发如果有钥匙且有盒子则进行 DFS
打开盒子之后加上糖果, 并将盒子置空, 表示已经访问过该盒子
然后打开钥匙盒中的所有钥匙, 将对应的 status 置为 1
如果有盒子则进行 DFS
打开盒子中的所有盒子, 将对应的 has_box 置为 1
如果有钥匙且有盒子则进行 DFS
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码__:

__C++__:

```C++
class Solution 
{
public:
    int maxCandies(vector<int>& status, vector<int>& candies, vector<vector<int>>& keys, vector<vector<int>>& containedBoxes, vector<int>& initialBoxes) 
    {
        int result = 0, n = status.size();
        vector<int> has_box(n);
        for (const auto& x : initialBoxes) has_box[x] = 1;
        auto dfs = [&](auto&& dfs, int x) -> void 
        {
            result += candies[x];
            has_box[x] = false;
            for (int key : keys[x]) 
            {
                status[key] = 1;
                if (has_box[key]) dfs(dfs, key);
            }
            for (int box : containedBoxes[x]) 
            {
                has_box[box] = true;
                if (status[box] > 0) dfs(dfs, box);
            }
        };
        for (const auto& x : initialBoxes) if (status[x] and has_box[x]) dfs(dfs, x);
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    private int result = 0;
    
    public int maxCandies(int[] status, int[] candies, int[][] keys, int[][] containedBoxes, int[] initialBoxes) {
        boolean[] hasBox = new boolean[status.length];
        for (int x : initialBoxes) hasBox[x] = true;
        for (int x : initialBoxes) if (status[x] > 0 && hasBox[x]) dfs(x, status, candies, keys, containedBoxes, hasBox);
        return result;
    }

    private void dfs(int x, int[] status, int[] candies, int[][] keys, int[][] containedBoxes, boolean[] hasBox) {
        result += candies[x];
        hasBox[x] = false;
        for (int key : keys[x]) {
            status[key] = 1;
            if (hasBox[key]) dfs(key, status, candies, keys, containedBoxes, hasBox);
        }
        for (int box : containedBoxes[x]) {
            hasBox[box] = true;
            if (status[box] > 0) dfs(box, status, candies, keys, containedBoxes, hasBox);
        }
    }
}
```

__Python__:

```Python
class Solution:
    def maxCandies(self, status: List[int], candies: List[int], keys: List[List[int]], containedBoxes: List[List[int]], initialBoxes: List[int]) -> int:
        result, has_box = 0, [i in set(initialBoxes) for i in range(len(status))]

        def dfs(x: int) -> NoReturn:
            nonlocal result
            result += candies[x]
            has_box[x] = 0
            for key in keys[x]:
                status[key] = 1
                if has_box[key]:
                    dfs(key)
            for box in containedBoxes[x]:
                has_box[box] = 1
                if status[box]:
                    dfs(box)
        for x in initialBoxes:
            if status[x] and has_box[x]:
                dfs(x)
        return result
```
