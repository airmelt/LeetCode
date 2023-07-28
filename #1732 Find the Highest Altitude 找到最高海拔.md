# 1732 Find the Highest Altitude 找到最高海拔

__Description:__

There is a biker going on a road trip. The road trip consists of `n + 1` points at different altitudes. The biker starts his trip on point `0` with altitude equal `0`.

You are given an integer array `gain` of length `n` where `gain[i]` is the __net gain in altitude__ between points `i`​​​​​​ and `i + 1` for all ( `0 <= i < n)`. Return _the __highest altitude__ of a point._

__Example:__

Example 1:

```text
Input: gain = [-5,1,5,0,-7]
Output: 1
Explanation: The altitudes are [0,-5,-4,1,1,-6]. The highest is 1.
```

Example 2:

```text
Input: gain = [-4,-3,-2,-1,4,3,2]
Output: 0
Explanation: The altitudes are [0,-4,-7,-9,-10,-6,-3,-1]. The highest is 0.
```

__Constraints:__

- `n == gain.length`
- `1 <= n <= 100`
- `-100 <= gain[i] <= 100`

__题目描述:__

有一个自行车手打算进行一场公路骑行，这条路线总共由 `n + 1` 个不同海拔的点组成。自行车手从海拔为 `0` 的点 `0` 开始骑行。

给你一个长度为 `n` 的整数数组 `gain` ，其中 `gain[i]` 是点 `i` 和点 `i + 1` 的 __净海拔高度差__（ `0 <= i < n`）。请你返回 __最高点的海拔__ 。

__示例:__

示例 1：

```text
输入：gain = [-5,1,5,0,-7]
输出：1
解释：海拔高度依次为 [0,-5,-4,1,1,-6] 。最高海拔为 1 。
```

示例 2：

```text
输入：gain = [-4,-3,-2,-1,4,3,2]
输出：0
解释：海拔高度依次为 [0,-4,-7,-9,-10,-6,-3,-1] 。最高海拔为 0 。
```

__提示：__

- `n == gain.length`
- `1 <= n <= 100`
- `-100 <= gain[i] <= 100`

__思路:__

```text
模拟
初始化结果为 0 或 gain[0] 中的较大值
遍历 gain, gain[i] += gain[i - 1], 更新结果
结果为累计值和 0 中的较大值
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int largestAltitude(vector<int>& gain) 
    {
        int result = max(gain.front(), 0), n = gain.size();
        for (int i = 1; i < n; i++) result = max(result, gain[i] += gain[i - 1]);
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int largestAltitude(int[] gain) {
        int result = Math.max(gain[0], 0), n = gain.length;
        for (int i = 1; i < n; i++) result = Math.max(result, gain[i] += gain[i - 1]);
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def largestAltitude(self, gain: List[int]) -> int:
        return max(list(accumulate(gain)) + [0])
```
