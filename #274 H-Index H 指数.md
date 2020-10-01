__Description__:
Given an array of citations (each citation is a non-negative integer) of a researcher, write a function to compute the researcher's h-index.

According to the definition of [h-index](https://en.wikipedia.org/wiki/H-index) on Wikipedia: "A scientist has index h if h of his/her N papers have at least h citations each, and the other N − h papers have no more than h citations each."

__Example:__

Input: citations = [3,0,6,1,5]
Output: 3 
Explanation: [3,0,6,1,5] means the researcher has 5 papers in total and each of them had 
             received 3, 0, 6, 1, 5 citations respectively. 
             Since the researcher has 3 papers with at least 3 citations each and the remaining 
             two with no more than 3 citations each, her h-index is 3.

__Note:__
If there are several possible values for h, the maximum one is taken as the h-index.

__题目描述__:
给定一位研究者论文被引用次数的数组（被引用次数是非负整数）。编写一个方法，计算出研究者的 h 指数。

[h 指数](https://baike.baidu.com/item/h-index/3991452?fr=aladdin)的定义：h 代表“高引用次数”（high citations），一名科研人员的 h 指数是指他（她）的 （N 篇论文中）总共有 h 篇论文分别被引用了至少 h 次。（其余的 N - h 篇论文每篇被引用次数 不超过 h 次。）

例如：某人的 h 指数是 20，这表示他已发表的论文中，每篇被引用了至少 20 次的论文总共有 20 篇。

__示例 :__

输入：citations = [3,0,6,1,5]
输出：3 
解释：给定数组表示研究者总共有 5 篇论文，每篇论文相应的被引用了 3, 0, 6, 1, 5 次。
     由于研究者有 3 篇论文每篇 至少 被引用了 3 次，其余两篇论文每篇被引用 不多于 3 次，所以她的 h 指数是 3。

__提示：__
如果 h 有多种可能的值，h 指数是其中最大的那个。

__思路__:
1. 先排序, 然后比较 i和citations[citations.size() - i - 1]
时间复杂度O(nlgn), 空间复杂度O(1)
2. 由于要求 h篇文章至少有 h个引用
最多只有 n个引用
先统计各引用出现的次数, 大于 n的按 n计
然后从 n开始找到第一个累计大于 i的值, 即为答案
时间复杂度O(n), 空间复杂度O(n)

__代码__:
__C++__:
```C++
class Solution 
{
public:
    int hIndex(vector<int>& citations) 
    {
        int n = citations.size();
        vector<int> count(n + 1, 0);
        for (auto &c : citations) ++count[min(n, c)];
        int result = n;
        for (int i = count[n]; result > i; i += count[result]) --result;
        return result;
    }
};
```

__Java__:
```Java
class Solution {
    public int hIndex(int[] citations) {
        int count[] = new int[citations.length + 1], n = citations.length;
        for (int c : citations) ++count[Math.min(n, c)];
        int result = n;
        for (int i = count[n]; result > i; i += count[result]) --result;
        return result;
    }
}
```

__Python__:
```Python
class Solution:
    def hIndex(self, citations: List[int]) -> int:
        return max((len(citations) - i for i in range(len(citations)) if sorted(citations)[i] >= len(citations) - i), default=0)
```