# 1899 Merge Triplets to Form Target Triplet 合并若干三元组以形成目标三元组

__Description:__

A __triplet__ is an array of three integers. You are given a 2D integer array `triplets`, where `triplets[i] = [ai, bi, ci]` describes the `i ^ th` __triplet__. You are also given an integer array `target = [x, y, z]` that describes the __triplet__ you want to obtain.

To obtain `target`, you may apply the following operation on `triplets` __any number__ of times (possibly __zero__):

- Choose two indices (__0-indexed__) `i` and `j` ( `i != j`) and __update__ `triplets[j]` to become `[max(ai, aj), max(bi, bj), max(ci, cj)]`.

  - For example, if `triplets[i] = [2, 5, 3]` and `triplets[j] = [1, 7, 5]`, `triplets[j]` will be updated to `[max(2, 1), max(5, 7), max(3, 5)] = [2, 7, 5]`.

Return `true` _if it is possible to obtain the_ `target` ___triplet___ `[x, y, z]` _as an __element__ of_ `triplets`_, or_ `false` _otherwise_.

__Example:__

Example 1:

```text
Input: triplets = [[2,5,3],[1,8,4],[1,7,5]], target = [2,7,5]
Output: true
Explanation: Perform the following operations:
```

- Choose the first and last triplets [[2,5,3],[1,8,4],[1,7,5]]. Update the last triplet to be [max(2,1), max(5,7), max(3,5)] = [2,7,5]. triplets = [[2,5,3],[1,8,4],[2,7,5]]
The target triplet [2,7,5] is now an element of triplets.

Example 2:

```text
Input: triplets = [[3,4,5],[4,5,6]], target = [3,2,5]
Output: false
Explanation: It is impossible to have [3,2,5] as an element because there is no 2 in any of the triplets.
```

Example 3:

```text
Input: triplets = [[2,5,3],[2,3,4],[1,2,5],[5,2,3]], target = [5,5,5]
Output: true
Explanation: Perform the following operations:
```

- Choose the first and third triplets [[2,5,3],[2,3,4],[1,2,5],[5,2,3]]. Update the third triplet to be [max(2,1), max(5,2), max(3,5)] = [2,5,5]. triplets = [[2,5,3],[2,3,4],[2,5,5],[5,2,3]].
- Choose the third and fourth triplets [[2,5,3],[2,3,4],[2,5,5],[5,2,3]]. Update the fourth triplet to be [max(2,5), max(5,2), max(5,3)] = [5,5,5]. triplets = [[2,5,3],[2,3,4],[2,5,5],[5,5,5]].
The target triplet [5,5,5] is now an element of triplets

__Constraints:__

- `1 <= triplets.length <= 10 ^ 5`
- `triplets[i].length == target.length == 3`
- `1 <= ai, bi, ci, x, y, z <= 1000`

__题目描述:__

__三元组__ 是一个由三个整数组成的数组。给你一个二维整数数组 `triplets` ，其中 `triplets[i] = [ai, bi, ci]` 表示第 `i` 个 __三元组__ 。同时，给你一个整数数组 `target = [x, y, z]` ，表示你想要得到的 __三元组__ 。

为了得到 `target` ，你需要对 `triplets` 执行下面的操作 __任意次__（可能 __零__ 次）:

- 选出两个下标（下标 __从 0 开始__ 计数） `i` 和 `j`（ `i != j`），并 __更新__ `triplets[j]` 为 `[max(ai, aj), max(bi, bj), max(ci, cj)]` 。

  - 例如， `triplets[i] = [2, 5, 3]` 且 `triplets[j] = [1, 7, 5]`， `triplets[j]` 将会更新为 `[max(2, 1), max(5, 7), max(3, 5)] = [2, 7, 5]` 。

如果通过以上操作我们可以使得目标 __三元组__ `target` 成为 `triplets` 的一个 __元素__ ，则返回 `true` ；否则，返回 `false` 。

__示例:__

示例 1：

```text
输入：triplets = [[2,5,3],[1,8,4],[1,7,5]], target = [2,7,5]
输出：true
解释：执行下述操作：
```

- 选择第一个和最后一个三元组 [[2,5,3],[1,8,4],[1,7,5]] 。更新最后一个三元组为 [max(2,1), max(5,7), max(3,5)] = [2,7,5] 。triplets = [[2,5,3],[1,8,4],[2,7,5]]
目标三元组 [2,7,5] 现在是 triplets 的一个元素。

示例 2：

```text
输入：triplets = [[1,3,4],[2,5,8]], target = [2,5,8]
输出：true
解释：目标三元组 [2,5,8] 已经是 triplets 的一个元素。
```

示例 3：

```text
输入：triplets = [[2,5,3],[2,3,4],[1,2,5],[5,2,3]], target = [5,5,5]
输出：true
解释：执行下述操作：
```

- 选择第一个和第三个三元组 [[2,5,3],[2,3,4],[1,2,5],[5,2,3]] 。更新第三个三元组为 [max(2,1), max(5,2), max(3,5)] = [2,5,5] 。triplets = [[2,5,3],[2,3,4],[2,5,5],[5,2,3]] 。
- 选择第三个和第四个三元组 [[2,5,3],[2,3,4],[2,5,5],[5,2,3]] 。更新第四个三元组为 [max(2,5), max(5,2), max(5,3)] = [5,5,5] 。triplets = [[2,5,3],[2,3,4],[2,5,5],[5,5,5]] 。
目标三元组 [5,5,5] 现在是 triplets 的一个元素。

示例 4：

```text
输入：triplets = [[3,4,5],[4,5,6]], target = [3,2,5]
输出：false
解释：无法得到 [3,2,5] ，因为 triplets 不含 2 。
```

__提示：__

- `1 <= triplets.length <= 10 ^ 5`
- `triplets[i].length == target.length == 3`
- `1 <= ai, bi, ci, x, y, z <= 1000`

__思路:__

```text
贪心
找到所有每一个数都不大于结果的三元组
将结果更新为上述三元组的对应的最大值
最后判断结果是否等于目标
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    bool mergeTriplets(vector<vector<int>>& triplets, vector<int>& target) 
    {
        vector<int> result(3);
        for (const auto& t : triplets) 
        {
            if (t[0] <= target[0] and t[1] <= target[1] and t[2] <= target[2]) 
            {
                result[0] = max(result[0], t[0]);
                result[1] = max(result[1], t[1]);
                result[2] = max(result[2], t[2]);
            }
        }
        return result == target;
    }
};
```

__Java__:

```Java
class Solution {
    public boolean mergeTriplets(int[][] triplets, int[] target) {
        int result[] = new int[3];
        for (int[] t : triplets) {
            if (t[0] <= target[0] && t[1] <= target[1] && t[2] <= target[2]) {
                result[0] = Math.max(result[0], t[0]);
                result[1] = Math.max(result[1], t[1]);
                result[2] = Math.max(result[2], t[2]);
            }
        }
        return result[0] == target[0] && result[1] == target[1] && result[2] == target[2];
    }
}
```

__Python__:

```Python
class Solution:
    def mergeTriplets(self, triplets: List[List[int]], target: List[int]) -> bool:
        result = [0] * 3
        for t in triplets:
            if all(t[i] <= target[i] for i in range(3)):
                result = [max(result[i], t[i]) for i in range(3)]
        return result == target
```
