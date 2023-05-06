# 1622 Fancy Sequence 奇妙序列

__Description:__

Write an API that generates fancy sequences using the `append`, `addAll`, and `multAll` operations.

Implement the `Fancy` class:

- `Fancy()` Initializes the object with an empty sequence.
- `void append(val)` Appends an integer `val` to the end of the sequence.
- `void addAll(inc)` Increments all existing values in the sequence by an integer `inc`.
- `void multAll(m)` Multiplies all existing values in the sequence by an integer `m`.
- `int getIndex(idx)` Gets the current value at index `idx` (0-indexed) of the sequence __modulo__ `10 ^ 9 + 7`. If the index is greater or equal than the length of the sequence, return `-1`.

__Example:__

Example 1:

```text
Input
["Fancy", "append", "addAll", "append", "multAll", "getIndex", "addAll", "append", "multAll", "getIndex", "getIndex", "getIndex"]
[[], [2], [3], [7], [2], [0], [3], [10], [2], [0], [1], [2]]
Output
[null, null, null, null, null, 10, null, null, null, 26, 34, 20]
```

Explanation

```Java
Fancy fancy = new Fancy();
fancy.append(2);   // fancy sequence: [2]
fancy.addAll(3);   // fancy sequence: [2+3] -> [5]
fancy.append(7);   // fancy sequence: [5, 7]
fancy.multAll(2);  // fancy sequence: [5*2, 7*2] -> [10, 14]
fancy.getIndex(0); // return 10
fancy.addAll(3);   // fancy sequence: [10+3, 14+3] -> [13, 17]
fancy.append(10);  // fancy sequence: [13, 17, 10]
fancy.multAll(2);  // fancy sequence: [13*2, 17*2, 10*2] -> [26, 34, 20]
fancy.getIndex(0); // return 26
fancy.getIndex(1); // return 34
fancy.getIndex(2); // return 20
```

__Constraints:__

- `1 <= val, inc, m <= 100`
- `0 <= idx <= 10 ^ 5`
- At most `10 ^ 5` calls total will be made to `append`, `addAll`, `multAll`, and `getIndex`.

__题目描述:__

请你实现三个 API `append`， `addAll` 和 `multAll` 来实现奇妙序列。

请实现 `Fancy` 类 :

- `Fancy()` 初始化一个空序列对象。
- `void append(val)` 将整数 `val` 添加在序列末尾。
- `void addAll(inc)` 将所有序列中的现有数值都增加 `inc` 。
- `void multAll(m)` 将序列中的所有现有数值都乘以整数 `m` 。
- `int getIndex(idx)` 得到下标为 `idx` 处的数值（下标从 0 开始），并将结果对 `10 ^ 9 + 7` 取余。如果下标大于等于序列的长度，请返回 `-1` 。

__示例:__

示例：

```text
输入：
["Fancy", "append", "addAll", "append", "multAll", "getIndex", "addAll", "append", "multAll", "getIndex", "getIndex", "getIndex"]
[[], [2], [3], [7], [2], [0], [3], [10], [2], [0], [1], [2]]
输出：
[null, null, null, null, null, 10, null, null, null, 26, 34, 20]
```

解释：

```Java
Fancy fancy = new Fancy();
fancy.append(2);   // 奇妙序列：[2]
fancy.addAll(3);   // 奇妙序列：[2+3] -> [5]
fancy.append(7);   // 奇妙序列：[5, 7]
fancy.multAll(2);  // 奇妙序列：[5*2, 7*2] -> [10, 14]
fancy.getIndex(0); // 返回 10
fancy.addAll(3);   // 奇妙序列：[10+3, 14+3] -> [13, 17]
fancy.append(10);  // 奇妙序列：[13, 17, 10]
fancy.multAll(2);  // 奇妙序列：[13*2, 17*2, 10*2] -> [26, 34, 20]
fancy.getIndex(0); // 返回 26
fancy.getIndex(1); // 返回 34
fancy.getIndex(2); // 返回 20
```

__提示：__

- `1 <= val, inc, m <= 100`
- `0 <= idx <= 10 ^ 5`
- 总共最多会有 `10 ^ 5` 次对 `append`， `addAll`， `multAll` 和 `getIndex` 的调用。

__思路:__

```text
乘法逆元
要得到 a * a ^ -1 = 1(mod m)
当 m 为质数时, 根据裴蜀定理和费马小定理只需求得 a ^ (m - 2) 即可
可以使用快速幂运算加快计算速度
增加值和乘可以用两个操作数 a, b 记录
即 ax + b
记录所有的原始值及操作
当前操作为 a[idx], b[idx], 对应可以将操作数变成 a[idx] * v[idx] + b[idx]
由最终 a, b 可以得到需要的操作数为 a0 * a[idx] = a(mod m), b0 * a[idx] + b[idx] = b(mod m)
可以解出 a0 和 b0, 那么对应的数即为 (a0 * v[idx] + b0) mod m
快速幂时间复杂度为 O(logM), 其余函数时间复杂度为 O(1), 空间复杂度为 O(N), 其中 m 为 1e9 + 7
```

__代码:__

__C++__:

```C++
class Fancy 
{
private:
    static constexpr int mod = 1e9 + 7;
    vector<int> v;
    int a, b;
    
    int quickmult(int x, int y) 
    {
        int result = 1;
        while (y) 
        {
            if (y & 1) result = (long long)result * x % mod;
            x = (long long)x * x % mod;
            y >>= 1;
        }
        return result;
    }
    
    int inv(int x) 
    {
        return quickmult(x, mod - 2);
    }
public:
    Fancy(): a(1), b(0) {}
    

    
    void append(int val) 
    {
        v.emplace_back((long long)((val - b + mod) % mod) * inv(a) % mod);
    }
    
    void addAll(int inc) 
    {
        b = (b + inc) % mod;
    }
    
    void multAll(int m) 
    {
        a = (long long)a * m % mod;
        b = (long long)b * m % mod;
    }
    
    int getIndex(int idx) 
    {
        return idx >= v.size() ? -1 : ((long long)a * v[idx] % mod + b) % mod;
    }
};
```

__Java__:

```Java
public class Fancy {
    private static final int MOD = 1_000_000_007;
    private List<Integer> sequence;
    private long add;
    private long mult;

    public Fancy() {
        sequence = new ArrayList<>();
        add = 0;
        mult = 1;
    }

    public void append(int val) {
        sequence.add((int) (((val - add + MOD) % MOD) * modInverse(mult) % MOD));
    }

    public void addAll(int inc) {
        add = (add + inc) % MOD;
    }

    public void multAll(int m) {
        mult = (mult * m) % MOD;
        add = (add * m) % MOD;
    }

    public int getIndex(int idx) {
        return idx >= sequence.size() ? -1 : (int) ((sequence.get(idx) * mult % MOD + add) % MOD);
    }

    private long modInverse(long a) {
        long m = MOD;
        long y = 0, x = 1;
        while (a > 1) {
            long q = a / m;
            long t = m;
            m = a % m;
            a = t;
            t = y;
            y = x - q * y;
            x = t;
        }
        if (x < 0) {
            x += MOD;
        }
        return x;
    }
}

/**
 * Your Fancy object will be instantiated and called as such:
 * Fancy obj = new Fancy();
 * obj.append(val);
 * obj.addAll(inc);
 * obj.multAll(m);
 * int param_4 = obj.getIndex(idx);
 */
```

__Python__:

```Python
class Fancy:
    
    def __init__(self):
        self.MOD = 10 ** 9 + 7
        self.data = []
        self.a = [1]
        self.b = [0]
        
        
    def quickmult(self, a: int, x: int) -> int:
        result = 1
        while x:
            if x & 1:
                result = result * a % self.MOD
            a = a * a % self.MOD
            x >>= 1
        return result

        
    def inv(self, a: int) -> int:
        return self.quickmult(a, self.MOD - 2)

    
    def append(self, val: int) -> None:
        self.data.append(val)
        self.a.append(self.a[-1])
        self.b.append(self.b[-1])


    def addAll(self, inc: int) -> None:
        self.b[-1] = (self.b[-1] + inc) % self.MOD


    def multAll(self, m: int) -> None:
        self.a[-1], self.b[-1] = self.a[-1] * m % self.MOD, self.b[-1] * m % self.MOD


    def getIndex(self, idx: int) -> int:
        return -1 if idx >= len(self.data) else ((a := self.inv(self.a[idx]) * self.a[-1]) * self.data[idx] + (self.b[-1] - self.b[idx] * a)) % self.MOD



# Your Fancy object will be instantiated and called as such:
# obj = Fancy()
# obj.append(val)
# obj.addAll(inc)
# obj.multAll(m)
# param_4 = obj.getIndex(idx)
```
