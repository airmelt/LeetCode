# 1105 Filling Bookcase Shelves 填充书架

__Description__:
You are given an array books where books[i] = [thicknessi, heighti] indicates the thickness and height of the ith book. You are also given an integer shelfWidth.

We want to place these books in order onto bookcase shelves that have a total width shelfWidth.

We choose some of the books to place on this shelf such that the sum of their thickness is less than or equal to shelfWidth, then build another level of the shelf of the bookcase so that the total height of the bookcase has increased by the maximum height of the books we just put down. We repeat this process until there are no more books to place.

Note that at each step of the above process, the order of the books we place is the same order as the given sequence of books.

For example, if we have an ordered list of 5 books, we might place the first and second book onto the first shelf, the third book on the second shelf, and the fourth and fifth book on the last shelf.
Return the minimum possible height that the total bookshelf can be after placing shelves in this manner.

__Example:__

Example 1:

![shelves](https://assets.leetcode.com/uploads/2019/06/24/shelves.png)

Input: books = [[1,1],[2,3],[2,3],[1,1],[1,1],[1,1],[1,2]], shelf_width = 4
Output: 6
Explanation:
The sum of the heights of the 3 shelves is 1 + 3 + 2 = 6.
Notice that book number 2 does not have to be on the first shelf.

Example 2:

Input: books = [[1,3],[2,4],[3,2]], shelfWidth = 6
Output: 4

__Constraints:__

1 <= books.length <= 1000
1 <= thicknessi <= shelfWidth <= 1000
1 <= heighti <= 1000

__题目描述__:
给定一个数组 books ，其中 books[i] = [thicknessi, heighti] 表示第 i 本书的厚度和高度。你也会得到一个整数 shelfWidth 。

按顺序 将这些书摆放到总宽度为 shelfWidth 的书架上。

先选几本书放在书架上（它们的厚度之和小于等于书架的宽度 shelfWidth ），然后再建一层书架。重复这个过程，直到把所有的书都放在书架上。

需要注意的是，在上述过程的每个步骤中，摆放书的顺序与你整理好的顺序相同。

例如，如果这里有 5 本书，那么可能的一种摆放情况是：第一和第二本书放在第一层书架上，第三本书放在第二层书架上，第四和第五本书放在最后一层书架上。
每一层所摆放的书的最大高度就是这一层书架的层高，书架整体的高度为各层高之和。

以这种方式布置书架，返回书架整体可能的最小高度。

__示例 :__

示例 1：

![书架](https://assets.leetcode.com/uploads/2019/06/24/shelves.png)

输入：books = [[1,1],[2,3],[2,3],[1,1],[1,1],[1,1],[1,2]], shelf_width = 4
输出：6
解释：
3 层书架的高度和为 1 + 3 + 2 = 6 。
第 2 本书不必放在第一层书架上。

示例 2:

输入: books = [[1,3],[2,4],[3,2]], shelfWidth = 6
输出: 4

__提示:__

1 <= books.length <= 1000
1 <= thicknessi <= shelfWidth <= 1000
1 <= heighti <= 1000

__思路__:

动态规划
令 dp[i] 表示前 i 本书所占的最小高度
每次将能放上新的一层书架的都放在该层
高度记录这一层书架最高的书的高度
将现在的高度更新为不能移动的高度加上现在的高度作为最小高度
时间复杂度O(n ^ 2), 空间复杂度O(n)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int minHeightShelves(vector<vector<int>>& books, int shelfWidth) 
    {
        int n = books.size();
        vector<int> dp(n + 1, INT_MAX);
        dp.front() = 0;
        for (int i = 1; i <= n; i++) 
        {
            int w = 0, h = 0;
            for (int j = i; j > 0; j--) 
            {
                w += books[j - 1][0];
                if (w > shelfWidth) break;
                h = max(h, books[j - 1][1]);
                dp[i] = min(dp[i], dp[j - 1] + h);
            }
        }
        return dp.back();
    }
};
```

__Java__:

```Java
class Solution {
    public int minHeightShelves(int[][] books, int shelfWidth) {
        int n = books.length, dp[] = new int[n + 1];
        for (int i = 1; i <= n; i++) {
            int w = 0, h = 0;
            dp[i] = Integer.MAX_VALUE;
            for (int j = i; j > 0; j--) {
                w += books[j - 1][0];
                if (w > shelfWidth) break;
                h = Math.max(h, books[j - 1][1]);
                dp[i] = Math.min(dp[i], dp[j - 1] + h);
            }
        }
        return dp[n];
    }
}
```

__Python__:

```Python
class Solution:
    def minHeightShelves(self, books: List[List[int]], shelfWidth: int) -> int:
        dp = [0] + [float('inf')] * (n := len(books))
        for i in range(1, n + 1):
            w = h = 0
            for j in range(i, 0, -1):
                w += books[j - 1][0]
                if w > shelfWidth:
                    break
                h = max(h, books[j - 1][1])
                dp[i] = min(dp[i], dp[j - 1] + h)
        return dp[n]
```
