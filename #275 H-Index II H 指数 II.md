__Description__:
Given an array of citations sorted in ascending order (each citation is a non-negative integer) of a researcher, write a function to compute the researcher's h-index.

According to the definition of [h-index](https://en.wikipedia.org/wiki/H-index)  on Wikipedia: "A scientist has index h if h of his/her N papers have at least h citations each, and the other N − h papers have no more than h citations each."

__Example:__

Input: citations = [0,1,3,5,6]
Output: 3 
Explanation: [0,1,3,5,6] means the researcher has 5 papers in total and each of them had 
             received 0, 1, 3, 5, 6 citations respectively. 
             Since the researcher has 3 papers with at least 3 citations each and the remaining 
             two with no more than 3 citations each, her h-index is 3.

__Note:__

If there are several possible values for h, the maximum one is taken as the h-index.

__Follow up:__

This is a follow up problem to H-Index, where citations is now guaranteed to be sorted in ascending order.
Could you solve it in logarithmic time complexity?

__题目描述__:
给定一位研究者论文被引用次数的数组（被引用次数是非负整数），数组已经按照 升序排列 。编写一个方法，计算出研究者的 h 指数。

[h 指数](https://baike.baidu.com/item/h-index/3991452?fr=aladdin)的定义: “h 代表“高引用次数”（high citations），一名科研人员的 h 指数是指他（她）的 （N 篇论文中）总共有 h 篇论文分别被引用了至少 h 次。（其余的 N - h 篇论文每篇被引用次数不多于 h 次。）"

__示例 :__

输入: citations = [0,1,3,5,6]
输出: 3 
解释: 给定数组表示研究者总共有 5 篇论文，每篇论文相应的被引用了 0, 1, 3, 5, 6 次。
     由于研究者有 3 篇论文每篇至少被引用了 3 次，其余两篇论文每篇被引用不多于 3 次，所以她的 h 指数是 3。
 
__说明：__

如果 h 有多有种可能的值 ，h 指数是其中最大的那个。

__进阶：__

这是 H 指数 的延伸题目，本题中的 citations 数组是保证有序的。
你可以优化你的算法到对数时间复杂度吗？

__思路__:
1. 由于数组已经有序, 考虑使用二分法
找到一个使得 citations[mid] >= citations.size() - mid的最左边的值即可
时间复杂度O(lgn), 空间复杂度O(1)

__代码__:
__C++__:
```C++
class Solution 
{
public:
    int hIndex(vector<int>& citations) 
    {
        int left = 0, right = citations.size() - 1, mid, result = 0;
        while (left <= right)
        {
            mid = left + ((right - left) >> 1);
            if (citations[mid] >= citations.size() - mid)
            {
                result = citations.size() - mid;
                right = mid - 1;
            }
            else left = mid + 1;
        }
        return result;
    }
};
```

__Java__:
```Java
class Solution {
    public int hIndex(int[] citations) {
        int left = 0, right = citations.length - 1, mid, result = 0;
        while (left <= right) {
            mid = left + ((right - left) >> 1);
            if (citations[mid] >= citations.length - mid) {
                result = citations.length - mid;
                right = mid - 1;
            }
            else left = mid + 1;
        }
        return result;
    }
}
```

__Python__:
```Python
class Solution:
    def hIndex(self, citations: List[int]) -> int:
        return max((len(citations) - i for i in range(len(citations)) if citations[i] >= len(citations) - i), default=0)
```