# 1982 Find Array Given Subset Sums 从子集的和还原数组

__Description:__

You are given an integer `n` representing the length of an unknown array that you are trying to recover. You are also given an array `sums` containing the values of all `2 ^ n` __subset sums__ of the unknown array (in no particular order).

Return _the array_ `ans` _of length_ `n` _representing the unknown array. If __multiple__ answers exist, return __any__ of them_.

An array `sub` is a __subset__ of an array `arr` if `sub` can be obtained from `arr` by deleting some (possibly zero or all) elements of `arr`. The sum of the elements in `sub` is one possible __subset sum__ of `arr`. The sum of an empty array is considered to be `0`.

Note: Test cases are generated such that there will always be at least one correct answer.

__Example:__

Example 1:

```text
Input: n = 3, sums = [-3,-2,-1,0,0,1,2,3]
Output: [1,2,-3]
Explanation: [1,2,-3] is able to achieve the given subset sums:
```

- []: sum is 0
- [1]: sum is 1
- [2]: sum is 2
- [1,2]: sum is 3
- [-3]: sum is -3
- [1,-3]: sum is -2
- [2,-3]: sum is -1
- [1,2,-3]: sum is 0
Note that any permutation of [1,2,-3] and also any permutation of [-1,-2,3] will also be accepted.

Example 2:

```text
Input: n = 2, sums = [0,0,0,0]
Output: [0,0]
Explanation: The only correct answer is [0,0].
```

Example 3:

```text
Input: n = 4, sums = [0,0,5,5,4,-1,4,9,9,-1,4,3,4,8,3,8]
Output: [0,-1,4,5]
Explanation: [0,-1,4,5] is able to achieve the given subset sums.
```

__Constraints:__

- `1 <= n <= 15`
- `sums.length == 2 ^ n`
- `-10 ^ 4 <= sums[i] <= 10 ^ 4`

__题目描述:__

存在一个未知数组需要你进行还原，给你一个整数 `n` 表示该数组的长度。另给你一个数组 `sums` ，由未知数组中全部 `2 ^ n` 个 __子集的和__ 组成（子集中的元素没有特定的顺序）。

返回一个长度为 `n` 的数组 `ans` 表示还原得到的未知数组。如果存在 __多种__ 答案，只需返回其中 __任意一个__ 。

如果可以由数组 `arr` 删除部分元素（也可能不删除或全删除）得到数组 `sub` ，那么数组 `sub` 就是数组 `arr` 的一个 __子集__ 。 `sub` 的元素之和就是 `arr` 的一个 __子集的和__ 。一个空数组的元素之和为 `0` 。

注意：生成的测试用例将保证至少存在一个正确答案。

__示例:__

示例 1：

```text
输入：n = 3, sums = [-3,-2,-1,0,0,1,2,3]
输出：[1,2,-3]
解释：[1,2,-3] 能够满足给出的子集的和：
```

- []：和是 0
- [1]：和是 1
- [2]：和是 2
- [1,2]：和是 3
- [-3]：和是 -3
- [1,-3]：和是 -2
- [2,-3]：和是 -1
- [1,2,-3]：和是 0
注意，[1,2,-3] 的任何排列和 [-1,-2,3] 的任何排列都会被视作正确答案。

示例 2：

```text
输入：n = 2, sums = [0,0,0,0]
输出：[0,0]
解释：唯一的正确答案是 [0,0] 。
```

示例 3：

```text
输入：n = 4, sums = [0,0,5,5,4,-1,4,9,9,-1,4,3,4,8,3,8]
输出：[0,-1,4,5]
解释：[0,-1,4,5] 能够满足给出的子集的和。
```

__提示：__

- `1 <= n <= 15`
- `sums.length == 2 ^ n`
- `-10 ^ 4 <= sums[i] <= 10 ^ 4`

__思路:__

```text
贪心
如果数组中只有正数
可以通过以下方式进行还原
1. 去掉空集的和 0
2. 找到数组中的最小值 min_value, 该值为还原数组的最小值
3. 将已经确定的元素从 sums 中去掉
重复 2-3 步骤直到还原数组的长度为 n
如果数组中有负数
可以通过以下方式进行还原
1. 找到数组中的最小值 min_value
2. 将数组中的每个元素减去 -min_value, 这样所有的元素都变成了非负数
3. 通过上述正数的方式进行还原
4. 如果还原数组的和为 -min_value, 则将还原数组中的所有元素取反
时间复杂度为 O(N * 2 ^ N), 空间复杂度为 O(2 ^ N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<int> recoverArray(int n, vector<int>& sums) 
    {
        int min_value = *min_element(sums.begin(), sums.end());
        multiset<int> st;
        for (int x : sums) st.insert(x - min_value);
        st.erase(st.begin());
        vector<int> result;
        result.emplace_back(*st.begin());
        for (int i = 1; i < n; i++)
        {
            for (int mask = 0, total = 1 << i; mask < total; mask++)
            {
                if (mask >> (i - 1) & 1)
                {
                    int cur = 0;
                    for (int j = 0; j < i; j++) if (mask >> j & 1) cur += result[j];
                    st.erase(st.find(cur));
                }
            }
            result.emplace_back(*st.begin());
        }
        for (int i = 0, total = 1 << n; i < total; i++)
        {
            int cur = 0;
            for (int j = 0; j < n; j++) if (i >> j & 1) cur += result[j];
            if (cur == -min_value)
            {
                for (int j = 0; j < n; j++) if (i >> j & 1) result[j] = -result[j];
                break;
            }
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int[] recoverArray(int n, int[] sums) {
        int minValue = Arrays.stream(sums).min().getAsInt();
        TreeMap<Integer, Integer> map = new TreeMap<>();
        for (int x : sums) map.merge(x - minValue, 1, Integer::sum);
        int result[] = new int[n], minKey = map.firstKey(), pos = 0;
        map.merge(minKey, -1, Integer::sum);
        if (map.get(minKey) == 0) map.remove(minKey);
        result[pos++] = map.firstKey();
        for (int i = 1; i < n; i++) {
            for (int mask = 0, total = 1 << i; mask < total; mask++) {
                if (((mask >> (i - 1)) & 1) == 1) {
                    int cur = 0;
                    for (int j = 0; j < i; j++) if (((mask >> j) & 1) == 1) cur += result[j];
                    map.merge(cur, -1, Integer::sum);
                    if (map.get(cur) == 0) map.remove(cur);
                }
            }
            result[pos++] = map.firstKey();
        }
        for (int i = 0, total = 1 << n; i < total; i++) {
            int cur = 0;
            for (int j = 0; j < n; j++) if (((i >> j) & 1) == 1) cur += result[j];
            if (cur == -minValue) {
                for (int j = 0; j < n; j++) if (((i >> j) & 1) == 1) result[j] = -result[j];
                break;
            }
        }
        return result;
    }
}
```

__Python__:

```Python
from sortedcontainers import SortedList
class Solution:
    def recoverArray(self, n: int, sums: List[int]) -> List[int]:
        min_value = min(sums)
        result, st = [], SortedList([x - min_value for x in sums])
        st.remove(0)
        result.append(st[0])
        for i in range(1, n):
            for mask in range(1 << i):
                if (mask >> (i - 1)) & 1:
                    st.remove(sum(result[j] for j in range(i) if (mask >> j) & 1))
            result.append(st[0])
        for i in range(1 << n):
            if sum(result[j] for j in range(n) if (i >> j) & 1) == -min_value:
                result = [-r if (i >> j) & 1 else r for j, r in enumerate(result)]
                break
        return result
```
