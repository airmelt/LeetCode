# 1054 Distant Barcodes 距离相等的条形码

__Description__:
In a warehouse, there is a row of barcodes, where the ith barcode is barcodes[i].

Rearrange the barcodes so that no two adjacent barcodes are equal. You may return any answer, and it is guaranteed an answer exists.

__Example:__

Example 1:

Input: barcodes = [1,1,1,2,2,2]
Output: [2,1,2,1,2,1]

Example 2:

Input: barcodes = [1,1,1,1,2,2,3,3]
Output: [1,3,1,3,1,2,1,2]

__Constraints:__

1 <= barcodes.length <= 10000
1 <= barcodes[i] <= 10000

__题目描述__:
在一个仓库里，有一排条形码，其中第 i 个条形码为 barcodes[i]。

请你重新排列这些条形码，使其中任意两个相邻的条形码不能相等。 你可以返回任何满足该要求的答案，此题保证存在答案。

__示例 :__

示例 1：

输入：barcodes = [1,1,1,2,2,2]
输出：[2,1,2,1,2,1]

示例 2：

输入：barcodes = [1,1,1,1,2,2,3,3]
输出：[1,3,1,3,2,1,2,1]

__提示:__

1 <= barcodes.length <= 10000
1 <= barcodes[i] <= 10000

__思路__:

模拟
记录每个元素的出现次数
利用最大堆存储每个元素的出现次数及对应值
按照出现次数每次弹出两个元素插入结果中
然后减少出现次数再插入最大堆中
时间复杂度O(nlgn), 空间复杂度O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    vector<int> rearrangeBarcodes(vector<int>& barcodes) 
    {
        unordered_map<int, int> m;
        priority_queue<pair<int,int>> pq;
        vector<int> result;
        for (const auto& b : barcodes) ++m[b];
        for (const auto& [k, v] : m) pq.push(make_pair(v, k));
        while(pq.size() >= 2)
        {
            auto top1 = pq.top(); 
            pq.pop();
            auto top2 = pq.top(); 
            pq.pop();
            result.emplace_back(top1.second);
            result.emplace_back(top2.second); 
            if (--top1.first > 0) pq.push(top1);
            if (--top2.first > 0) pq.push(top2);
        }
        if (!pq.empty()) result.emplace_back(pq.top().second);
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int[] rearrangeBarcodes(int[] barcodes) {
        int n = barcodes.length, count[] = new int[10001], cur[] = new int[n], j = 0, result[] = new int[n];
        for (int barcode : barcodes) ++count[barcode];
        PriorityQueue<Integer> pq = new PriorityQueue<>((o1, o2) -> count[o2] - count[o1]);
        for (int i = 1; i < count.length; i++) if (count[i] != 0) pq.offer(i); 
        while (!pq.isEmpty()) {
            int item = pq.poll();
            for (int i = 0; i < count[item]; i++) cur[j++] = item;
        }
        j = 0; 
        for (int i = 0; i < n; i += 2) result[i] = cur[j++];
        for (int i = 1; i < n; i += 2) result[i] = cur[j++];
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def rearrangeBarcodes(self, barcodes: List[int]) -> List[int]:
        return list(itertools.chain.from_iterable(zip_longest((data := sum([[i] * j for i, j in Counter(barcodes).most_common()], []))[:(((n := len(barcodes)) + 1)) >> 1], data[(n + 1) >> 1:])))[:n]
```
