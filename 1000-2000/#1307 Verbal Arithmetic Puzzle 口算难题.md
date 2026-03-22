# 1307 Verbal Arithmetic Puzzle 口算难题

__Description:__

Given an equation, represented by words on the left side and the result on the right side.

You need to check if the equation is solvable under the following rules:

Each character is decoded as one digit (0 - 9).
No two characters can map to the same digit.
Each words[i] and result are decoded as one number without leading zeros.
Sum of numbers on the left side (words) will equal to the number on the right side (result).
Return true if the equation is solvable, otherwise return false.

__Example:__

Example 1:

Input: words = ["SEND","MORE"], result = "MONEY"
Output: true
Explanation: Map 'S'-> 9, 'E'->5, 'N'->6, 'D'->7, 'M'->1, 'O'->0, 'R'->8, 'Y'->'2'
Such that: "SEND" + "MORE" = "MONEY" ,  9567 + 1085 = 10652

Example 2:

Input: words = ["SIX","SEVEN","SEVEN"], result = "TWENTY"
Output: true
Explanation: Map 'S'-> 6, 'I'->5, 'X'->0, 'E'->8, 'V'->7, 'N'->2, 'T'->1, 'W'->'3', 'Y'->4
Such that: "SIX" + "SEVEN" + "SEVEN" = "TWENTY" ,  650 + 68782 + 68782 = 138214

Example 3:

Input: words = ["LEET","CODE"], result = "POINT"
Output: false
Explanation: There is no possible mapping to satisfy the equation, so we return false.
Note that two different characters cannot map to the same digit.

__Constraints:__

2 <= words.length <= 5
1 <= words[i].length, result.length <= 7
words[i], result contain only uppercase English letters.
The number of different characters used in the expression is at most 10.

__题目描述：__

给你一个方程，左边用 words 表示，右边用 result 表示。

你需要根据以下规则检查方程是否可解：

每个字符都会被解码成一位数字（0 - 9）。
每对不同的字符必须映射到不同的数字。
每个 words[i] 和 result 都会被解码成一个没有前导零的数字。
左侧数字之和（words）等于右侧数字（result）。
如果方程可解，返回 True，否则返回 False。

__示例：__

示例 1：

输入：words = ["SEND","MORE"], result = "MONEY"
输出：true
解释：映射 'S'-> 9, 'E'->5, 'N'->6, 'D'->7, 'M'->1, 'O'->0, 'R'->8, 'Y'->'2'
所以 "SEND" + "MORE" = "MONEY" ,  9567 + 1085 = 10652

示例 2：

输入：words = ["SIX","SEVEN","SEVEN"], result = "TWENTY"
输出：true
解释：映射 'S'-> 6, 'I'->5, 'X'->0, 'E'->8, 'V'->7, 'N'->2, 'T'->1, 'W'->'3', 'Y'->4
所以 "SIX" + "SEVEN" + "SEVEN" = "TWENTY" ,  650 + 68782 + 68782 = 138214

示例 3：

输入：words = ["THIS","IS","TOO"], result = "FUNNY"
输出：true

示例 4：

输入：words = ["LEET","CODE"], result = "POINT"
输出：false

__提示：__

2 <= words.length <= 5
1 <= words[i].length, results.length <= 7
words[i], result 只含有大写英文字母
表达式中使用的不同字符数最大为 10

__思路：__

DFS
先将字母按照位置换算成整数
比如 "SEND" = 'D' \* 1 + 'N' \* 10 + 'E' \* 100 + 'S' \* 1000
将等式两边的同类项进行合并, 并移动到等式的同一侧
比如 "SEND" + "MORE" = "MONEY" 可以转化为
'S' \* 1000 + 'E' \* 91 - 'N' \* 90 + 'D' - 'M' \* 9000 - 'O' \* 900 + 'R' \* 10 - 'Y' = 0
这样只需要对每一个字符搜索即可
注意到每个字符的系数不同, 可以通过系数来剪枝
按照系数的绝对值进行排序
'M' \* (-9000) + 'S' \* 1000 + 'O' \* (-900) + 'E' \* 91 + 'N' \* (-90) + 'R' \* 10 + 'D' \* 1 + 'Y' \* (-1) = 0
如果 'M' 大于 2, 那么剩下的数字无论取多少都不能使等式成立
搜索 'M' 时, 可以将 'S' \* 1000 + 'O' \* (-900) + 'E' \* 91 + 'N' \* (-90) + 'R' \* 10 + 'D' \* 1 + 'Y' \* (-1)
按照正负系数分开处理
系数为正：S \* 1000 + E \* 91 + R \* 10 + D
系数为负：O \* 900 + N \* 90 + Y
从大到小依次赋予 9, 8... 给正数系数以及从小到大依次赋予 0, 1... 给负数系数可以得到式子的最大值, 最小值的处理则相反
这样以来可以确定 'M' 的取值范围为 [-9712, 8713], 所以 'M' 只能为 1
然后依次确定其他字符的值
时间复杂度为 O(10 ^ n), 空间复杂度为 O(n)

__代码__:

__C++__:

```C++
using PCI = pair<char, int>;

class Solution {
private:
    vector<PCI> weight;
    vector<int> suffix_sum_min, suffix_sum_max;
    vector<int> lead_zero;
    bool used[10];

public:
    int pow10(int x) {
        int ret = 1;
        for (int i = 0; i < x; ++i) {
            ret *= 10;
        }
        return ret;
    }

    bool dfs(int pos, int total) {
        if (pos == weight.size()) {
            return total == 0;
        }
        if (!(total + suffix_sum_min[pos] <= 0 && 0 <= total + suffix_sum_max[pos])) {
            return false;
        }
        for (int i = lead_zero[pos]; i < 10; ++i) {
            if (!used[i]) {
                used[i] = true;
                bool check = dfs(pos + 1, total + weight[pos].second * i);
                used[i] = false;
                if (check) {
                    return true;
                }
            }
        }
        return false;
    }

    bool isSolvable(vector<string>& words, string result) {
        unordered_map<char, int> _weight;
        unordered_set<char> _lead_zero;
        for (const string& word: words) {
            for (int i = 0; i < word.size(); ++i) {
                _weight[word[i]] += pow10(word.size() - i - 1);
            }
            if (word.size() > 1) {
                _lead_zero.insert(word[0]);
            }
        }
        for (int i = 0; i < result.size(); ++i) {
            _weight[result[i]] -= pow10(result.size() - i - 1);
        }
        if (result.size() > 1) {
            _lead_zero.insert(result[0]);
        }

        weight = vector<PCI>(_weight.begin(), _weight.end());
        sort(weight.begin(), weight.end(), [](const PCI& u, const PCI& v) {
            return abs(u.second) > abs(v.second);
        });
        int n = weight.size();
        suffix_sum_min.resize(n);
        suffix_sum_max.resize(n);
        for (int i = 0; i < n; ++i) {
            vector<int> suffix_pos, suffix_neg;
            for (int j = i; j < n; ++j) {
                if (weight[j].second > 0) {
                    suffix_pos.push_back(weight[j].second);
                }
                else if (weight[j].second < 0) {
                    suffix_neg.push_back(weight[j].second);
                }
                sort(suffix_pos.begin(), suffix_pos.end());
                sort(suffix_neg.begin(), suffix_neg.end());
            }
            for (int j = 0; j < suffix_pos.size(); ++j) {
                suffix_sum_min[i] += (suffix_pos.size() - 1 - j) * suffix_pos[j];
                suffix_sum_max[i] += (10 - suffix_pos.size() + j) * suffix_pos[j];
            }
            for (int j = 0; j < suffix_neg.size(); ++j) {
                suffix_sum_min[i] += (9 - j) * suffix_neg[j];
                suffix_sum_max[i] += j * suffix_neg[j];
            }
        }

        lead_zero.resize(n);
        for (int i = 0; i < n; ++i) {
            lead_zero[i] = (_lead_zero.count(weight[i].first) ? 1 : 0);
        }
        
        memset(used, false, sizeof(used));
        return dfs(0, 0);
    }
};
```

__Java__:

```Java
class Solution {
    class Pair {
        char c;
        int val;
        Pair(char c, int val) {
            this.c = c;
            this.val = val;
        }
    }
    
    List<Pair> weight;
    List<Integer> suffixSumMin, suffixSumMax;
    List<Integer> leadZero;
    boolean[] used;
    
    private int pow10(int x) {
        int result = 1;
        for (int i = 0; i < x; i++) result *= 10;
        return result;
    }
    
    private boolean dfs(int pos, int total) {
        if (pos == weight.size()) return total == 0;
        if (!(total + suffixSumMin.get(pos) <= 0 && total + suffixSumMax.get(pos) >= 0)) return false;
        for (int i = leadZero.get(pos); i < 10; i++) {
            if (!used[i]) {
                used[i] = true;
                boolean check = dfs(pos + 1, total + weight.get(pos).val * i);
                used[i] = false;
                if (check) return true;
            }
        }
        return false;
    }
    
    public boolean isSolvable(String[] words, String result) {
        used = new boolean[10];
        weight = new ArrayList<>();
        suffixSumMin = new ArrayList<>();
        suffixSumMax = new ArrayList<>();
        leadZero = new ArrayList<>();
        Map<Character, Integer> _weight = new HashMap<>();
        Set<Character> _leadZero = new HashSet<>();
        for (String word : words) {
            for (int i = 0; i < word.length(); i++) _weight.put(word.charAt(i), _weight.getOrDefault(word.charAt(i), 0) + pow10(word.length() - i - 1));
            if (word.length() > 1) _leadZero.add(word.charAt(0));
        }
        for (int i = 0; i < result.length(); i++) _weight.put(result.charAt(i), _weight.getOrDefault(result.charAt(i), 0) - pow10(result.length() - i - 1));
        if (result.length() > 1) _leadZero.add(result.charAt(0));
        for (Map.Entry<Character, Integer> entry : _weight.entrySet()) weight.add(new Pair(entry.getKey(), entry.getValue()));
        Collections.sort(weight, (a, b) -> Integer.compare(Math.abs(a.val), Math.abs(b.val)));
        int n = weight.size();
        for (int i = 0; i < n; i++) {
            suffixSumMin.add(0);
            suffixSumMax.add(0);
            leadZero.add(0);
        }
        for (int i = 0; i < n; i++) {
            List<Integer> suffixPos = new ArrayList<>(), suffixNeg = new ArrayList<>();
            for (int j = i; j < n; j++) {
                if (weight.get(j).val > 0) suffixPos.add(weight.get(j).val);
                else if (weight.get(j).val < 0) suffixNeg.add(weight.get(j).val);
                Collections.sort(suffixPos);
                Collections.sort(suffixNeg);
            }
            for (int j = 0; j < suffixPos.size(); j++) {
                suffixSumMin.set(i, (suffixPos.size() - 1 - j) * suffixPos.get(j) + suffixSumMin.get(i));
                suffixSumMax.set(i, (10 - suffixPos.size() + j) * suffixPos.get(j) + suffixSumMax.get(i));
            }
            for (int j = 0; j < suffixNeg.size(); j++) {
                suffixSumMin.set(i, (9 - j) * suffixNeg.get(j) + suffixSumMin.get(i));
                suffixSumMax.set(i, j * suffixNeg.get(j) + suffixSumMax.get(i));
            }
        }
        for (int i = 0; i < n; i++) leadZero.set(i, _leadZero.contains(weight.get(i).c) ? 1 : 0);
        return dfs(0, 0);
    }
}
```

__Python__:

```Python
class Solution:
    def isSolvable(self, words: List[str], result: str) -> bool:
        if (max_len := max(max(len(word) for word in words), (n := len(result)))) > n:
            return False
        words, result, visited, m, all_num, used_num = [word.rjust(max_len, '#') for word in words], result.rjust(max_len, '#'), [False] * 10, len(words) + 1, set(range(10)), set()
        arr, char_dict = [x for x in zip(*words, result)][::-1], {'#': 0}
        def dfs(pos: int, idx: int, carry: int) -> bool:
            if pos == max_len:
                return not carry and max_len == 1 or all(char_dict[arr[-1][i]] or arr[-1][i] == '#' for i in range(m)) and sum(reduce(lambda x, y: 10 * x + y, map(lambda x: char_dict[x], i[:list(i).index('#') if '#' in i else max_len][::-1])) for i in list(zip(*arr))[:-1]) == int(reduce(lambda x, y: 10 * x + y, map(lambda x: char_dict[x], list(zip(*arr))[-1][::-1])))
            if idx == m:
                return dfs(pos + 1, 0, s // 10) if (s := carry + sum(char_dict[t] for t in arr[pos][:-1])) % 10 == char_dict[arr[pos][-1]] else False
            if arr[pos][idx] in char_dict:
                return dfs(pos, idx + 1, carry)
            else:
                for num in all_num - used_num:
                    if ((pos + 1 < max_len and arr[pos + 1][idx] == "#") or pos - 1 == max_len) and not num: continue
                    used_num.add(num)
                    char_dict[arr[pos][idx]] = num
                    if dfs(pos, idx + 1, carry): 
                        return True
                    char_dict.pop(arr[pos][idx])
                    used_num.remove(num)
            return False
        return dfs(0, 0, 0)
```
