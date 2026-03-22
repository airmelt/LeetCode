# 904 Fruit Into Baskets 水果成篮

__Description__:
You are visiting a farm that has a single row of fruit trees arranged from left to right. The trees are represented by an integer array fruits where fruits[i] is the type of fruit the ith tree produces.

You want to collect as much fruit as possible. However, the owner has some strict rules that you must follow:

You only have two baskets, and each basket can only hold a single type of fruit. There is no limit on the amount of fruit each basket can hold.
Starting from any tree of your choice, you must pick exactly one fruit from every tree (including the start tree) while moving to the right. The picked fruits must fit in one of your baskets.
Once you reach a tree with fruit that cannot fit in your baskets, you must stop.
Given the integer array fruits, return the maximum number of fruits you can pick.

__Example:__

Example 1:

Input: fruits = [1,2,1]
Output: 3
Explanation: We can pick from all 3 trees.

Example 2:

Input: fruits = [0,1,2,2]
Output: 3
Explanation: We can pick from trees [1,2,2].
If we had started at the first tree, we would only pick from trees [0,1].

Example 3:

Input: fruits = [1,2,3,2,2]
Output: 4
Explanation: We can pick from trees [2,3,2,2].
If we had started at the first tree, we would only pick from trees [1,2].

Example 4:

Input: fruits = [3,3,3,1,2,1,1,2,3,3,4]
Output: 5
Explanation: We can pick from trees [1,2,1,1,2].

__Constraints:__

1 <= fruits.length <= 10^5
0 <= fruits[i] < fruits.length

__题目描述__:
在一排树中，第 i 棵树产生 tree[i] 型的水果。
你可以从你选择的任何树开始，然后重复执行以下步骤：

把这棵树上的水果放进你的篮子里。如果你做不到，就停下来。
移动到当前树右侧的下一棵树。如果右边没有树，就停下来。
请注意，在选择一颗树后，你没有任何选择：你必须执行步骤 1，然后执行步骤 2，然后返回步骤 1，然后执行步骤 2，依此类推，直至停止。

你有两个篮子，每个篮子可以携带任何数量的水果，但你希望每个篮子只携带一种类型的水果。

用这个程序你能收集的水果树的最大总量是多少？

__示例 :__

示例 1：

输入：[1,2,1]
输出：3
解释：我们可以收集 [1,2,1]。

示例 2：

输入：[0,1,2,2]
输出：3
解释：我们可以收集 [1,2,2]
如果我们从第一棵树开始，我们将只能收集到 [0, 1]。

示例 3：

输入：[1,2,3,2,2]
输出：4
解释：我们可以收集 [2,3,2,2]
如果我们从第一棵树开始，我们将只能收集到 [1, 2]。

示例 4：

输入：[3,3,3,1,2,1,1,2,3,3,4]
输出：5
解释：我们可以收集 [1,2,1,1,2]
如果我们从第一棵树或第八棵树开始，我们将只能收集到 4 棵水果树。

__提示:__

1 <= tree.length <= 40000
0 <= tree[i] < tree.length

__思路__:

滑动窗口
题目的实际含义是找到一个最多只含有 2 个元素的滑动窗口的最长长度
滑动窗口里的元素可以用哈希表或者两个指针存储
滑动窗口右边界向右滑动直到滑动窗口的元素个数多于 2 时, 滑动窗口左边界向右滑, 直到移除一个元素为止
时间复杂度为 O(n), 空间复杂度为 O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int totalFruit(vector<int>& fruits) 
    {
        int left = 0, right = 0, result = 0, cur = 0, n = fruits.size();
        for (int i = 0; i < n; i++)
        {
            if (fruits[i] != fruits[left] and fruits[i] != fruits[right])
            {
                if (left != right) left = cur;
                right = i;
            }
            result = max(result, i - left + 1);
            if (fruits[cur] != fruits[i]) cur = i;
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int totalFruit(int[] fruits) {
        int left = 0, right = 0, result = 0, cur = 0, n = fruits.length;
        for (int i = 0; i < n; i++)
        {
            if (fruits[i] != fruits[left] && fruits[i] != fruits[right])
            {
                if (left != right) left = cur;
                right = i;
            }
            result = Math.max(result, i - left + 1);
            if (fruits[cur] != fruits[i]) cur = i;
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def totalFruit(self, fruits: List[int]) -> int:
        basket, n, result, last = set(), len(fruits), 0, 0
        for i in range(n):
            basket.add(fruits[i])
            if len(basket) > 2:
                last = i - 2
                while fruits[last] == fruits[i - 1]:
                    last -= 1
                basket.remove(fruits[last])
                last += 1
            result = max(result, i - last + 1)
        return result
```
