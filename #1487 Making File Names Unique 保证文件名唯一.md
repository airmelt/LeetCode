# 1487 Making File Names Unique 保证文件名唯一

__Description:__

Given an array of strings `names` of size `n`. You will create `n` folders in your file system __such that__, at the `i ^ th` minute, you will create a folder with the name `names[i]`.

Since two files __cannot__ have the same name, if you enter a folder name that was previously used, the system will have a suffix addition to its name in the form of `(k)`, where, `k` is the __smallest positive integer__ such that the obtained name remains unique.

Return _an array of strings of length_ `n` where `ans[i]` is the actual name the system will assign to the `i ^ th` folder when you create it.

__Example:__

Example 1:

```text
Input: names = ["pes","fifa","gta","pes(2019)"]
Output: ["pes","fifa","gta","pes(2019)"]
Explanation: Let's see how the file system creates folder names:
"pes" --> not assigned before, remains "pes"
"fifa" --> not assigned before, remains "fifa"
"gta" --> not assigned before, remains "gta"
"pes(2019)" --> not assigned before, remains "pes(2019)"
```

Example 2:

```text
Input: names = ["gta","gta(1)","gta","avalon"]
Output: ["gta","gta(1)","gta(2)","avalon"]
Explanation: Let's see how the file system creates folder names:
"gta" --> not assigned before, remains "gta"
"gta(1)" --> not assigned before, remains "gta(1)"
"gta" --> the name is reserved, system adds (k), since "gta(1)" is also reserved, systems put k = 2. it becomes "gta(2)"
"avalon" --> not assigned before, remains "avalon"
```

Example 3:

```text
Input: names = ["onepiece","onepiece(1)","onepiece(2)","onepiece(3)","onepiece"]
Output: ["onepiece","onepiece(1)","onepiece(2)","onepiece(3)","onepiece(4)"]
Explanation: When the last folder is created, the smallest positive valid k is 4, and it becomes "onepiece(4)".
```

__Constraints:__

- `1 <= names.length <= 5 * 10 ^ 4`
- `1 <= names[i].length <= 20`
- `names[i]` consists of lowercase English letters, digits, and/or round brackets.

__题目描述:__

给你一个长度为 `n` 的字符串数组 `names` 。你将会在文件系统中创建 `n` 个文件夹：在第 `i` 分钟，新建名为 `names[i]` 的文件夹。

由于两个文件 __不能__ 共享相同的文件名，因此如果新建文件夹使用的文件名已经被占用，系统会以 `(k)` 的形式为新文件夹的文件名添加后缀，其中 `k` 是能保证文件名唯一的 __最小正整数__ 。

返回长度为 _`n`_ 的字符串数组，其中 `ans[i]` 是创建第 `i` 个文件夹时系统分配给该文件夹的实际名称。

__示例:__

示例 1：

```text
输入：names = ["pes","fifa","gta","pes(2019)"]
输出：["pes","fifa","gta","pes(2019)"]
解释：文件系统将会这样创建文件名：
"pes" --> 之前未分配，仍为 "pes"
"fifa" --> 之前未分配，仍为 "fifa"
"gta" --> 之前未分配，仍为 "gta"
"pes(2019)" --> 之前未分配，仍为 "pes(2019)"
```

示例 2：

```text
输入：names = ["gta","gta(1)","gta","avalon"]
输出：["gta","gta(1)","gta(2)","avalon"]
解释：文件系统将会这样创建文件名：
"gta" --> 之前未分配，仍为 "gta"
"gta(1)" --> 之前未分配，仍为 "gta(1)"
"gta" --> 文件名被占用，系统为该名称添加后缀 (k)，由于 "gta(1)" 也被占用，所以 k = 2 。实际创建的文件名为 "gta(2)" 。
"avalon" --> 之前未分配，仍为 "avalon"
```

示例 3：

```text
输入：names = ["onepiece","onepiece(1)","onepiece(2)","onepiece(3)","onepiece"]
输出：["onepiece","onepiece(1)","onepiece(2)","onepiece(3)","onepiece(4)"]
解释：当创建最后一个文件夹时，最小的正有效 k 为 4 ，文件名变为 "onepiece(4)"。
```

示例 4：

```text
输入：names = ["wano","wano","wano","wano"]
输出：["wano","wano(1)","wano(2)","wano(3)"]
解释：每次创建文件夹 "wano" 时，只需增加后缀中 k 的值即可。
```

示例 5：

```text
输入：names = ["kaido","kaido(1)","kaido","kaido(1)"]
输出：["kaido","kaido(1)","kaido(2)","kaido(1)(1)"]
解释：注意，如果含后缀文件名被占用，那么系统也会按规则在名称后添加新的后缀 (k) 。
```

__提示：__

- `1 <= names.length <= 5 * 10 ^ 4`
- `1 <= names[i].length <= 20`
- `names[i]` 由小写英文字母、数字和/或圆括号组成。

__思路:__

```text
哈希表
用哈希表存储已经使用过的文件名
记录使用过的文件名的次数
每次从哈希表中找出没被使用过的文件名
次数自增直到找到没使用过的文件名
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<string> getFolderNames(vector<string>& names) 
    {
        unordered_map<string, int> m;
        for (int i = 0, n = names.size(); i < n; i++) 
        {
            string cur = names[i];
            if (m.count(cur)) 
            {
                int num = m[cur] + 1;
                string next = cur + "(" + to_string(num) + ")";
                while (m.count(next)) next = cur + "(" + to_string(++num) + ")";
                m[cur] = num;
                names[i] = next;
                m[next] = 0;
            } else m[cur] = 0;
        }
        return names;
    }
};
```

__Java__:

```Java
class Solution {
    public String[] getFolderNames(String[] names) {
        Map<String, Integer> map = new HashMap<>();
        for (int i = 0, n = names.length; i < n; i++) {
            String cur = names[i];
            if (map.containsKey(cur)) {
                int num = map.get(cur) + 1;
                String next = cur + "(" + num + ")";
                while (map.containsKey(next)) next = cur + "(" + ++num + ")";
                map.put(cur, num);
                names[i] = next;
                map.put(next, 0);
            } else map.put(cur, 0);
        }
        return names;
    }
}
```

__Python__:

```Python
class Solution:
    def getFolderNames(self, names: List[str]) -> List[str]:
        return (d := {}) or [*map(lambda name: ((s := [name]), [*itertools.takewhile(lambda s: s[0] in d and (s.__setitem__(0, f'{name}({d[name]})'), d.__setitem__(name, d[name] + 1)), itertools.repeat(s))], d.__setitem__(s[0], 1)) and s[0], names)]
```
