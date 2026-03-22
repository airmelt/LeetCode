# 1998 GCD Sort of an Array 数组的最大公因数排序

__Description:__

You are given an integer array `nums`, and you can perform the following operation __any__ number of times on `nums`:

- Swap the positions of two elements `nums[i]` and `nums[j]` if `gcd(nums[i], nums[j]) > 1` where `gcd(nums[i], nums[j])` is the __greatest common divisor__ of `nums[i]` and `nums[j]`.

Return `true` _if it is possible to sort_ `nums` _in __non-decreasing__ order using the above swap method, or_ `false` _otherwise._

__Example:__

Example 1:

```text
Input: nums = [7,21,3]
Output: true
Explanation: We can sort [7,21,3] by performing the following operations:
```

- Swap 7 and 21 because gcd(7,21) = 7. nums = [21,7,3]
- Swap 21 and 3 because gcd(21,3) = 3. nums = [3,7,21]

Example 2:

```text
Input: nums = [5,2,6,2]
Output: false
Explanation: It is impossible to sort the array because 5 cannot be swapped with any other element.
```

Example 3:

```text
Input: nums = [10,5,9,3,15]
Output: true
We can sort [10,5,9,3,15] by performing the following operations:
```

- Swap 10 and 15 because gcd(10,15) = 5. nums = [15,5,9,3,10]
- Swap 15 and 3 because gcd(15,3) = 3. nums = [3,5,9,15,10]
- Swap 10 and 15 because gcd(10,15) = 5. nums = [3,5,9,10,15]

__Constraints:__

- `1 <= nums.length <= 3 * 10 ^ 4`
- `2 <= nums[i] <= 10 ^ 5`

__题目描述:__

给你一个整数数组 `nums` ，你可以在 `nums` 上执行下述操作 __任意次__ :

- 如果 `gcd(nums[i], nums[j]) > 1` ，交换 `nums[i]` 和 `nums[j]` 的位置。其中 `gcd(nums[i], nums[j])` 是 `nums[i]` 和 `nums[j]` 的最大公因数。

如果能使用上述交换方式将 `nums` 按 __非递减顺序__ 排列，返回 `true` ；否则，返回 `false` 。

__示例:__

示例 1：

```text
输入：nums = [7,21,3]
输出：true
解释：可以执行下述操作完成对 [7,21,3] 的排序：
```

- 交换 7 和 21 因为 gcd(7,21) = 7 。nums = [21,7,3]
- 交换 21 和 3 因为 gcd(21,3) = 3 。nums = [3,7,21]

示例 2：

```text
输入：nums = [5,2,6,2]
输出：false
解释：无法完成排序，因为 5 不能与其他元素交换。
```

示例 3：

```text
输入：nums = [10,5,9,3,15]
输出：true
解释：
可以执行下述操作完成对 [10,5,9,3,15] 的排序：
```

- 交换 10 和 15 因为 gcd(10,15) = 5 。nums = [15,5,9,3,10]
- 交换 15 和 3 因为 gcd(15,3) = 3 。nums = [3,5,9,15,10]
- 交换 10 和 15 因为 gcd(10,15) = 5 。nums = [3,5,9,10,15]

__提示：__

- `1 <= nums.length <= 3 * 10 ^ 4`
- `2 <= nums[i] <= 10 ^ 5`

__思路:__

```text
并查集
将 gcd 不为 1 的数进行合并
排序之后与原数组进行比较
如果一一对应在并查集中, 则返回 true
否则返回 false
时间复杂度为 O(N), 空间复杂度为 O(N), 其中 N 为数组元素范围
```

__代码:__

__C++__:

```C++

class UF 
{
public:
    static constexpr int N = 100010;
    int parent[N];
    int weight[N];

    UF() 
    {
        for (int i = 0; i < N; i++) 
        {
            parent[i] = i;
            weight[i] = 1;
        }
    }

    int find(int p) 
    {
        return parent[p] == p ? p : (parent[p] = find(parent[p]));
    }

    void connect(int p, int q) 
    {
        if ((p = find(p)) == (q = find(q))) return;
        if (weight[p] > weight[q]) 
        {
            parent[q] = p;
            weight[p] += weight[q];
        } 
        else 
        {
            parent[p] = q;
            weight[q] += weight[p];
        }
    }

    bool connected(int p, int q) 
    {
        return find(p) == find(q);
    }
};
class Solution 
{
public:
    bool gcdSort(vector<int>& nums) 
    {
        UF uf = UF();
        for (const auto& num : nums) 
        {
            int a = 2;
            while (a * a <= num) 
            {
                if (!(num % a)) 
                {
                    uf.connect(num, a);
                    uf.connect(num, num / a);
                }
                ++a;
            }
        }
        vector<int> target = nums;
        sort(target.begin(), target.end());
        for (int i = 0, n = nums.size(); i < n; i++) if (!uf.connected(target[i], nums[i])) return false;
        return true;
    }
};
```

__Java__:

```Java
class UF {
    private static final int N = 100_010;
    public int[] parent = new int[N];
    public int[] weight = new int[N];
    
    public UF() {
        for (int i = 1; i < N; i++) parent[i] = i;
        Arrays.fill(weight, 1);
    }

    public int find(int p) {
        return parent[p] == p ? p : (parent[p] = find(parent[p]));
    }

    public void connect(int p, int q) {
        if ((p = find(p)) == (q = find(q))) return;
        if (weight[p] > weight[q]) {
            parent[q] = p;
            weight[p] += weight[q];
        } else {
            parent[p] = q;
            weight[q] += weight[p];
        }
    }

    public boolean connected(int p, int q) {
        return find(p) == find(q);
    }
}
class Solution {
    public boolean gcdSort(int[] nums) {
        UF uf = new UF();
        for (int num : nums) {
            int a = 2;
            while (a * a <= num) {
                if (num % a == 0) {
                    uf.connect(num, a);
                    uf.connect(num, num / a);
                }
                ++a;
            }
        }
        int n = nums.length, target[] = Arrays.copyOfRange(nums, 0, n);
        Arrays.sort(target);
        for (int i = 0; i < n; i++) if (!uf.connected(target[i], nums[i])) return false;
        return true;
    }
}
```

__Python__:

```Python
class UF:
    def __init__(self, n: int) -> None:
        self.parent = [i for i in range(n)]
        self.weight = [1] * n
        self.count = n

    def union(self, p: int, q: int) -> None:
        """
        连接两个点
        :param p: 一个节点
        :param q: 另一个节点
        :return:
        """
        p = self.find(p)
        q = self.find(q)
        if p == q:
            return
        if self.weight[p] > self.weight[q]:
            self.parent[q] = p
            self.weight[p] += self.weight[q]
        else:
            self.parent[p] = q
            self.weight[q] += self.weight[p]
        self.count -= 1

    def connected(self, p: int, q: int) -> bool:
        """
        检查两个点是否在同一分量
        :param p: 一个节点
        :param q: 另一个节点
        :return: 返回两个点是否在同一个分量
        """
        return self.find(p) == self.find(q)

    def find(self, p: int) -> int:
        """
        查找根节点, 并进行路径压缩
        :param p: 一个节点
        :return: 根节点
        """
        while self.parent[p] != p:
            self.parent[p] = self.parent[self.parent[p]]
            p = self.parent[p]
        return p

class Solution:
    def gcdSort(self, nums: List[int]) -> bool:
        uf, n, target = UF(10 ** 5 + 1), len(nums), sorted(nums)
        for num in nums:
            a = 2
            while a ** 2 <= num:
                if not num % a:
                    uf.union(num, a)
                    uf.union(num, num // a)
                a += 1
        return all(uf.connected(nums[i], target[i]) for i in range(n))
```
