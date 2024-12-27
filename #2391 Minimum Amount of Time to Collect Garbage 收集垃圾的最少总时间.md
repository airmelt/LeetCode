# 2391 Minimum Amount of Time to Collect Garbage 收集垃圾的最少总时间

__Description:__

You are given a __0-indexed__ array of strings `garbage` where `garbage[i]` represents the assortment of garbage at the `i ^ th` house. `garbage[i]` consists only of the characters `'M'`, `'P'` and `'G'` representing one unit of metal, paper and glass garbage respectively. Picking up __one__ unit of any type of garbage takes `1` minute.

You are also given a __0-indexed__ integer array `travel` where `travel[i]` is the number of minutes needed to go from house `i` to house `i + 1`.

There are three garbage trucks in the city, each responsible for picking up one type of garbage. Each garbage truck starts at house `0` and must visit each house __in order__; however, they do __not__ need to visit every house.

Only one garbage truck may be used at any given moment. While one truck is driving or picking up garbage, the other two trucks cannot do anything.

Return the minimum number of minutes needed to pick up all the garbage.

__Example:__

Example 1:

```text
Input: garbage = ["G","P","GP","GG"], travel = [2,4,3]
Output: 21
Explanation:
The paper garbage truck:
```

1. Travels from house 0 to house 1
2. Collects the paper garbage at house 1
3. Travels from house 1 to house 2
4. Collects the paper garbage at house 2

Altogether, it takes 8 minutes to pick up all the paper garbage.

The glass garbage truck:

1. Collects the glass garbage at house 0
2. Travels from house 0 to house 1
3. Travels from house 1 to house 2
4. Collects the glass garbage at house 2
5. Travels from house 2 to house 3
6. Collects the glass garbage at house 3

Altogether, it takes 13 minutes to pick up all the glass garbage.

Since there is no metal garbage, we do not need to consider the metal garbage truck.

Therefore, it takes a total of 8 + 13 = 21 minutes to collect all the garbage.

Example 2:

```text
Input: garbage = ["MMM","PGM","GP"], travel = [3,10]
Output: 37
Explanation:
The metal garbage truck takes 7 minutes to pick up all the metal garbage.
The paper garbage truck takes 15 minutes to pick up all the paper garbage.
The glass garbage truck takes 15 minutes to pick up all the glass garbage.
It takes a total of 7 + 15 + 15 = 37 minutes to collect all the garbage.
```

__Constraints:__

- `2 <= garbage.length <= 10 ^ 5`
- `garbage[i]` consists of only the letters `'M'`, `'P'`, and `'G'`.
- `1 <= garbage[i].length <= 10`
- `travel.length == garbage.length - 1`
- `1 <= travel[i] <= 100`

__题目描述:__

给你一个下标从 __0__ 开始的字符串数组 `garbage` ，其中 `garbage[i]` 表示第 `i` 个房子的垃圾集合。 `garbage[i]` 只包含字符 `'M'` ， `'P'` 和 `'G'` ，但可能包含多个相同字符，每个字符分别表示一单位的金属、纸和玻璃。垃圾车收拾 __一__ 单位的任何一种垃圾都需要花费 `1` 分钟。

同时给你一个下标从 __0__ 开始的整数数组 `travel` ，其中 `travel[i]` 是垃圾车从房子 `i` 行驶到房子 `i + 1` 需要的分钟数。

城市里总共有三辆垃圾车，分别收拾三种垃圾。每辆垃圾车都从房子 `0` 出发，__按顺序__ 到达每一栋房子。但它们 __不是必须__ 到达所有的房子。

任何时刻只有 一辆 垃圾车处在使用状态。当一辆垃圾车在行驶或者收拾垃圾的时候，另外两辆车 不能 做任何事情。

请你返回收拾完所有垃圾需要花费的 最少 总分钟数。

__示例:__

示例 1：

```text
输入：garbage = ["G","P","GP","GG"], travel = [2,4,3]
输出：21
解释：
收拾纸的垃圾车：
```

1. 从房子 0 行驶到房子 1
2. 收拾房子 1 的纸垃圾
3. 从房子 1 行驶到房子 2
4. 收拾房子 2 的纸垃圾

收拾纸的垃圾车总共花费 8 分钟收拾完所有的纸垃圾。

收拾玻璃的垃圾车：

1. 收拾房子 0 的玻璃垃圾
2. 从房子 0 行驶到房子 1
3. 从房子 1 行驶到房子 2
4. 收拾房子 2 的玻璃垃圾
5. 从房子 2 行驶到房子 3
6. 收拾房子 3 的玻璃垃圾

收拾玻璃的垃圾车总共花费 13 分钟收拾完所有的玻璃垃圾。

由于没有金属垃圾，收拾金属的垃圾车不需要花费任何时间。

所以总共花费 8 + 13 = 21 分钟收拾完所有垃圾。

示例 2：

```text
输入：garbage = ["MMM","PGM","GP"], travel = [3,10]
输出：37
解释：
收拾金属的垃圾车花费 7 分钟收拾完所有的金属垃圾。
收拾纸的垃圾车花费 15 分钟收拾完所有的纸垃圾。
收拾玻璃的垃圾车花费 15 分钟收拾完所有的玻璃垃圾。
总共花费 7 + 15 + 15 = 37 分钟收拾完所有的垃圾。
```

__提示：__

- `2 <= garbage.length <= 10 ^ 5`
- `garbage[i]` 只包含字母 `'M'` ， `'P'` 和 `'G'` 。
- `1 <= garbage[i].length <= 10`
- `travel.length == garbage.length - 1`
- `1 <= travel[i] <= 100`

__思路:__

```text
1. 正序遍历
垃圾车收集第 0 个房子所有垃圾的时间为 garbage[0].size()
对于 i > 0, 垃圾车收集第 i 个房子所有垃圾的时间为 garbage[i].size() + travel[i - 1] - sum(pre), pre 表示上一个相同类型的垃圾的时间
使用 c & 3 是因为 'M', 'G', 'P' 的 ASCII 码分别为 77, 71, 80, 与 3 与运算可以得到 1, 3, 0
实现的时候 pre 可以用一个数组记录, 也可以用一个哈希表记录
时间复杂度为 O(N + M), 空间复杂度为 O(1), N 为 garbage 的长度, M 为垃圾字符的总数, 只用到了常数空间记录 'M', 'G', 'P' 的时间
2. 倒序遍历
对于 i > 0, 垃圾车收集垃圾的时间为 travel[i - 1] * visited.size() + garbage[i].size()
visited 用来记录已经出现过的垃圾的种类
最后加上第 0 个房子的垃圾的时间
时间复杂度为 O(N + M), 空间复杂度为 O(1), N 为 garbage 的长度, M 为垃圾字符的总数, 只用到了常数空间记录 'M', 'G', 'P' 的时间
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int garbageCollection(vector<string>& garbage, vector<int>& travel) 
    {
        int result = garbage.front().size(), s = 0, char_count[4]{}, n = garbage.size();
        for (int i = 1; i < n; i++) 
        {
            result += garbage[i].size();
            s += travel[i - 1];
            for (const auto& c : garbage[i]) 
            {
                result += s - char_count[c & 3];
                char_count[c & 3] = s;
            }
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int garbageCollection(String[] garbage, int[] travel) {
        int result = garbage[0].length();
        Set<Character> visited = new HashSet<>();
        for (int i = garbage.length - 1; i > 0; i--) {
            for (char c : garbage[i].toCharArray()) visited.add(c);
            result += garbage[i].length() + travel[i - 1] * visited.size();
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def garbageCollection(self, garbage: List[str], travel: List[int]) -> int:
        return sum(map(len, garbage)) + sum(pre[i] for i in {g: i for i, gar in enumerate(garbage) for g in gar}.values()) if (pre := list(accumulate(travel, initial=0))) else -1
```
