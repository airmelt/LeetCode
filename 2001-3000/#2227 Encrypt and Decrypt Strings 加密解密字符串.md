# 2227 Encrypt and Decrypt Strings 加密解密字符串

__Description:__

You are given a character array `keys` containing __unique__ characters and a string array `values` containing strings of length 2. You are also given another string array `dictionary` that contains all permitted original strings after decryption. You should implement a data structure that can encrypt or decrypt a __0-indexed__ string.

A string is encrypted with the following process:

Note that in case a character of the string is __not present__ in `keys`, the encryption process cannot be carried out, and an empty string `""` is returned.

A string is decrypted with the following process:

Implement the `Encrypter` class:

- `Encrypter(char[] keys, String[] values, String[] dictionary)` Initializes the `Encrypter` class with `keys, values`, and `dictionary`.
- `String encrypt(String word1)` Encrypts `word1` with the encryption process described above and returns the encrypted string.
- `int decrypt(String word2)` Returns the number of possible strings `word2` could decrypt to that also appear in `dictionary`.

__Example:__

Example 1:

```text
Input
["Encrypter", "encrypt", "decrypt"]
[[['a', 'b', 'c', 'd'], ["ei", "zf", "ei", "am"], ["abcd", "acbd", "adbc", "badc", "dacb", "cadb", "cbda", "abad"]], ["abcd"], ["eizfeiam"]]
Output
[null, "eizfeiam", 2]

Explanation
```

```Java
Encrypter encrypter = new Encrypter([['a', 'b', 'c', 'd'], ["ei", "zf", "ei", "am"], ["abcd", "acbd", "adbc", "badc", "dacb", "cadb", "cbda", "abad"]);
encrypter.encrypt("abcd"); // return "eizfeiam". 
                           // 'a' maps to "ei", 'b' maps to "zf", 'c' maps to "ei", and 'd' maps to "am".
encrypter.decrypt("eizfeiam"); // return 2. 
                              // "ei" can map to 'a' or 'c', "zf" maps to 'b', and "am" maps to 'd'. 
                              // Thus, the possible strings after decryption are "abad", "cbad", "abcd", and "cbcd". 
                              // 2 of those strings, "abad" and "abcd", appear in dictionary, so the answer is 2.
```

__Constraints:__

- `1 <= keys.length == values.length <= 26`
- `values[i].length == 2`
- `1 <= dictionary.length <= 100`
- `1 <= dictionary[i].length <= 100`
- All `keys[i]` and `dictionary[i]` are __unique__.
- `1 <= word1.length <= 2000`
- `1 <= word2.length <= 200`
- All `word1[i]` appear in `keys`.
- `word2.length` is even.
- `keys`, `values[i]`, `dictionary[i]`, `word1`, and `word2` only contain lowercase English letters.
- At most `200` calls will be made to `encrypt` and `decrypt` __in total__.

__题目描述:__

给你一个字符数组 `keys` ，由若干 __互不相同__ 的字符组成。还有一个字符串数组 `values` ，内含若干长度为 2 的字符串。另给你一个字符串数组 `dictionary` ，包含解密后所有允许的原字符串。请你设计并实现一个支持加密及解密下标从 __0__ 开始字符串的数据结构。

字符串 加密 按下述步骤进行：

请注意，如果 `keys` 中不存在字符串中的字符，则无法执行加密过程，返回空字符串 `""`。

字符串 解密 按下述步骤进行：

实现 `Encrypter` 类:

- `Encrypter(char[] keys, String[] values, String[] dictionary)` 用 `keys`、 `values` 和 `dictionary` 初始化 `Encrypter` 类。
- `String encrypt(String word1)` 按上述加密过程完成对 `word1` 的加密，并返回加密后的字符串。
- `int decrypt(String word2)` 统计并返回可以由 `word2` 解密得到且出现在 `dictionary` 中的字符串数目。

__示例:__

示例：

```text
输入：
["Encrypter", "encrypt", "decrypt"]
[[['a', 'b', 'c', 'd'], ["ei", "zf", "ei", "am"], ["abcd", "acbd", "adbc", "badc", "dacb", "cadb", "cbda", "abad"]], ["abcd"], ["eizfeiam"]]
输出：
[null, "eizfeiam", 2]

解释：
```

```Java
Encrypter encrypter = new Encrypter([['a', 'b', 'c', 'd'], ["ei", "zf", "ei", "am"], ["abcd", "acbd", "adbc", "badc", "dacb", "cadb", "cbda", "abad"]);
encrypter.encrypt("abcd"); // 返回 "eizfeiam"。 
                           // 'a' 映射为 "ei"，'b' 映射为 "zf"，'c' 映射为 "ei"，'d' 映射为 "am"。
encrypter.decrypt("eizfeiam"); // return 2. 
                              // "ei" 可以映射为 'a' 或 'c'，"zf" 映射为 'b'，"am" 映射为 'd'。 
                              // 因此，解密后可以得到的字符串是 "abad"，"cbad"，"abcd" 和 "cbcd"。 
                              // 其中 2 个字符串，"abad" 和 "abcd"，在 dictionary 中出现，所以答案是 2 。
```

__提示：__

- `1 <= keys.length == values.length <= 26`
- `values[i].length == 2`
- `1 <= dictionary.length <= 100`
- `1 <= dictionary[i].length <= 100`
- 所有 `keys[i]` 和 `dictionary[i]` __互不相同__
- `1 <= word1.length <= 2000`
- `1 <= word2.length <= 200`
- 所有 `word1[i]` 都出现在 `keys` 中
- `word2.length` 是偶数
- `keys`、 `values[i]`、 `dictionary[i]`、 `word1` 和 `word2` 只含小写英文字母
- 至多调用 `encrypt` 和 `decrypt` __总计__ `200` 次

__思路:__

```text
模拟
使用哈希表记录加密和解密的映射关系
对每一个 key 记录对应的 value
使用哈希表记录加密后的字符串出现的次数
解密直接返回对应的次数
加密时如果字符在哈希表中不存在, 返回空字符串
否则结果加上对应的 value
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Encrypter 
{
public:
    Encrypter(vector<char>& keys, vector<string>& values, vector<string>& dictionary) 
    {
        for (int i = 0, n = keys.size(); i < n; i++) m[keys[i]] = values[i];
        for (const auto& s : dictionary) ++counter[encrypt(s)];
    }
    
    string encrypt(string word1) 
    {
        string result = "";
        for (const auto& c : word1) 
        {
            if (!m.count(c)) return "";
            result += m[c];
        }
        return result;
    }
    
    int decrypt(string word2) 
    {
        return counter[word2];
    }
private:
    unordered_map<char, string> m;
    unordered_map<string, int> counter;
};

/**
 * Your Encrypter object will be instantiated and called as such:
 * Encrypter* obj = new Encrypter(keys, values, dictionary);
 * string param_1 = obj->encrypt(word1);
 * int param_2 = obj->decrypt(word2);
 */
```

__Java__:

```Java
class Encrypter {
    private String[] map = new String[26];
    private Map<String, Integer> counter = new HashMap<>();

    public Encrypter(char[] keys, String[] values, String[] dictionary) {
        for (int i = 0, n = keys.length; i < n; i++) map[keys[i] - 'a'] = values[i];
        for (String s : dictionary) counter.merge(encrypt(s), 1, Integer::sum);
    }
    
    public String encrypt(String word1) {
        StringBuilder result = new StringBuilder();
        for (char c : word1.toCharArray()) {
            if (map[c - 'a'] == null) return "";
            result.append(map[c - 'a']);
        }
        return result.toString();
    }
    
    public int decrypt(String word2) {
        return counter.getOrDefault(word2, 0);
    }
}

/**
 * Your Encrypter object will be instantiated and called as such:
 * Encrypter obj = new Encrypter(keys, values, dictionary);
 * String param_1 = obj.encrypt(word1);
 * int param_2 = obj.decrypt(word2);
 */
```

__Python__:

```Python
class Encrypter:

    def __init__(self, keys: List[str], values: List[str], dictionary: List[str]):
        self.map = dict(zip(keys, values))
        self.count = Counter(self.encrypt(s) for s in dictionary)


    def encrypt(self, word1: str) -> str:
        result = ""
        for c in word1:
            if c not in self.map:
                return ""
            result += self.map[c]
        return result


    def decrypt(self, word2: str) -> int:
        return self.count[word2]



# Your Encrypter object will be instantiated and called as such:
# obj = Encrypter(keys, values, dictionary)
# param_1 = obj.encrypt(word1)
# param_2 = obj.decrypt(word2)
```
