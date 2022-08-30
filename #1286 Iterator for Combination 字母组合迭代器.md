# 1286 Iterator for Combination 字母组合迭代器

__Description:__

Design the CombinationIterator class:

CombinationIterator(string characters, int combinationLength) Initializes the object with a string characters of sorted distinct lowercase English letters and a number combinationLength as arguments.
next() Returns the next combination of length combinationLength in lexicographical order.
hasNext() Returns true if and only if there exists a next combination.

__Example:__

Example 1:

Input
["CombinationIterator", "next", "hasNext", "next", "hasNext", "next", "hasNext"]
[["abc", 2], [], [], [], [], [], []]
Output
[null, "ab", true, "ac", true, "bc", false]

Explanation

```Java
CombinationIterator itr = new CombinationIterator("abc", 2);
itr.next();    // return "ab"
itr.hasNext(); // return True
itr.next();    // return "ac"
itr.hasNext(); // return True
itr.next();    // return "bc"
itr.hasNext(); // return False
```

__Constraints:__

1 <= combinationLength <= characters.length <= 15
All the characters of characters are unique.
At most 10^4 calls will be made to next and hasNext.
It is guaranteed that all calls of the function next are valid.

__题目描述：__

请你设计一个迭代器类 CombinationIterator ，包括以下内容：

CombinationIterator(string characters, int combinationLength) 一个构造函数，输入参数包括：用一个 有序且字符唯一 的字符串 characters（该字符串只包含小写英文字母）和一个数字 combinationLength 。
函数 next() ，按 字典序 返回长度为 combinationLength 的下一个字母组合。
函数 hasNext() ，只有存在长度为 combinationLength 的下一个字母组合时，才返回 true

__示例：__

示例 1：

输入:
["CombinationIterator", "next", "hasNext", "next", "hasNext", "next", "hasNext"]
[["abc", 2], [], [], [], [], [], []]
输出：
[null, "ab", true, "ac", true, "bc", false]
解释：

```Java
CombinationIterator iterator = new CombinationIterator("abc", 2); // 创建迭代器 iterator
iterator.next(); // 返回 "ab"
iterator.hasNext(); // 返回 true
iterator.next(); // 返回 "ac"
iterator.hasNext(); // 返回 true
iterator.next(); // 返回 "bc"
iterator.hasNext(); // 返回 false
```

__提示：__

1 <= combinationLength <= characters.length <= 15
 characters 中每个字符都 不同
每组测试数据最多对 next 和 hasNext 调用 10^4次
题目保证每次调用函数 next 时都存在下一个字母组合。

__思路：__

回溯
DFS 预处理所有的字符串到一个列表中
直接用列表返回结果
也可以使用生成法
第一个组合为初始字符串的前 k 个字符
从第 k 个字符开始, 如果有没使用过的比当前位置更大的字符, 直接替换
否则往前寻找第一个可以替换的字符, 并将该位置到 k 位置上的字符都换成最小的
时间复杂度为 O(k), 空间复杂度为 O(1)

__代码__:

__C++__:

```C++
class CombinationIterator 
{
private:
    vector<string> list;
    string cur = "";
    int index = 0;
    
    void dfs(string& characters, int combinationLength, int index)
    {
        if (cur.size() == combinationLength) list.emplace_back(cur);
        for (int i = index; i < characters.length(); ++i) 
        {
            cur += characters[i];
            dfs(characters, combinationLength, i + 1);
            cur.erase(cur.size() - 1);
        }
    }
public:
    CombinationIterator(string characters, int combinationLength) 
    {
        dfs(characters, combinationLength, 0);
    }
    
    string next() 
    {
        return list[index++];
    }
    
    bool hasNext() 
    {
        return index < list.size();
    }
};

/**
 * Your CombinationIterator object will be instantiated and called as such:
 * CombinationIterator obj = new CombinationIterator(characters, combinationLength);
 * String param_1 = obj.next();
 * boolean param_2 = obj.hasNext();
 */
/**
 * Your CombinationIterator object will be instantiated and called as such:
 * CombinationIterator* obj = new CombinationIterator(characters, combinationLength);
 * string param_1 = obj->next();
 * bool param_2 = obj->hasNext();
 */
```

__Java__:

```Java
class CombinationIterator {
    private List<String> list = new LinkedList<>();
    private StringBuilder sb = new StringBuilder();
    private int index = 0;
    
    public CombinationIterator(String characters, int combinationLength) {
        dfs(characters, combinationLength, 0);
    }

    private void dfs(String characters, int combinationLength, int index){
        if (sb.length() == combinationLength) list.add(sb.toString());
        for (int i = index; i < characters.length(); i++) {
            sb.append(characters.charAt(i));
            dfs(characters, combinationLength, i + 1);
            sb.deleteCharAt(sb.length() - 1);
        }
    }
    
    public String next() {
        return list.get(index++);
    }
    
    public boolean hasNext() {
        return index < list.size();
    }
}

/**
 * Your CombinationIterator object will be instantiated and called as such:
 * CombinationIterator obj = new CombinationIterator(characters, combinationLength);
 * String param_1 = obj.next();
 * boolean param_2 = obj.hasNext();
 */
```

__Python__:

```Python
class CombinationIterator:

    def __init__(self, characters: str, combinationLength: int):
        self.data = list(combinations(characters, combinationLength))
        self.pos = 0
        self.n = len(self.data)


    def next(self) -> str:
        result = ''.join(self.data[self.pos])
        self.pos += 1
        return result


    def hasNext(self) -> bool:
        return self.pos < self.n



# Your CombinationIterator object will be instantiated and called as such:
# obj = CombinationIterator(characters, combinationLength)
# param_1 = obj.next()
# param_2 = obj.hasNext()
```
