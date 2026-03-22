# 2502 Design Memory Allocator 设计内存分配器

__Description:__

You are given an integer `n` representing the size of a __0-indexed__ memory array. All memory units are initially free.

You have a memory allocator with the following functionalities:

Note that:

- Multiple blocks can be allocated to the same `mID`.
- You should free all the memory units with `mID`, even if they were allocated in different blocks.

Implement the `Allocator` class:

- `Allocator(int n)` Initializes an `Allocator` object with a memory array of size `n`.
- `int allocate(int size, int mID)` Find the __leftmost__ block of `size` __consecutive__ free memory units and allocate it with the id `mID`. Return the block's first index. If such a block does not exist, return `-1`.
- `int freeMemory(int mID)` Free all memory units with the id `mID`. Return the number of memory units you have freed.

__Example:__

Example 1:

```text
Input
["Allocator", "allocate", "allocate", "allocate", "freeMemory", "allocate", "allocate", "allocate", "freeMemory", "allocate", "freeMemory"]
[[10], [1, 1], [1, 2], [1, 3], [2], [3, 4], [1, 1], [1, 1], [1], [10, 2], [7]]
Output
[null, 0, 1, 2, 1, 3, 1, 6, 3, -1, 0]

Explanation
```

```Java
Allocator loc = new Allocator(10); // Initialize a memory array of size 10. All memory units are initially free.
loc.allocate(1, 1); // The leftmost block's first index is 0. The memory array becomes [1,_,_,_,_,_,_,_,_,_]. We return 0.
loc.allocate(1, 2); // The leftmost block's first index is 1. The memory array becomes [1,2,_,_,_,_,_,_,_,_]. We return 1.
loc.allocate(1, 3); // The leftmost block's first index is 2. The memory array becomes [1,2,3,_,_,_,_,_,_,_]. We return 2.
loc.freeMemory(2); // Free all memory units with mID 2. The memory array becomes [1,_, 3,_,_,_,_,_,_,_]. We return 1 since there is only 1 unit with mID 2.
loc.allocate(3, 4); // The leftmost block's first index is 3. The memory array becomes [1,_,3,4,4,4,_,_,_,_]. We return 3.
loc.allocate(1, 1); // The leftmost block's first index is 1. The memory array becomes [1,1,3,4,4,4,_,_,_,_]. We return 1.
loc.allocate(1, 1); // The leftmost block's first index is 6. The memory array becomes [1,1,3,4,4,4,1,_,_,_]. We return 6.
loc.freeMemory(1); // Free all memory units with mID 1. The memory array becomes [_,_,3,4,4,4,_,_,_,_]. We return 3 since there are 3 units with mID 1.
loc.allocate(10, 2); // We can not find any free block with 10 consecutive free memory units, so we return -1.
loc.freeMemory(7); // Free all memory units with mID 7. The memory array remains the same since there is no memory unit with mID 7. We return 0.
```

__Constraints:__

- `1 <= n, size, mID <= 1000`
- At most `1000` calls will be made to `allocate` and `freeMemory`.

__题目描述:__

给你一个整数 `n` ，表示下标从 __0__ 开始的内存数组的大小。所有内存单元开始都是空闲的。

请你设计一个具备以下功能的内存分配器：

注意：

- 多个块可以被分配到同一个 `mID` 。
- 你必须释放 `mID` 对应的所有内存单元，即便这些内存单元被分配在不同的块中。

实现 `Allocator` 类:

- `Allocator(int n)` 使用一个大小为 `n` 的内存数组初始化 `Allocator` 对象。
- `int allocate(int size, int mID)` 找出大小为 `size` 个连续空闲内存单元且位于 __最左侧__ 的块，分配并赋 id `mID` 。返回块的第一个下标。如果不存在这样的块，返回 `-1` 。
- `int freeMemory(int mID)` 释放 id `mID` 对应的所有内存单元。返回释放的内存单元数目。

__示例:__

示例：

```text
输入
["Allocator", "allocate", "allocate", "allocate", "freeMemory", "allocate", "allocate", "allocate", "freeMemory", "allocate", "freeMemory"]
[[10], [1, 1], [1, 2], [1, 3], [2], [3, 4], [1, 1], [1, 1], [1], [10, 2], [7]]
输出
[null, 0, 1, 2, 1, 3, 1, 6, 3, -1, 0]

解释
```

```Java
Allocator loc = new Allocator(10); // 初始化一个大小为 10 的内存数组，所有内存单元都是空闲的。
loc.allocate(1, 1); // 最左侧的块的第一个下标是 0 。内存数组变为 [1, , , , , , , , , ]。返回 0 。
loc.allocate(1, 2); // 最左侧的块的第一个下标是 1 。内存数组变为 [1,2, , , , , , , , ]。返回 1 。
loc.allocate(1, 3); // 最左侧的块的第一个下标是 2 。内存数组变为 [1,2,3, , , , , , , ]。返回 2 。
loc.freeMemory(2); // 释放 mID 为 2 的所有内存单元。内存数组变为 [1, ,3, , , , , , , ] 。返回 1 ，因为只有 1 个 mID 为 2 的内存单元。
loc.allocate(3, 4); // 最左侧的块的第一个下标是 3 。内存数组变为 [1, ,3,4,4,4, , , , ]。返回 3 。
loc.allocate(1, 1); // 最左侧的块的第一个下标是 1 。内存数组变为 [1,1,3,4,4,4, , , , ]。返回 1 。
loc.allocate(1, 1); // 最左侧的块的第一个下标是 6 。内存数组变为 [1,1,3,4,4,4,1, , , ]。返回 6 。
loc.freeMemory(1); // 释放 mID 为 1 的所有内存单元。内存数组变为 [ , ,3,4,4,4, , , , ] 。返回 3 ，因为有 3 个 mID 为 1 的内存单元。
loc.allocate(10, 2); // 无法找出长度为 10 个连续空闲内存单元的空闲块，所有返回 -1 。
loc.freeMemory(7); // 释放 mID 为 7 的所有内存单元。内存数组保持原状，因为不存在 mID 为 7 的内存单元。返回 0 。
```

__提示：__

- `1 <= n, size, mID <= 1000`
- 最多调用 `allocate` 和 `free` 方法 `1000` 次

__思路:__

```text
1. 暴力法
遍历数组，找到第一个连续的空闲内存单元
如果找到，则将其分配给 mID
否则返回 -1
释放内存时，遍历数组，将所有 mID 对应的内存单元释放
时间复杂度为 O(N ^ 2), 空间复杂度为 O(1)
2. 线段树
使用线段树来维护内存分配
每个节点维护一个区间的状态
区间的状态包括：
- 前缀和 pre0
- 后缀和 suf0
- 最大值 max0
- 懒标记
- 懒标记表示当前区间的状态是否需要更新
- 如果懒标记不为 -1，则表示当前区间的状态需要更新
max0 = max(左子树的 max0, 右子树的 max0, 左子树的 suf0 + 右子树的 pre0)
```

__代码:__

__C++__:

```C++
class SegTree 
{
    struct Node 
    {
        int pre0, suf0, max0, todo;
    };

    vector<Node> t;

    void do_(int i, int l, int r, int v) 
    {
        auto& o = t[i];
        int size = v <= 0 ? r - l + 1 : 0;
        o.pre0 = o.suf0 = o.max0 = size;
        o.todo = v;
    }

    void spread(int o, int l, int r) 
    {
        int& v = t[o].todo;
        if (v != -1) 
        {
            int m = (l + r) >> 1;
            do_(o << 1, l, m, v);
            do_((o << 1) + 1, m + 1, r, v);
            v = -1;
        }
    }

    void build(int o, int l, int r) 
    {
        do_(o, l, r, -1);
        if (l == r) return;
        int m = (l + r) >> 1;
        build(o << 1, l, m);
        build((o << 1) + 1, m + 1, r);
    }

public:
    SegTree(int n) 
    {
        t.resize(2 << bit_width((unsigned) n - 1));
        build(1, 0, n - 1);
    }

    void update(int o, int l, int r, int ql, int qr, int v) 
    {
        if (ql <= l and r <= qr) 
        {
            do_(o, l, r, v);
            return;
        }
        spread(o, l, r);
        int m = (l + r) >> 1;
        if (ql <= m) update(o << 1, l, m, ql, qr, v);
        if (m < qr) update((o << 1) + 1, m + 1, r, ql, qr, v);
        Node& lo = t[o << 1];
        Node& ro = t[(o << 1) + 1];
        t[o].pre0 = lo.pre0;
        if (lo.pre0 == m - l + 1) t[o].pre0 += ro.pre0;
        t[o].suf0 = ro.suf0;
        if (ro.suf0 == r - m) t[o].suf0 += lo.suf0;
        t[o].max0 = max({lo.max0, ro.max0, lo.suf0 + ro.pre0});
    }

    int find_first(int o, int l, int r, int size) 
    {
        if (t[o].max0 < size) return -1;
        if (l == r) return l;
        spread(o, l, r);
        int m = (l + r) >> 1;
        int idx = find_first(o << 1, l, m, size);
        if (idx < 0) 
        {
            if (t[o << 1].suf0 + t[(o << 1) + 1].pre0 >= size) return m - t[o * 2].suf0 + 1;
            idx = find_first(o * 2 + 1, m + 1, r, size);
        }
        return idx;
    }
};

class Allocator 
{
public:
    Allocator(int n) : n(n), tree(n) {}

    int allocate(int size, int mID) 
    {
        int i = tree.find_first(1, 0, n - 1, size);
        if (i < 0) return -1;
        blocks[mID].emplace_back(i, i + size - 1);
        tree.update(1, 0, n - 1, i, i + size - 1, 1);
        return i;
    }

    int freeMemory(int mID) 
    {
        int result = 0;
        for (auto& [l, r] : blocks[mID]) 
        {
            result += r - l + 1;
            tree.update(1, 0, n - 1, l, r, 0);
        }
        blocks.erase(mID);
        return result;
    }
private:
    int n;
    SegTree tree;
    unordered_map<int, vector<pair<int, int>>> blocks;
};

/**
 * Your Allocator object will be instantiated and called as such:
 * Allocator* obj = new Allocator(n);
 * int param_1 = obj->allocate(size,mID);
 * int param_2 = obj->freeMemory(mID);
 */
```

__Java__:

```Java
class Allocator {
    private final int[] data;

    public Allocator(int n) {
        data = new int[n];
    }
    
    public int allocate(int size, int mID) {
        for (int i = 0, n = data.length, cur = 0; i < n; i++) {
            cur = data[i] > 0 ? 0 : cur + 1;
            if (cur == size) {
                Arrays.fill(data, i - size + 1, i + 1, mID);
                return i - size + 1;
            }
        }
        return -1;
    }
    
    public int freeMemory(int mID) {
        int result = 0;
        for (int i = 0, n = data.length; i < n; i++) {
            if (data[i] == mID) {
                ++result;
                data[i] = 0;
            }
        }
        return result;
    }
}

/**
 * Your Allocator object will be instantiated and called as such:
 * Allocator obj = new Allocator(n);
 * int param_1 = obj.allocate(size,mID);
 * int param_2 = obj.freeMemory(mID);
 */
```

__Python__:

```Python
class Allocator:

    def __init__(self, n: int):
        self.data = [0] * n
        

    def allocate(self, size: int, mID: int) -> int:
        cur = 0
        for i, x in enumerate(self.data):
            if (cur := 0 if x else cur + 1) == size:
                self.data[i - size + 1:i + 1] = [mID] * size
                return i - size + 1
        return -1
        

    def freeMemory(self, mID: int) -> int:
        result = 0
        for i, x in enumerate(self.data):
            if x == mID:
                self.data[i] = 0
                result += 1
        return result


# Your Allocator object will be instantiated and called as such:
# obj = Allocator(n)
# param_1 = obj.allocate(size,mID)
# param_2 = obj.freeMemory(mID)
```
