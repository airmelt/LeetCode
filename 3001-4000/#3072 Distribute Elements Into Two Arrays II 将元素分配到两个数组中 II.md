# 3072 Distribute Elements Into Two Arrays II 将元素分配到两个数组中 II

__Description:__

You are given a __1-indexed__ array of integers `nums` of length `n`.

We define a function `greaterCount` such that `greaterCount(arr, val)` returns the number of elements in `arr` that are __strictly greater__ than `val`.

You need to distribute all the elements of `nums` between two arrays `arr1` and `arr2` using `n` operations. In the first operation, append `nums[1]` to `arr1`. In the second operation, append `nums[2]` to `arr2`. Afterwards, in the `i ^ th` operation:

- If `greaterCount(arr1, nums[i]) > greaterCount(arr2, nums[i])`, append `nums[i]` to `arr1`.
- If `greaterCount(arr1, nums[i]) < greaterCount(arr2, nums[i])`, append `nums[i]` to `arr2`.
- If `greaterCount(arr1, nums[i]) == greaterCount(arr2, nums[i])`, append `nums[i]` to the array with a __lesser__ number of elements.
- If there is still a tie, append `nums[i]` to `arr1`.

The array `result` is formed by concatenating the arrays `arr1` and `arr2`. For example, if `arr1 == [1,2,3]` and `arr2 == [4,5,6]`, then `result = [1,2,3,4,5,6]`.

Return _the integer array_ `result`.

__Example:__

Example 1:

```text
Input: nums = [2,1,3,3]
Output: [2,3,1,3]
Explanation: After the first 2 operations, arr1 = [2] and arr2 = [1].
In the 3rd operation, the number of elements greater than 3 is zero in both arrays. Also, the lengths are equal, hence, append nums[3] to arr1.
In the 4th operation, the number of elements greater than 3 is zero in both arrays. As the length of arr2 is lesser, hence, append nums[4] to arr2.
After 4 operations, arr1 = [2,3] and arr2 = [1,3].
Hence, the array result formed by concatenation is [2,3,1,3].
```

Example 2:

```text
Input: nums = [5,14,3,1,2]
Output: [5,3,1,2,14]
Explanation: After the first 2 operations, arr1 = [5] and arr2 = [14].
In the 3rd operation, the number of elements greater than 3 is one in both arrays. Also, the lengths are equal, hence, append nums[3] to arr1.
In the 4th operation, the number of elements greater than 1 is greater in arr1 than arr2 (2 > 1). Hence, append nums[4] to arr1.
In the 5th operation, the number of elements greater than 2 is greater in arr1 than arr2 (2 > 1). Hence, append nums[5] to arr1.
After 5 operations, arr1 = [5,3,1,2] and arr2 = [14].
Hence, the array result formed by concatenation is [5,3,1,2,14].
```

Example 3:

```text
Input: nums = [3,3,3,3]
Output: [3,3,3,3]
Explanation: At the end of 4 operations, arr1 = [3,3] and arr2 = [3,3].
Hence, the array result formed by concatenation is [3,3,3,3].
```

__Constraints:__

- `3 <= n <= 10 ^ 5`
- `1 <= nums[i] <= 10 ^ 9`

__题目描述:__

给你一个下标从 __1__ 开始、长度为 `n` 的整数数组 `nums` 。

现定义函数 `greaterCount` ，使得 `greaterCount(arr, val)` 返回数组 `arr` 中 __严格大于__ `val` 的元素数量。

你需要使用 `n` 次操作，将 `nums` 的所有元素分配到两个数组 `arr1` 和 `arr2` 中。在第一次操作中，将 `nums[1]` 追加到 `arr1` 。在第二次操作中，将 `nums[2]` 追加到 `arr2` 。之后，在第 `i` 次操作中:

- 如果 `greaterCount(arr1, nums[i]) > greaterCount(arr2, nums[i])` ，将 `nums[i]` 追加到 `arr1` 。
- 如果 `greaterCount(arr1, nums[i]) < greaterCount(arr2, nums[i])` ，将 `nums[i]` 追加到 `arr2` 。
- 如果 `greaterCount(arr1, nums[i]) == greaterCount(arr2, nums[i])` ，将 `nums[i]` 追加到元素数量较少的数组中。
- 如果仍然相等，那么将 `nums[i]` 追加到 `arr1` 。

连接数组 `arr1` 和 `arr2` 形成数组 `result` 。例如，如果 `arr1 == [1,2,3]` 且 `arr2 == [4,5,6]` ，那么 `result = [1,2,3,4,5,6]` 。

返回整数数组 `result` 。

__示例:__

示例 1：

```text
输入：nums = [2,1,3,3]
输出：[2,3,1,3]
解释：在前两次操作后，arr1 = [2] ，arr2 = [1] 。
在第 3 次操作中，两个数组中大于 3 的元素数量都是零，并且长度相等，因此，将 nums[3] 追加到 arr1 。
在第 4 次操作中，两个数组中大于 3 的元素数量都是零，但 arr2 的长度较小，因此，将 nums[4] 追加到 arr2 。
在 4 次操作后，arr1 = [2,3] ，arr2 = [1,3] 。
因此，连接形成的数组 result 是 [2,3,1,3] 。
```

示例 2：

```text
输入：nums = [5,14,3,1,2]
输出：[5,3,1,2,14]
解释：在前两次操作后，arr1 = [5] ，arr2 = [14] 。
在第 3 次操作中，两个数组中大于 3 的元素数量都是一，并且长度相等，因此，将 nums[3] 追加到 arr1 。
在第 4 次操作中，arr1 中大于 1 的元素数量大于 arr2 中的数量（2 > 1），因此，将 nums[4] 追加到 arr1 。
在第 5 次操作中，arr1 中大于 2 的元素数量大于 arr2 中的数量（2 > 1），因此，将 nums[5] 追加到 arr1 。
在 5 次操作后，arr1 = [5,3,1,2] ，arr2 = [14] 。
因此，连接形成的数组 result 是 [5,3,1,2,14] 。
```

示例 3：

```text
输入：nums = [3,3,3,3]
输出：[3,3,3,3]
解释：在 4 次操作后，arr1 = [3,3] ，arr2 = [3,3] 。
因此，连接形成的数组 result 是 [3,3,3,3] 。
```

__提示：__

- `3 <= n <= 10 ^ 5`
- `1 <= nums[i] <= 10 ^ 9`

__思路:__

```text
1. 二分法
使用有序数组
查找数组元素在有序数组中的位置
按照题意分别插入对应的有序数组和 arr 中
最后将两个 arr 数组合并即可
时间复杂度为 O(NlogN), 空间复杂度为 O(N)
2. 树状数组
将数组元素映射到 [1, n] 区间, 离散化得到 sorted 数组
每次将元素插入到树状数组中
如果加入到 arr1 就加 1, 否则减 1
二分 sorted 数组的位置 pos
然后在树状数组中查询这个位置
如果这个位置大于 0 或者等于 0 的时候 arr1 的大小不大于 arr2 的大小
就加入 arr1
否则加入 arr2
时间复杂度为 O(NlogN), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class BIT
{
public:
    BIT(int _n) : n(_n), tree(_n) {}

    void update(int x, int v) 
    {
        while (x < n) 
        {
            tree[x] += v;
            x += lowbit(x);
        }
    }

    int query(int x) 
    {
        int result = 0;
        while (x) 
        {
            result += tree[x];
            x -= lowbit(x);
        }
        return result;
    }
private:
    vector<int> tree;
    int n;

    int lowbit(int x) 
    {
        return x & -x;
    } 
};

class Solution 
{
public:
    vector<int> resultArray(vector<int>& nums) 
    {
        vector<int> sorted = nums, a{ nums[0]}, b{ nums[1] };
        sort(sorted.begin(), sorted.end());
        BIT t(nums.size() + 1);
        t.update(sorted.end() - lower_bound(sorted.begin(), sorted.end(), nums[0]), 1);
        t.update(sorted.end() - lower_bound(sorted.begin(), sorted.end(), nums[1]), -1);
        for (int i = 2, v = 0, pos = 0, diff = 0, n = nums.size(); i < n; i++) 
        {
            if ((diff = t.query((pos = sorted.end() - lower_bound(sorted.begin(), sorted.end(), v = nums[i])) - 1)) > 0 or !diff and a.size() <= b.size()) 
            {
                a.emplace_back(v);
                t.update(pos, 1);
            } 
            else 
            {
                b.emplace_back(v);
                t.update(pos, -1);
            }
        }
        a.insert(a.end(), b.begin(), b.end());
        return a;
    }
};
```

__Java__:

```Java
class BIT {
    private final int[] tree;
    private int n;

    public BIT(int n) {
        this.n = n;
        tree = new int[n];
    }

    public void update(int x, int v) {
        while (x < n) {
            tree[x] += v;
            x += lowbit(x);
        }
    }

    private int lowbit(int x) {
        return x & -x;
    } 

    public int query(int x) {
        int result = 0;
        while (x > 0) {
            result += tree[x];
            x -= lowbit(x);
        }
        return result;
    }
}

class Solution {
    public int[] resultArray(int[] nums) {
        int n = nums.length, sorted[] = nums.clone();
        Arrays.sort(sorted);
        List<Integer> a = new ArrayList<>(n){{ add(nums[0]); }}, b = new ArrayList<>(n){{ add(nums[1]); }};
        BIT t = new BIT(n + 1);
        t.update(n - Arrays.binarySearch(sorted, nums[0]), 1);
        t.update(n - Arrays.binarySearch(sorted, nums[1]), -1);
        for (int i = 2, v = 0, pos = 0, diff = 0; i < n; i++) {
            if ((diff = t.query((pos = n - Arrays.binarySearch(sorted, v = nums[i])) - 1)) > 0 || diff == 0 && a.size() <= b.size()) {
                a.add(v);
                t.update(pos, 1);
            } else {
                b.add(v);
                t.update(pos, -1);
            }
        }
        a.addAll(b);
        return a.stream().mapToInt(i -> i).toArray();
    }
}
```

__Python__:

```Python
from sortedcontainers import SortedList

class Solution:
    def resultArray(self, nums: List[int]) -> List[int]:
        sl1, sl2 = SortedList(arr1 := [nums[0]]), SortedList(arr2 := [nums[1]])
        for num in nums[2:]:
            if (c1 := len(sl1) - sl1.bisect_right(num)) > (c2 := len(sl2) - sl2.bisect_right(num)) or c1 == c2 and len(sl1) <= len(sl2):
                sl1.add(num)
                arr1.append(num)
            else:
                sl2.add(num)
                arr2.append(num)
        return arr1 + arr2
```
