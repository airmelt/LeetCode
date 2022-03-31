# 1090 Largest Values From Labels 受标签影响的最大值

__Description__:
There is a set of n items. You are given two integer arrays values and labels where the value and the label of the ith element are values[i] and labels[i] respectively. You are also given two integers numWanted and useLimit.

Choose a subset s of the n elements such that:

The size of the subset s is less than or equal to numWanted.
There are at most useLimit items with the same label in s.
The score of a subset is the sum of the values in the subset.

Return the maximum score of a subset s.

__Example:__

Example 1:

Input: values = [5,4,3,2,1], labels = [1,1,2,2,3], numWanted = 3, useLimit = 1
Output: 9
Explanation: The subset chosen is the first, third, and fifth items.

Example 2:

Input: values = [5,4,3,2,1], labels = [1,3,3,3,2], numWanted = 3, useLimit = 2
Output: 12
Explanation: The subset chosen is the first, second, and third items.

Example 3:

Input: values = [9,8,8,7,6], labels = [0,0,0,1,1], numWanted = 3, useLimit = 1
Output: 16
Explanation: The subset chosen is the first and fourth items.

__Constraints:__

n == values.length == labels.length
1 <= n <= 2 \* 10^4
0 <= values[i], labels[i] <= 2 \* 10^4
1 <= numWanted, useLimit <= n

__题目描述__:
我们有一个 n 项的集合。给出两个整数数组 values 和 labels ，第 i 个元素的值和标签分别是 values[i] 和 labels[i]。还会给出两个整数 numWanted 和 useLimit 。

从 n 个元素中选择一个子集 s :

子集 s 的大小 小于或等于 numWanted 。
s 中 最多 有相同标签的 useLimit 项。
一个子集的 分数 是该子集的值之和。

返回子集 s 的最大 分数 。

__示例 :__

示例 1：

输入：values = [5,4,3,2,1], labels = [1,1,2,2,3], numWanted = 3, useLimit = 1
输出：9
解释：选出的子集是第一项，第三项和第五项。

示例 2：

输入：values = [5,4,3,2,1], labels = [1,3,3,3,2], numWanted = 3, useLimit = 2
输出：12
解释：选出的子集是第一项，第二项和第三项。

示例 3：

输入：values = [9,8,8,7,6], labels = [0,0,0,1,1], numWanted = 3, useLimit = 1
输出：16
解释：选出的子集是第一项和第四项。

__提示:__

n == values.length == labels.length
1 <= n <= 2 \* 10^4
0 <= values[i], labels[i] <= 2 \* 10^4
1 <= numWanted, useLimit <= n

__思路__:

排序/优先队列 ➕ 贪心
按照 values 和 labels 的对应排序, 将 value 大的放在前面
先取 value 较大的, 直到满足 useLimit
当取得的数的个数为 numWanted 时跳出循环
时间复杂度O(nlgn), 空间复杂度O(n)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int largestValsFromLabels(vector<int>& values, vector<int>& labels, int numWanted, int useLimit) 
    {
        vector<pair<int, int>> items;
        for (int i = 0; i < values.size(); i++) items.push_back({-values[i], labels[i]});
        sort(items.begin(), items.end());
        int result = 0, cur = 0;
        unordered_map<int, int> count;
        for (const auto& item : items) 
        {
            if (count[item.second] < useLimit) 
            {
                result += -item.first;
                if (!(--numWanted)) break;
                ++count[item.second];
            }
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int largestValsFromLabels(int[] values, int[] labels, int numWanted, int useLimit) {
        int result = 0, cur = 0, n = values.length, items[][] = new int[n][2];
        for (int i = 0; i < n; ++i) {
            items[i][0] = values[i];
            items[i][1] = labels[i];
        }
        Arrays.sort(items, Comparator.comparingInt(i -> -i[0]));
        Map<Integer, Integer> map = new HashMap<>();
        for (int[] item : items) {
            int labelCount = map.getOrDefault(item[1], 0);
            if (labelCount < useLimit) {
                result += item[0];
                if (--numWanted == 0) break;
                map.put(item[1], labelCount + 1);
            }
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def largestValsFromLabels(self, values: List[int], labels: List[int], numWanted: int, useLimit: int) -> int:
        items, result, count = sorted(list(zip(values, labels)), reverse=True), 0, defaultdict(int)
        for item in items:
            if (label_count := count[item[1]]) < useLimit:
                result += item[0]
                numWanted -= 1
                if not numWanted:
                    break
                count[item[1]] += 1
        return result
```
