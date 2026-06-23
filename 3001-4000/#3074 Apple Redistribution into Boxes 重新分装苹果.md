# 3074 Apple Redistribution into Boxes 重新分装苹果

__Description:__

You are given an array `apple` of size `n` and an array `capacity` of size `m`.

There are `n` packs where the `i ^ th` pack contains `apple[i]` apples. There are `m` boxes as well, and the `i ^ th` box has a capacity of `capacity[i]` apples.

Return _the __minimum__ number of boxes you need to select to redistribute these_ `n` _packs of apples into boxes_.

Note that, apples from the same pack can be distributed into different boxes.

__Example:__

Example 1:

```text
Input: apple = [1,3,2], capacity = [4,3,1,5,2]
Output: 2
Explanation: We will use boxes with capacities 4 and 5.
It is possible to distribute the apples as the total capacity is greater than or equal to the total number of apples.
```

Example 2:

```text
Input: apple = [5,5,5], capacity = [2,4,2,7]
Output: 4
Explanation: We will need to use all the boxes.
```

__Constraints:__

- `1 <= n == apple.length <= 50`
- `1 <= m == capacity.length <= 50`
- `1 <= apple[i], capacity[i] <= 50`
- The input is generated such that it's possible to redistribute packs of apples into boxes.

__题目描述:__

给你一个长度为 `n` 的数组 `apple` 和另一个长度为 `m` 的数组 `capacity` 。

一共有 `n` 个包裹，其中第 `i` 个包裹中装着 `apple[i]` 个苹果。同时，还有 `m` 个箱子，第 `i` 个箱子的容量为 `capacity[i]` 个苹果。

请你选择一些箱子来将这 `n` 个包裹中的苹果重新分装到箱子中，返回你需要选择的箱子的 __最小__ 数量。

注意，同一个包裹中的苹果可以分装到不同的箱子中。

__示例:__

示例 1：

```text
输入：apple = [1,3,2], capacity = [4,3,1,5,2]
输出：2
解释：使用容量为 4 和 5 的箱子。
总容量大于或等于苹果的总数，所以可以完成重新分装。
```

示例 2：

```text
输入：apple = [5,5,5], capacity = [2,4,2,7]
输出：4
解释：需要使用所有箱子。
```

__提示：__

- `1 <= n == apple.length <= 50`
- `1 <= m == capacity.length <= 50`
- `1 <= apple[i], capacity[i] <= 50`
- 输入数据保证可以将包裹中的苹果重新分装到箱子中。

__思路:__

```text
贪心
将 capacity 从大到小排序
一直装苹果直到装完
时间复杂度为 O(NlogN), 空间复杂度为 O(logN)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int minimumBoxes(vector<int>& apple, vector<int>& capacity) 
    {
        sort(capacity.begin(), capacity.end());
        int s = accumulate(apple.begin(), apple.end(), 0), m = capacity.size(), i = m - 1;
        while (s > 0) s -= capacity[i--];
        return m - i - 1;
    }
};
```

__Java__:

```Java
class Solution {
    public int minimumBoxes(int[] apple, int[] capacity) {
        Arrays.sort(capacity);
        int s = Arrays.stream(apple).sum(), m = capacity.length, i = m - 1;
        while (s > 0) s -= capacity[i--];
        return m - i - 1;
    }
}
```

__Python__:

```Python
class Solution:
    def minimumBoxes(self, apple: List[int], capacity: List[int]) -> int:
        return bisect_left(list(accumulate(sorted(capacity, reverse=True), initial=0)), sum(apple))
```
