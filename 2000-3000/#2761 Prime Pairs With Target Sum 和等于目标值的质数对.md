# 2761 Prime Pairs With Target Sum 和等于目标值的质数对

__Description:__

You are given an integer `n`. We say that two integers `x` and `y` form a prime number pair if:

- `1 <= x <= y <= n`
- `x + y == n`
- `x` and `y` are prime numbers

Return _the 2D sorted list of prime number pairs_ `[xi, yi]`. The list should be sorted in __increasing__ order of `xi`. If there are no prime number pairs at all, return _an empty array_.

__Note:__ A prime number is a natural number greater than `1` with only two factors, itself and `1`.

__Example:__

Example 1:

```text
Input: n = 10
Output: [[3,7],[5,5]]
Explanation: In this example, there are two prime pairs that satisfy the criteria. 
These pairs are [3,7] and [5,5], and we return them in the sorted order as described in the problem statement.
```

Example 2:

```text
Input: n = 2
Output: []
Explanation: We can show that there is no prime number pair that gives a sum of 2, so we return an empty array.
```

__Constraints:__

- `1 <= n <= 10 ^ 6`

__题目描述:__

给你一个整数 `n` 。如果两个整数 `x` 和 `y` 满足下述条件，则认为二者形成一个质数对:

- `1 <= x <= y <= n`
- `x + y == n`
- `x` 和 `y` 都是质数

请你以二维有序列表的形式返回符合题目要求的所有 `[xi, yi]` ，列表需要按 `xi` 的 __非递减顺序__ 排序。如果不存在符合要求的质数对，则返回一个空数组。

__注意:__ 质数是大于 `1` 的自然数，并且只有两个因子，即它本身和 `1` 。

__示例:__

示例 1：

```text
输入：n = 10
输出：[[3,7],[5,5]]
解释：在这个例子中，存在满足条件的两个质数对。 
这两个质数对分别是 [3,7] 和 [5,5]，按照题面描述中的方式排序后返回。
```

示例 2：

```text
输入：n = 2
输出：[]
解释：可以证明不存在和为 2 的质数对，所以返回一个空数组。
```

__提示：__

- `1 <= n <= 10 ^ 6`

__思路:__

```text
数学
先使用筛法选出 10 ^ 6 内的全部质数
如果 n 是奇数, 特判一下 n 是否比 4 大且 n - 2 是否为质数, 且只有这一种情况
否则将 n - x 不小于 x 的所有质数对加入集合
时间复杂度为 O(N / logN), 空间复杂度为 O(1), 质数对不大于 N / logN 个
```

__代码:__

__C++__:

```C++
constexpr int MX = 1e6 + 1;
vector<int> primes;
bool is_not_prime[MX + 1];

int init = []() 
{
    for (int i = 2; i < MX; i++) 
    {
        if (!is_not_prime[i]) 
        {
            primes.emplace_back(i);
            for (int j = i; j <= MX / i; j++) is_not_prime[i * j] = true;
        }
    }
    return 0;
}();

class Solution 
{
public:
    vector<vector<int>> findPrimePairs(int n) 
    {
        vector<vector<int>> result;
        if (n & 1)
        {
            if (n > 4 and !is_not_prime[n - 2]) result.emplace_back(vector<int>{ 2, n - 2 });
            return result;
        }
        for (const auto& x : primes)
        {
            if (n - x < x) break;
            if (!is_not_prime[n - x]) result.emplace_back(vector<int>{ x, n - x });
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    private final static int MX = 1_000_001;
    private final static List<Integer> primes = new ArrayList<>();
    private final static boolean[] isNotPrime = new boolean[MX + 1];

    static {
        for (int i = 2; i < MX; i++) {
            if (!isNotPrime[i]) {
                primes.add(i);
                for (int j = i; j <= MX / i; j++) isNotPrime[i * j] = true;
            }
        }
    }

    public List<List<Integer>> findPrimePairs(int n) {
        var result = new ArrayList<List<Integer>>();
        if ((n & 1) == 1) {
            if (n > 4 && !isNotPrime[n - 2]) result.add(List.of(2, n - 2));
            return result;
        }
        for (int x : primes) {
            if (n - x < x) break;
            if (!isNotPrime[n - x]) result.add(List.of(x, n - x));
        }
        return result;
    }
}
```

__Python__:

```Python
MX = 10 ** 6 + 1
primes = []
is_prime = [True] * MX
for i in range(2, MX):
    if is_prime[i]:
        primes.append(i)
        for j in range(i * i, MX, i):
            is_prime[j] = False

class Solution:
    def findPrimePairs(self, n: int) -> List[List[int]]:
        result = []
        if n & 1:
            if n > 4 and is_prime[n - 2]:
                result.append([2, n - 2])
            return result
        for x in primes:
            if (y := n - x) < x:
                break
            if is_prime[y]:
                result.append([x, y])
        return result
```
