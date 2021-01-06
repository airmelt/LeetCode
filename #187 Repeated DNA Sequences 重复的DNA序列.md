# 187 Repeated DNA Sequences 重复的DNA序列

__Description__:
All DNA is composed of a series of nucleotides abbreviated as A, C, G, and T, for example: "ACGAATTCCG". When studying DNA, it is sometimes useful to identify repeated sequences within the DNA.

Write a function to find all the 10-letter-long sequences (substrings) that occur more than once in a DNA molecule.

__Example:__

Input: s = "AAAAACCCCCAAAAACCCCCCAAAAAGGGTTT"

Output: ["AAAAACCCCC", "CCCCCAAAAA"]

__题目描述__:
所有 DNA 都由一系列缩写为 A，C，G 和 T 的核苷酸组成，例如：“ACGAATTCCG”。在研究 DNA 时，识别 DNA 中的重复序列有时会对研究非常有帮助。

编写一个函数来查找目标子串，目标子串的长度为 10，且在 DNA 字符串 s 中出现次数超过一次。

__示例 :__

输入：s = "AAAAACCCCCAAAAACCCCCCAAAAAGGGTTT"
输出：["AAAAACCCCC", "CCCCCAAAAA"]

__思路__:

将 ACTG与 0123对应, 即对应二进制 00, 01, 10, 11
用位运算加快查找
由于子字符串长度为 10, 所以需要 20位, 表示范围为 0 ~ 2 ^ 20 - 1
时间复杂度O(n), 空间复杂度O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    vector<string> findRepeatedDnaSequences(string s) 
    {
        vector<string> result;
        unordered_map<char, int> m{{'A', 0}, {'C', 1}, {'T', 2}, {'G', 3}};
        bitset<1 << 20> b1, b2;
        int val = 0, mask = (1 << 20) - 1;
        for (int i = 0; i < 10; i++) val = (val << 2) | m[s[i]];
        b1.set(val);
        for (int i = 10; i < s.size(); i++)
        {
            val = ((val << 2) & mask) | m[s[i]];
            if (b2.test(val)) continue;
            if (b1.test(val))
            {
                result.push_back(s.substr(i - 9, 10));
                b2.set(val);
            }
            else b1.set(val);
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public List<String> findRepeatedDnaSequences(String s) {
        List<String> result = new ArrayList<>();
        if (s == null | s.length() < 10) return result;
        Map<Character, Integer> map = new HashMap<>() {
            {
            put('A', 0);
            put('C', 1);
            put('T', 2);
            put('G', 3);
            }
        };
        int val = 0, mask = (1 << 20) - 1;
        boolean b1[] = new boolean[1 << 20], b2[] = new boolean[1 << 20];
        for (int i = 0; i < 10; i++) val = (val << 2) | map.get(s.charAt(i));
        b1[val] = true;
        for (int i = 10; i < s.length(); i++) {
            val = ((val << 2) & mask) | map.get(s.charAt(i));
            if (b2[val]) continue;
            if (b1[val]) {
                result.add(s.substring(i - 9, i + 1));
                b2[val] = true;
            }
            else b1[val] = true;
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def findRepeatedDnaSequences(self, s: str) -> List[str]:
        d = collections.Counter(s[i: i + 10] for i in range(len(s) - 9))
        return list(filter(lambda i: d[i] > 1, d))
```
