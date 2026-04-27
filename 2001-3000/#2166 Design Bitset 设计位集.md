# 2166 Design Bitset 设计位集

__Description:__

A Bitset is a data structure that compactly stores bits.

Implement the `Bitset` class:

- `Bitset(int size)` Initializes the Bitset with `size` bits, all of which are `0`.
- `void fix(int idx)` Updates the value of the bit at the index `idx` to `1`. If the value was already `1`, no change occurs.
- `void unfix(int idx)` Updates the value of the bit at the index `idx` to `0`. If the value was already `0`, no change occurs.
- `void flip()` Flips the values of each bit in the Bitset. In other words, all bits with value `0` will now have value `1` and vice versa.
- `boolean all()` Checks if the value of __each__ bit in the Bitset is `1`. Returns `true` if it satisfies the condition, `false` otherwise.
- `boolean one()` Checks if there is __at least one__ bit in the Bitset with value `1`. Returns `true` if it satisfies the condition, `false` otherwise.
- `int count()` Returns the __total number__ of bits in the Bitset which have value `1`.
- `String toString()` Returns the current composition of the Bitset. Note that in the resultant string, the character at the `i ^ th` index should coincide with the value at the `i ^ th` bit of the Bitset.

__Example:__

Example 1:

```text
Input
["Bitset", "fix", "fix", "flip", "all", "unfix", "flip", "one", "unfix", "count", "toString"]
[[5], [3], [1], [], [], [0], [], [], [0], [], []]
Output
[null, null, null, null, false, null, null, true, null, 2, "01010"]

Explanation
```

```Java
Bitset bs = new Bitset(5); // bitset = "00000".
bs.fix(3);     // the value at idx = 3 is updated to 1, so bitset = "00010".
bs.fix(1);     // the value at idx = 1 is updated to 1, so bitset = "01010". 
bs.flip();     // the value of each bit is flipped, so bitset = "10101". 
bs.all();      // return False, as not all values of the bitset are 1.
bs.unfix(0);   // the value at idx = 0 is updated to 0, so bitset = "00101".
bs.flip();     // the value of each bit is flipped, so bitset = "11010". 
bs.one();      // return True, as there is at least 1 index with value 1.
bs.unfix(0);   // the value at idx = 0 is updated to 0, so bitset = "01010".
bs.count();    // return 2, as there are 2 bits with value 1.
bs.toString(); // return "01010", which is the composition of bitset.
```

__Constraints:__

- `1 <= size <= 10 ^ 5`
- `0 <= idx <= size - 1`
- At most `10 ^ 5` calls will be made __in total__ to `fix`, `unfix`, `flip`, `all`, `one`, `count`, and `toString`.
- At least one call will be made to `all`, `one`, `count`, or `toString`.
- At most `5` calls will be made to `toString`.

__题目描述:__

位集 Bitset 是一种能以紧凑形式存储位的数据结构。

请你实现 `Bitset` 类。

- `Bitset(int size)` 用 `size` 个位初始化 Bitset ，所有位都是 `0` 。
- `void fix(int idx)` 将下标为 `idx` 的位上的值更新为 `1` 。如果值已经是 `1` ，则不会发生任何改变。
- `void unfix(int idx)` 将下标为 `idx` 的位上的值更新为 `0` 。如果值已经是 `0` ，则不会发生任何改变。
- `void flip()` 翻转 Bitset 中每一位上的值。换句话说，所有值为 `0` 的位将会变成 `1` ，反之亦然。
- `boolean all()` 检查 Bitset 中 __每一位__ 的值是否都是 `1` 。如果满足此条件，返回 `true` ；否则，返回 `false` 。
- `boolean one()` 检查 Bitset 中 是否 __至少一位__ 的值是 `1` 。如果满足此条件，返回 `true` ；否则，返回 `false` 。
- `int count()` 返回 Bitset 中值为 1 的位的 __总数__ 。
- `String toString()` 返回 Bitset 的当前组成情况。注意，在结果字符串中，第 `i` 个下标处的字符应该与 Bitset 中的第 `i` 位一致。

__示例:__

示例：

```text
输入
["Bitset", "fix", "fix", "flip", "all", "unfix", "flip", "one", "unfix", "count", "toString"]
[[5], [3], [1], [], [], [0], [], [], [0], [], []]
输出
[null, null, null, null, false, null, null, true, null, 2, "01010"]

解释
```

```Java
Bitset bs = new Bitset(5); // bitset = "00000".
bs.fix(3);     // 将 idx = 3 处的值更新为 1 ，此时 bitset = "00010" 。
bs.fix(1);     // 将 idx = 1 处的值更新为 1 ，此时 bitset = "01010" 。
bs.flip();     // 翻转每一位上的值，此时 bitset = "10101" 。
bs.all();      // 返回 False ，bitset 中的值不全为 1 。
bs.unfix(0);   // 将 idx = 0 处的值更新为 0 ，此时 bitset = "00101" 。
bs.flip();     // 翻转每一位上的值，此时 bitset = "11010" 。
bs.one();      // 返回 True ，至少存在一位的值为 1 。
bs.unfix(0);   // 将 idx = 0 处的值更新为 0 ，此时 bitset = "01010" 。
bs.count();    // 返回 2 ，当前有 2 位的值为 1 。
bs.toString(); // 返回 "01010" ，即 bitset 的当前组成情况。
```

__提示：__

- `1 <= size <= 10 ^ 5`
- `0 <= idx <= size - 1`
- 至多调用 `fix`、 `unfix`、 `flip`、 `all`、 `one`、 `count` 和 `toString` 方法 __总共__ `10 ^ 5` 次
- 至少调用 `all`、 `one`、 `count` 或 `toString` 方法一次
- 至多调用 `toString` 方法 `5` 次

__思路:__

```text
模拟 ➕ 懒标记
用一个数组记录所有的位, 长度为 size
用一个变量 total 记录当前位为 1 的个数
用一个变量 flag 记录当前是否翻转
fix(idx) 将 idx 位置的值更新为 1, 如果原来为 1, 则不变, 由于可能翻转, 所以需要与 flag 异或, 如果异或后为 1, 则 total 加 1, 并且翻转
unfix(idx) 同理
flip() 翻转所有位, total 为 size - total, flag 取反
all() 判断 total 是否等于 size
one() 判断 total 是否大于 0
count() 返回 total
toString() 用 flag 判断是否翻转, 返回字符串
初始化和 toString() 时间复杂度为 O(N), 其他函数时间复杂度为 O(1), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Bitset 
{
public:
    Bitset(int size) 
    {
        this -> size = size;
        s.resize(size);
    }
    
    void fix(int idx) 
    {
        if (!(s[idx] ^ flag))
        {
            s[idx] ^= 1;
            ++total;
        }
    }
    
    void unfix(int idx) 
    {
        if (s[idx] ^ flag)
        {
            s[idx] ^= 1;
            --total;
        }
    }
    
    void flip() 
    {
        flag ^= 1;
        total = size - total;
    }
    
    bool all() 
    {
        return total == size;
    }
    
    bool one() 
    {
        return !!total;
    }
    
    int count() 
    {
        return total;
    }
    
    string toString() 
    {
        string result;
        for (int b : s) result += ('0' + (b ^ flag));
        return result;
    }
private:
    vector<int> s;
    int size = 0;
    int total = 0;
    int flag = 0;
};

/**
 * Your Bitset object will be instantiated and called as such:
 * Bitset* obj = new Bitset(size);
 * obj->fix(idx);
 * obj->unfix(idx);
 * obj->flip();
 * bool param_4 = obj->all();
 * bool param_5 = obj->one();
 * int param_6 = obj->count();
 * string param_7 = obj->toString();
 */
```

__Java__:

```Java
class Bitset {
    private int total, size;
    private boolean flag;
    private byte[] s;

    public Bitset(int size) {
        this.size = size;
        s = new byte[size];
    }
    
    public void fix(int idx) {
        if ((s[idx] == 1 && flag) || (s[idx] == 0 && !flag)) {
            s[idx] ^= 1;
            ++total;
        }
    }
    
    public void unfix(int idx) {
        if ((s[idx] == 1 && !flag) || (s[idx] == 0 && flag)) {
            s[idx] ^= 1;
            --total;
        }
    }
    
    public void flip() {
        flag = !flag;
        total = size - total;
    }
    
    public boolean all() {
        return total == size;
    }
    
    public boolean one() {
        return total > 0;
    }
    
    public int count() {
        return total;
    }
    
    public String toString() {
        StringBuilder sb = new StringBuilder();
        for (byte b : s) sb.append(b ^ (flag ? 1 : 0));
        return sb.toString();
    }
}

/**
 * Your Bitset object will be instantiated and called as such:
 * Bitset obj = new Bitset(size);
 * obj.fix(idx);
 * obj.unfix(idx);
 * obj.flip();
 * boolean param_4 = obj.all();
 * boolean param_5 = obj.one();
 * int param_6 = obj.count();
 * String param_7 = obj.toString();
 */
```

__Python__:

```Python
class Bitset:

    def __init__(self, size: int):
        self.s = [0] * size
        self.size = size
        self.total = 0
        self.flag = 0


    def fix(self, idx: int) -> None:
        if not self.s[idx] ^ self.flag:
            self.s[idx] ^= 1
            self.total += 1


    def unfix(self, idx: int) -> None:
        if self.s[idx] ^ self.flag:
            self.s[idx] ^= 1
            self.total -= 1


    def flip(self) -> None:
        self.flag ^= 1
        self.total = self.size - self.total


    def all(self) -> bool:
        return self.total == self.size


    def one(self) -> bool:
        return not not self.total


    def count(self) -> int:
        return self.total


    def toString(self) -> str:
        return ''.join(str(b ^ self.flag) for b in self.s)


# Your Bitset object will be instantiated and called as such:
# obj = Bitset(size)
# obj.fix(idx)
# obj.unfix(idx)
# obj.flip()
# param_4 = obj.all()
# param_5 = obj.one()
# param_6 = obj.count()
# param_7 = obj.toString()
```
