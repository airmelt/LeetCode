# 1600 Throne Inheritance 王位继承顺序

__Description:__

A kingdom consists of a king, his children, his grandchildren, and so on. Every once in a while, someone in the family dies or a child is born.

The kingdom has a well-defined order of inheritance that consists of the king as the first member. Let's define the recursive function `Successor(x, curOrder)`, which given a person `x` and the inheritance order so far, returns who should be the next person after `x` in the order of inheritance.

Successor(x, curOrder):
    if x has no children or all of x's children are in curOrder:
        if x is the king return null
        else return Successor(x's parent, curOrder)
    else return x's oldest child who's not in curOrder

For example, assume we have a kingdom that consists of the king, his children Alice and Bob (Alice is older than Bob), and finally Alice's son Jack.

Using the above function, we can always obtain a unique order of inheritance.

Implement the `ThroneInheritance` class:

- `ThroneInheritance(string kingName)` Initializes an object of the `ThroneInheritance` class. The name of the king is given as part of the constructor.
- `void birth(string parentName, string childName)` Indicates that `parentName` gave birth to `childName`.
- `void death(string name)` Indicates the death of `name`. The death of the person doesn't affect the `Successor` function nor the current inheritance order. You can treat it as just marking the person as dead.
- `string[] getInheritanceOrder()` Returns a list representing the current order of inheritance __excluding__ dead people.

__Example:__

Example 1:

```text
Input
["ThroneInheritance", "birth", "birth", "birth", "birth", "birth", "birth", "getInheritanceOrder", "death", "getInheritanceOrder"]
[["king"], ["king", "andy"], ["king", "bob"], ["king", "catherine"], ["andy", "matthew"], ["bob", "alex"], ["bob", "asha"], [null], ["bob"], [null]]
Output
[null, null, null, null, null, null, null, ["king", "andy", "matthew", "bob", "alex", "asha", "catherine"], null, ["king", "andy", "matthew", "alex", "asha", "catherine"]]
```

Explanation

```Java
ThroneInheritance t = new ThroneInheritance("king"); // order: king
t.birth("king", "andy"); // order: king > andy
t.birth("king", "bob"); // order: king > andy > bob
t.birth("king", "catherine"); // order: king > andy > bob > catherine
t.birth("andy", "matthew"); // order: king > andy > matthew > bob > catherine
t.birth("bob", "alex"); // order: king > andy > matthew > bob > alex > catherine
t.birth("bob", "asha"); // order: king > andy > matthew > bob > alex > asha > catherine
t.getInheritanceOrder(); // return ["king", "andy", "matthew", "bob", "alex", "asha", "catherine"]
t.death("bob"); // order: king > andy > matthew > bob > alex > asha > catherine
t.getInheritanceOrder(); // return ["king", "andy", "matthew", "alex", "asha", "catherine"]
```

__Constraints:__

- `1 <= kingName.length, parentName.length, childName.length, name.length <= 15`
- `kingName`, `parentName`, `childName`, and `name` consist of lowercase English letters only.
- All arguments `childName` and `kingName` are __distinct__.
- All `name` arguments of `death` will be passed to either the constructor or as `childName` to `birth` first.
- For each call to `birth(parentName, childName)`, it is guaranteed that `parentName` is alive.
- At most `10 ^ 5` calls will be made to `birth` and `death`.
- At most `10` calls will be made to `getInheritanceOrder`.

__题目描述:__

一个王国里住着国王、他的孩子们、他的孙子们等等。每一个时间点，这个家庭里有人出生也有人死亡。

这个王国有一个明确规定的王位继承顺序，第一继承人总是国王自己。我们定义递归函数 `Successor(x, curOrder)` ，给定一个人 `x` 和当前的继承顺序，该函数返回 `x` 的下一继承人。

Successor(x, curOrder):
    如果 x 没有孩子或者所有 x 的孩子都在 curOrder 中：
        如果 x 是国王，那么返回 null
        否则，返回 Successor(x 的父亲, curOrder)
    否则，返回 x 不在 curOrder 中最年长的孩子

比方说，假设王国由国王，他的孩子 Alice 和 Bob （Alice 比 Bob 年长）和 Alice 的孩子 Jack 组成。

通过以上的函数，我们总是能得到一个唯一的继承顺序。

请你实现 `ThroneInheritance` 类:

- `ThroneInheritance(string kingName)` 初始化一个 `ThroneInheritance` 类的对象。国王的名字作为构造函数的参数传入。
- `void birth(string parentName, string childName)` 表示 `parentName` 新拥有了一个名为 `childName` 的孩子。
- `void death(string name)` 表示名为 `name` 的人死亡。一个人的死亡不会影响 `Successor` 函数，也不会影响当前的继承顺序。你可以只将这个人标记为死亡状态。
- `string[] getInheritanceOrder()` 返回 __除去__ 死亡人员的当前继承顺序列表。

__示例:__

示例：

```text
输入：
["ThroneInheritance", "birth", "birth", "birth", "birth", "birth", "birth", "getInheritanceOrder", "death", "getInheritanceOrder"]
[["king"], ["king", "andy"], ["king", "bob"], ["king", "catherine"], ["andy", "matthew"], ["bob", "alex"], ["bob", "asha"], [null], ["bob"], [null]]
输出：
[null, null, null, null, null, null, null, ["king", "andy", "matthew", "bob", "alex", "asha", "catherine"], null, ["king", "andy", "matthew", "alex", "asha", "catherine"]]
```

解释：

```Java
ThroneInheritance t = new ThroneInheritance("king"); // 继承顺序：king
t.birth("king", "andy"); // 继承顺序：king > andy
t.birth("king", "bob"); // 继承顺序：king > andy > bob
t.birth("king", "catherine"); // 继承顺序：king > andy > bob > catherine
t.birth("andy", "matthew"); // 继承顺序：king > andy > matthew > bob > catherine
t.birth("bob", "alex"); // 继承顺序：king > andy > matthew > bob > alex > catherine
t.birth("bob", "asha"); // 继承顺序：king > andy > matthew > bob > alex > asha > catherine
t.getInheritanceOrder(); // 返回 ["king", "andy", "matthew", "bob", "alex", "asha", "catherine"]
t.death("bob"); // 继承顺序：king > andy > matthew > bob（已经去世）> alex > asha > catherine
t.getInheritanceOrder(); // 返回 ["king", "andy", "matthew", "alex", "asha", "catherine"]
```

__提示：__

- `1 <= kingName.length, parentName.length, childName.length, name.length <= 15`
- `kingName`， `parentName`， `childName` 和 `name` 仅包含小写英文字母。
- 所有的参数 `childName` 和 `kingName` __互不相同__。
- 所有 `death` 函数中的死亡名字 `name` 要么是国王，要么是已经出生了的人员名字。
- 每次调用 `birth(parentName, childName)` 时，测试用例都保证 `parentName` 对应的人员是活着的。
- 最多调用 `10 ^ 5` 次 `birth` 和 `death` 。
- 最多调用 `10` 次 `getInheritanceOrder` 。

__思路:__

```text
前序遍历
用一个哈希映射存放所有后代
用一个哈希表存放所有死去的人
每次查询继承顺序时, 跳过死去的人, 按照前序遍历的方式将所有后代按照顺序查询并返回
birth() 函数时间复杂度为 O(1)
death() 函数时间复杂度为 O(1)
getInheritanceOrder() 函数时间复杂度为 O(N)
空间复杂度为 O(N)
人名所占时间和空间复杂度不计
```

__代码:__

__C++__:

```C++
class ThroneInheritance 
{
private:
    unordered_map<string, vector<string>> edges;
    unordered_set<string> dead;
    string king;
public:
    ThroneInheritance(string kingName): king{move(kingName)} {}
    
    void birth(string parentName, string childName) 
    {
        edges[move(parentName)].push_back(move(childName));
    }
    
    void death(string name) 
    {
        dead.insert(move(name));
    }
    
    vector<string> getInheritanceOrder() 
    {
        vector<string> result;
        function<void(const string&)> preorder = [&](const string& name) 
        {
            if (dead.find(name) == dead.end()) result.emplace_back(name);
            if (edges.find(name) != edges.end()) for (const string& child: edges[name]) preorder(child);
        };
        preorder(king);
        return result;
    }
};

/**
 * Your ThroneInheritance object will be instantiated and called as such:
 * ThroneInheritance* obj = new ThroneInheritance(kingName);
 * obj->birth(parentName,childName);
 * obj->death(name);
 * vector<string> param_3 = obj->getInheritanceOrder();
 */
```

__Java__:

```Java
class ThroneInheritance 
{
    private Map<String, List<String>> edges;
    private Set<String> dead;
    private String king;

    public ThroneInheritance(String kingName) {
        edges = new HashMap<String, List<String>>();
        dead = new HashSet<String>();
        king = kingName;
    }
    
    public void birth(String parentName, String childName) {
        List<String> children = edges.getOrDefault(parentName, new ArrayList<String>());
        children.add(childName);
        edges.put(parentName, children);
    }
    
    public void death(String name) {
        dead.add(name);
    }
    
    public List<String> getInheritanceOrder() {
        List<String> result = new ArrayList<String>();
        preorder(result, king);
        return result;
    }

    private void preorder(List<String> result, String name) {
        if (!dead.contains(name)) result.add(name);
        List<String> children = edges.getOrDefault(name, new ArrayList<String>());
        for (String child : children) preorder(result, child);
    }
}

/**
 * Your ThroneInheritance object will be instantiated and called as such:
 * ThroneInheritance obj = new ThroneInheritance(kingName);
 * obj.birth(parentName,childName);
 * obj.death(name);
 * List<String> param_3 = obj.getInheritanceOrder();
 */
```

__Python__:

```Python
class ThroneInheritance:

    def __init__(self, kingName: str):
        self.edges = defaultdict(list)
        self.dead = set()
        self.king = kingName
        

    def birth(self, parentName: str, childName: str) -> None:
        self.edges[parentName].append(childName)

        
    def death(self, name: str) -> None:
        self.dead.add(name)

        
    def getInheritanceOrder(self) -> List[str]:
        result = []

        def preorder(name: str) -> None:
            if name not in self.dead:
                result.append(name)
            if name in self.edges:
                for child in self.edges[name]:
                    preorder(child)
        preorder(self.king)
        return result
    

# Your ThroneInheritance object will be instantiated and called as such:
# obj = ThroneInheritance(kingName)
# obj.birth(parentName,childName)
# obj.death(name)
# param_3 = obj.getInheritanceOrder()
```
