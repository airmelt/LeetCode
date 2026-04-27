# 1993 Operations on Tree 树上的操作

__Description:__

You are given a tree with `n` nodes numbered from `0` to `n - 1` in the form of a parent array `parent` where `parent[i]` is the parent of the `i ^ th` node. The root of the tree is node `0`, so `parent[0] = -1` since it has no parent. You want to design a data structure that allows users to lock, unlock, and upgrade nodes in the tree.

The data structure should support the following functions:

- __Lock:__ __Locks__ the given node for the given user and prevents other users from locking the same node. You may only lock a node using this function if the node is unlocked.
- __Unlock: Unlocks__ the given node for the given user. You may only unlock a node using this function if it is currently locked by the same user.
- _Upgrade_ __: Locks__ the given node for the given user and __unlocks__ all of its descendants __regardless__ of who locked it. You may only upgrade a node if __all__ 3 conditions are true:

- The node is unlocked,
- It has at least one locked descendant (by __any__ user), and
- It does not have any locked ancestors.

Implement the `LockingTree` class:

- `LockingTree(int[] parent)` initializes the data structure with the parent array.
- `lock(int num, int user)` returns `true` if it is possible for the user with id `user` to lock the node `num`, or `false` otherwise. If it is possible, the node `num` will become __locked__ by the user with id `user`.
- `unlock(int num, int user)` returns `true` if it is possible for the user with id `user` to unlock the node `num`, or `false` otherwise. If it is possible, the node `num` will become __unlocked__.
- `upgrade(int num, int user)` returns `true` if it is possible for the user with id `user` to upgrade the node `num`, or `false` otherwise. If it is possible, the node `num` will be __upgraded__.

__Example:__

Example 1:

![1993-1](https://assets.leetcode.com/uploads/2021/07/29/untitled.png)

```text
Input
["LockingTree", "lock", "unlock", "unlock", "lock", "upgrade", "lock"]
[[[-1, 0, 0, 1, 1, 2, 2]], [2, 2], [2, 3], [2, 2], [4, 5], [0, 1], [0, 1]]
Output
[null, true, false, true, true, true, false]

Explanation
```

```Java
LockingTree lockingTree = new LockingTree([-1, 0, 0, 1, 1, 2, 2]);
lockingTree.lock(2, 2);    // return true because node 2 is unlocked.
                           // Node 2 will now be locked by user 2.
lockingTree.unlock(2, 3);  // return false because user 3 cannot unlock a node locked by user 2.
lockingTree.unlock(2, 2);  // return true because node 2 was previously locked by user 2.
                           // Node 2 will now be unlocked.
lockingTree.lock(4, 5);    // return true because node 4 is unlocked.
                           // Node 4 will now be locked by user 5.
lockingTree.upgrade(0, 1); // return true because node 0 is unlocked and has at least one locked descendant (node 4).
                           // Node 0 will now be locked by user 1 and node 4 will now be unlocked.
lockingTree.lock(0, 1);    // return false because node 0 is already locked.
```

__Constraints:__

- `n == parent.length`
- `2 <= n <= 2000`
- `0 <= parent[i] <= n - 1` for `i != 0`
- `parent[0] == -1`
- `0 <= num <= n - 1`
- `1 <= user <= 10 ^ 4`
- `parent` represents a valid tree.
- At most `2000` calls __in total__ will be made to `lock`, `unlock`, and `upgrade`.

__题目描述:__

给你一棵 `n` 个节点的树，编号从 `0` 到 `n - 1` ，以父节点数组 `parent` 的形式给出，其中 `parent[i]` 是第 `i` 个节点的父节点。树的根节点为 `0` 号节点，所以 `parent[0] = -1` ，因为它没有父节点。你想要设计一个数据结构实现树里面对节点的加锁，解锁和升级操作。

数据结构需要支持如下函数：

- __Lock:__指定用户给指定节点 __上锁__ ，上锁后其他用户将无法给同一节点上锁。只有当节点处于未上锁的状态下，才能进行上锁操作。
- __Unlock:__指定用户给指定节点 __解锁__ ，只有当指定节点当前正被指定用户锁住时，才能执行该解锁操作。
- _Upgrade:_ 指定用户给指定节点 __上锁__ ，并且将该节点的所有子孙节点 __解锁__ 。只有如下 3 个条件 __全部__ 满足时才能执行升级操作:

- 指定节点当前状态为未上锁。
- 指定节点至少有一个上锁状态的子孙节点（可以是 __任意__ 用户上锁的）。
- 指定节点没有任何上锁的祖先节点。

请你实现 `LockingTree` 类:

- `LockingTree(int[] parent)` 用父节点数组初始化数据结构。
- `lock(int num, int user)` 如果 id 为 `user` 的用户可以给节点 `num` 上锁，那么返回 `true` ，否则返回 `false` 。如果可以执行此操作，节点 `num` 会被 id 为 `user` 的用户 __上锁__ 。
- `unlock(int num, int user)` 如果 id 为 `user` 的用户可以给节点 `num` 解锁，那么返回 `true` ，否则返回 `false` 。如果可以执行此操作，节点 `num` 变为 __未上锁__ 状态。
- `upgrade(int num, int user)` 如果 id 为 `user` 的用户可以给节点 `num` 升级，那么返回 `true` ，否则返回 `false` 。如果可以执行此操作，节点 `num` 会被 __升级__ 。

__示例:__

示例 1：

![1993-2](https://assets.leetcode.com/uploads/2021/07/29/untitled.png)

```text
输入：
["LockingTree", "lock", "unlock", "unlock", "lock", "upgrade", "lock"]
[[[-1, 0, 0, 1, 1, 2, 2]], [2, 2], [2, 3], [2, 2], [4, 5], [0, 1], [0, 1]]
输出：
[null, true, false, true, true, true, false]

解释：
```

```Java
LockingTree lockingTree = new LockingTree([-1, 0, 0, 1, 1, 2, 2]);
lockingTree.lock(2, 2);    // 返回 true ，因为节点 2 未上锁。
                           // 节点 2 被用户 2 上锁。
lockingTree.unlock(2, 3);  // 返回 false ，因为用户 3 无法解锁被用户 2 上锁的节点。
lockingTree.unlock(2, 2);  // 返回 true ，因为节点 2 之前被用户 2 上锁。
                           // 节点 2 现在变为未上锁状态。
lockingTree.lock(4, 5);    // 返回 true ，因为节点 4 未上锁。
                           // 节点 4 被用户 5 上锁。
lockingTree.upgrade(0, 1); // 返回 true ，因为节点 0 未上锁且至少有一个被上锁的子孙节点（节点 4）。
                           // 节点 0 被用户 1 上锁，节点 4 变为未上锁。
lockingTree.lock(0, 1);    // 返回 false ，因为节点 0 已经被上锁了。
```

__提示：__

- `n == parent.length`
- `2 <= n <= 2000`
- 对于 `i != 0` ，满足 `0 <= parent[i] <= n - 1`
- `parent[0] == -1`
- `0 <= num <= n - 1`
- `1 <= user <= 10 ^ 4`
- `parent` 表示一棵合法的树。
- `lock` ， `unlock` 和 `upgrade` 的调用 __总共__ 不超过 `2000` 次。

__思路:__

```text
DFS
使用 parent 数组构建树
记录每个节点的用户状态 users
记录每个节点的子节点 children
lock, unlock, 直接检查节点的用户状态
upgrade, 检查节点的用户状态, 祖先节点的用户状态, 子节点的用户状态
分成三个函数进行处理
初始化时间复杂度为 O(N), 空间复杂度为 O(N)
lock, unlock 时间复杂度为 O(1), 空间复杂度为 O(1)
upgrade 时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class LockingTree 
{
public:
    LockingTree(vector<int>& parent) 
    {
        int n = parent.size();
        this -> parent = parent;
        this -> users = vector<int>(n, -1);
        this -> children = vector<vector<int>>(n);
        for (int i = 0; i < n; i++) if (parent[i] != -1) children[parent[i]].emplace_back(i);
    }
    
    bool lock(int num, int user) 
    {
        if (!~users[num]) 
        {
            users[num] = user;
            return true;
        }
        return false;
    }
    
    bool unlock(int num, int user) 
    {
        if (users[num] == user) 
        {
            users[num] = -1;
            return true;
        }
        return false;
    }
    
    bool upgrade(int num, int user) 
    {
        bool result = !~users[num] and !ancestor(num) and check(num);
        if (result) users[num] = user;
        return result;
    }
private:
    vector<int> parent;
    vector<int> users;
    vector<vector<int>> children;

    bool ancestor(int num)
    {
        num = parent[num];
        while (~num) 
        {
            if (~users[num]) return true;
            num = parent[num];
        }
        return false;
    }

    bool check(int num)
    {
        bool result = users[num] != -1;
        users[num] = -1;
        for (const auto& child : children[num]) result |= check(child);
        return result;
    }
};

/**
 * Your LockingTree object will be instantiated and called as such:
 * LockingTree* obj = new LockingTree(parent);
 * bool param_1 = obj->lock(num,user);
 * bool param_2 = obj->unlock(num,user);
 * bool param_3 = obj->upgrade(num,user);
 */
```

__Java__:

```Java
class LockingTree {
    private int[] parent;
    private int[] users;
    private List<Integer>[] children;

    public LockingTree(int[] parent) {
        int n = parent.length;
        this.parent = parent;
        this.users = new int[n];
        Arrays.fill(users, -1);
        this.children = new List[n];
        for (int i = 0; i < n; i++) children[i] = new ArrayList<>();
        for (int i = 0; i < n; i++) if (parent[i] != -1) children[parent[i]].add(i);
    }
    
    public boolean lock(int num, int user) {
        if (users[num] == -1) {
            users[num] = user;
            return true;
        }
        return false;
    }
    
    public boolean unlock(int num, int user) {
        if (users[num] == user) {
            users[num] = -1;
            return true;
        }
        return false;
    }
    
    public boolean upgrade(int num, int user) {
        boolean result = users[num] == -1 && !ancestor(num) && check(num);
        if (result) users[num] = user;
        return result;
    }

    boolean ancestor(int num) {
        num = parent[num];
        while (num != -1) {
            if (users[num] != -1) return true;
            num = parent[num];
        }
        return false;
    }

    boolean check(int num) {
        boolean result = users[num] != -1;
        users[num] = -1;
        for (int child : children[num]) result |= check(child);
        return result;
    }
}

/**
 * Your LockingTree object will be instantiated and called as such:
 * LockingTree obj = new LockingTree(parent);
 * boolean param_1 = obj.lock(num,user);
 * boolean param_2 = obj.unlock(num,user);
 * boolean param_3 = obj.upgrade(num,user);
 */
```

__Python__:

```Python
class LockingTree:

    def __init__(self, parent: List[int]):
        n = len(parent)
        self.parent = parent
        self.users = [-1] * n
        self.children = [[] for _ in range(n)]
        for i, p in enumerate(parent):
            if p != -1:
                self.children[p].append(i)


    def lock(self, num: int, user: int) -> bool:
        if not ~self.users[num]:
            self.users[num] = user
            return True
        return False


    def unlock(self, num: int, user: int) -> bool:
        if self.users[num] == user:
            self.users[num] = -1
            return True
        return False


    def upgrade(self, num: int, user: int) -> bool:
        def ancestor() -> bool:
            cur = self.parent[num]
            while ~cur:
                if ~self.users[cur]:
                    return True
                cur = self.parent[cur]
            return False

        def check(num: int) -> bool:
            result = self.users[num] != -1
            self.users[num] = -1
            for child in self.children[num]:
                result |= check(child)
            return result
        if result := not ~self.users[num] and not ancestor() and check(num):
            self.users[num] = user
        return result



# Your LockingTree object will be instantiated and called as such:
# obj = LockingTree(parent)
# param_1 = obj.lock(num,user)
# param_2 = obj.unlock(num,user)
# param_3 = obj.upgrade(num,user)
```
