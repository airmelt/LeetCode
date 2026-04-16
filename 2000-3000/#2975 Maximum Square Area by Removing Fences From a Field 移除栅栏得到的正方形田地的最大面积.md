# 2975 Maximum Square Area by Removing Fences From a Field 移除栅栏得到的正方形田地的最大面积

__Description:__

There is a large `(m - 1) x (n - 1)` rectangular field with corners at `(1, 1)` and `(m, n)` containing some horizontal and vertical fences given in arrays `hFences` and `vFences` respectively.

Horizontal fences are from the coordinates `(hFences[i], 1)` to `(hFences[i], n)` and vertical fences are from the coordinates `(1, vFences[i])` to `(m, vFences[i])`.

Return _the __maximum__ area of a __square__ field that can be formed by __removing__ some fences (__possibly none__) or_ `-1` _if it is impossible to make a square field_.

Since the answer may be large, return it __modulo__ `10 ^ 9 + 7`.

__Note:__ The field is surrounded by two horizontal fences from the coordinates `(1, 1)` to `(1, n)` and `(m, 1)` to `(m, n)` and two vertical fences from the coordinates `(1, 1)` to `(m, 1)` and `(1, n)` to `(m, n)`. These fences __cannot__ be removed.

__Example:__

Example 1:

![2975-1](https://assets.leetcode.com/uploads/2023/11/05/screenshot-from-2023-11-05-22-40-25.png)

```text
Input: m = 4, n = 3, hFences = [2,3], vFences = [2]
Output: 4
Explanation: Removing the horizontal fence at 2 and the vertical fence at 2 will give a square field of area 4.
```

Example 2:

![2975-2](https://assets.leetcode.com/uploads/2023/11/22/maxsquareareaexample1.png)

```text
Input: m = 6, n = 7, hFences = [2], vFences = [4]
Output: -1
Explanation: It can be proved that there is no way to create a square field by removing fences.
```

__Constraints:__

- `3 <= m, n <= 10 ^ 9`
- `1 <= hFences.length, vFences.length <= 600`
- `1 < hFences[i] < m`
- `1 < vFences[i] < n`
- `hFences` and  `vFences` are unique.

__题目描述:__

有一个大型的 `(m - 1) x (n - 1)` 矩形田地，其两个对角分别是 `(1, 1)` 和 `(m, n)` ，田地内部有一些水平栅栏和垂直栅栏，分别由数组 `hFences` 和 `vFences` 给出。

水平栅栏为坐标 `(hFences[i], 1)` 到 `(hFences[i], n)`，垂直栅栏为坐标 `(1, vFences[i])` 到 `(m, vFences[i])` 。

返回通过 __移除__ 一些栅栏（__可能不移除__）所能形成的最大面积的 __正方形__ 田地的面积，或者如果无法形成正方形田地则返回 `-1`。

由于答案可能很大，所以请返回结果对 `10 ^ 9 + 7` __取余__ 后的值。

__注意:__田地外围两个水平栅栏（坐标 `(1, 1)` 到 `(1, n)` 和坐标 `(m, 1)` 到 `(m, n)` ）以及两个垂直栅栏（坐标 `(1, 1)` 到 `(m, 1)` 和坐标 `(1, n)` 到 `(m, n)` ）所包围。这些栅栏 __不能__ 被移除。

__示例:__

示例 1：

![2975-3](https://assets.leetcode.com/uploads/2023/11/05/screenshot-from-2023-11-05-22-40-25.png)

```text
输入：m = 4, n = 3, hFences = [2,3], vFences = [2]
输出：4
解释：移除位于 2 的水平栅栏和位于 2 的垂直栅栏将得到一个面积为 4 的正方形田地。
```

示例 2：

![2975-4](https://assets.leetcode.com/uploads/2023/11/22/maxsquareareaexample1.png)

```text
输入：m = 6, n = 7, hFences = [2], vFences = [4]
输出：-1
解释：可以证明无法通过移除栅栏形成正方形田地。
```

__提示：__

- `3 <= m, n <= 10 ^ 9`
- `1 <= hFences.length, vFences.length <= 600`
- `1 < hFences[i] < m`
- `1 < vFences[i] < n`
- `hFences` 和 `vFences` 中的元素是唯一的。

__思路:__

```text
贪心
将 1 和 m 塞进 hFences
将 1 和 n 塞进 vFences
然后对两个数组排序
排序之后暴力查询两个元素之间的距离
可以将这个距离放入哈希表中
两个哈希表交集的最大值就是正方形的最大边长
返回这个边长的平方, 注意需要取模
如果哈希表交集为空答案就为 -1
时间复杂度为 O(H ^ 2 + V ^ 2), 空间复杂度为 O(H ^ 2 + V ^ 2), 其中 H, V 分别为 hFences, vFences 的长度
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int maximizeSquareArea(int m, int n, vector<int>& hFences, vector<int>& vFences) 
    {
        hFences.emplace_back(1);
        hFences.emplace_back(m);
        sort(hFences.begin(), hFences.end());
        vFences.emplace_back(1);
        vFences.emplace_back(n);
        sort(vFences.begin(), vFences.end());
        unordered_set<int> s;
        for (int i = 1, p = hFences.size(); i < p; i++) for (int j = 0; j < i; j++) s.insert(hFences[i] - hFences[j]);
        int result = -1, c = 0, MOD = 1e9 + 7;
        for (int i = 1, q = vFences.size(); i < q; i++) for (int j = 0; j < i; j++) if (s.count(c = vFences[i] - vFences[j])) result = max(result, c);
        return ~result ? ((long long)result * result % MOD) : result;
    }
};
```

__Java__:

```Java
class Solution {
    public int maximizeSquareArea(int m, int n, int[] hFences, int[] vFences) {
        List<Integer> h = Arrays.stream(hFences).boxed().collect(Collectors.toCollection(ArrayList::new)), v = Arrays.stream(vFences).boxed().collect(Collectors.toCollection(ArrayList::new));
        h.add(1);
        h.add(m);
        Collections.sort(h);
        v.add(1);
        v.add(n);
        Collections.sort(v);
        var set = new HashSet<Integer>();
        for (int i = 1, p = h.size(); i < p; i++) for (int j = 0; j < i; j++) set.add(h.get(i) - h.get(j));
        int result = -1, c = 0, MOD = 1_000_000_007;
        for (int i = 1, q = v.size(); i < q; i++) for (int j = 0; j < i; j++) if (set.contains(c = v.get(i) - v.get(j))) result = Math.max(result, c);
        return result > 0 ? (int)((long)result * result % MOD) : result;
    }
}
```

__Python__:

```Python
class Solution:
    def maximizeSquareArea(self, m: int, n: int, hFences: List[int], vFences: List[int]) -> int:
        return result ** 2 % (10 ** 9 + 7) if (result := max(set(y - x for x, y in combinations(sorted([1] + hFences + [m]), 2)) & set(y - x for x, y in combinations(sorted([1] + vFences + [n]), 2)), default=0)) else -1
```
