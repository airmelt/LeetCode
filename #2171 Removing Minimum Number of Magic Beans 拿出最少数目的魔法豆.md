# 2171 Removing Minimum Number of Magic Beans 拿出最少数目的魔法豆

__Description:__

You are given an array of __positive__ integers `beans`, where each integer represents the number of magic beans found in a particular magic bag.

Remove any number of beans (possibly none) from each bag such that the number of beans in each remaining non-empty bag (still containing at least one bean) is equal. Once a bean has been removed from a bag, you are not allowed to return it to any of the bags.

Return the minimum number of magic beans that you have to remove.

__Example:__

Example 1:

```text
Input: beans = [4,1,6,5]
Output: 4
Explanation: 
```

- We remove 1 bean from the bag with only 1 bean.
  This results in the remaining bags: [4,0,6,5]
- Then we remove 2 beans from the bag with 6 beans.
  This results in the remaining bags: [4,0,4,5]
- Then we remove 1 bean from the bag with 5 beans.
  This results in the remaining bags: [4,0,4,4]

We removed a total of 1 + 2 + 1 = 4 beans to make the remaining non-empty bags have an equal number of beans.
There are no other solutions that remove 4 beans or fewer.

Example 2:

```text
Input: beans = [2,10,3,2]
Output: 7
Explanation:
```

- We remove 2 beans from one of the bags with 2 beans.
  This results in the remaining bags: [0,10,3,2]
- Then we remove 2 beans from the other bag with 2 beans.
  This results in the remaining bags: [0,10,3,0]
- Then we remove 3 beans from the bag with 3 beans.
  This results in the remaining bags: [0,10,0,0]

We removed a total of 2 + 2 + 3 = 7 beans to make the remaining non-empty bags have an equal number of beans.
There are no other solutions that removes 7 beans or fewer.

__Constraints:__

- `1 <= beans.length <= 10 ^ 5`
- `1 <= beans[i] <= 10 ^ 5`

__题目描述:__

给定一个 __正整数__ 数组 `beans` ，其中每个整数表示一个袋子里装的魔法豆的数目。

请你从每个袋子中 拿出 一些豆子（也可以 不拿出），使得剩下的 非空 袋子中（即 至少还有一颗 魔法豆的袋子）魔法豆的数目 相等。一旦把魔法豆从袋子中取出，你不能再将它放到任何袋子中。

请返回你需要拿出魔法豆的 最少数目。

__示例:__

示例 1：

```text
输入：beans = [4,1,6,5]
输出：4
解释：
```

- 我们从有 1 个魔法豆的袋子中拿出 1 颗魔法豆。
  剩下袋子中魔法豆的数目为：[4,0,6,5]
- 然后我们从有 6 个魔法豆的袋子中拿出 2 个魔法豆。
  剩下袋子中魔法豆的数目为：[4,0,4,5]
- 然后我们从有 5 个魔法豆的袋子中拿出 1 个魔法豆。
  剩下袋子中魔法豆的数目为：[4,0,4,4]

总共拿出了 1 + 2 + 1 = 4 个魔法豆，剩下非空袋子中魔法豆的数目相等。
没有比取出 4 个魔法豆更少的方案。

示例 2：

```text
输入：beans = [2,10,3,2]
输出：7
解释：
```

- 我们从有 2 个魔法豆的其中一个袋子中拿出 2 个魔法豆。
  剩下袋子中魔法豆的数目为：[0,10,3,2]
- 然后我们从另一个有 2 个魔法豆的袋子中拿出 2 个魔法豆。
  剩下袋子中魔法豆的数目为：[0,10,3,0]
- 然后我们从有 3 个魔法豆的袋子中拿出 3 个魔法豆。
  剩下袋子中魔法豆的数目为：[0,10,0,0]

总共拿出了 2 + 2 + 3 = 7 个魔法豆，剩下非空袋子中魔法豆的数目相等。
没有比取出 7 个魔法豆更少的方案。

__提示：__

- `1 <= beans.length <= 10 ^ 5`
- `1 <= beans[i] <= 10 ^ 5`

__思路:__

```text
贪心
先对 beans 排序
取 beans 中的值, 使得比 beans[i] 小的减为 0, 比 beans[i] 大的减为 beans[i]
即 m = max((n - i) * beans[i], m)
最后返回 sum(beans) - m
时间复杂度为 O(NlogN), 空间复杂度为 O(logN)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    long long minimumRemoval(vector<int>& beans) 
    {
        sort(beans.begin(), beans.end());
        long long sum = accumulate(beans.begin(), beans.end(), 0LL), m = 0LL;
        for (int i = 0, n = beans.size(); i < n; i++) m = max(m, (long long)(n - i) * beans[i]);
        return sum - m;
    }
};
```

__Java__:

```Java
class Solution {
    public long minimumRemoval(int[] beans) {
        long sum = Arrays.stream(beans).asLongStream().sum(), m = 0L;
        Arrays.sort(beans);
        for (int i = 0, n = beans.length; i < n; i++) m = Math.max(m, (long)(n - i) * beans[i]);
        return sum - m;
    }
}
```

__Python__:

```Python
class Solution:
    def minimumRemoval(self, beans: List[int]) -> int:
        return sum(beans) - max((len(beans) - i) * v for i, v in enumerate(sorted(beans)))
```
