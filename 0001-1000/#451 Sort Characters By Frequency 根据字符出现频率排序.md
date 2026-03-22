# 451 Sort Characters By Frequency 根据字符出现频率排序

__Description__:
Given a string, sort it in decreasing order based on the frequency of characters.

__Example:__

Example 1:

Input:
"tree"

Output:
"eert"

Explanation:
'e' appears twice while 'r' and 't' both appear once.
So 'e' must appear before both 'r' and 't'. Therefore "eetr" is also a valid answer.

Example 2:

Input:
"cccaaa"

Output:
"cccaaa"

Explanation:
Both 'c' and 'a' appear three times, so "aaaccc" is also a valid answer.
Note that "cacaca" is incorrect, as the same characters must be together.

Example 3:

Input:
"Aabb"

Output:
"bbAa"

Explanation:
"bbaA" is also a valid answer, but "Aabb" is incorrect.
Note that 'A' and 'a' are treated as two different characters.

__题目描述__:

给定一个字符串，请将字符串里的字符按照出现的频率降序排列。

__示例 :__

示例 1:

输入:
"tree"

输出:
"eert"

解释:
'e'出现两次，'r'和't'都只出现一次。
因此'e'必须出现在'r'和't'之前。此外，"eetr"也是一个有效的答案。

示例 2:

输入:
"cccaaa"

输出:
"cccaaa"

解释:
'c'和'a'都出现三次。此外，"aaaccc"也是有效的答案。
注意"cacaca"是不正确的，因为相同的字母必须放在一起。

示例 3:

输入:
"Aabb"

输出:
"bbAa"

解释:
此外，"bbaA"也是一个有效的答案，但"Aabb"是不正确的。
注意'A'和'a'被认为是两种不同的字符。

__思路__:

用一个数组或者 map记录出现的字符和次数
排序或者每次选择最大值加入结果
时间复杂度O(n), 空间复杂度O(1), 排序时间复杂度O(nlgn)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    string frequencySort(string s) 
    {
        vector<int> count(128, 0);
        for (auto c : s) ++count[c];
        int index = 0, n = s.size();
        string result(n, 42);
        while (index < n) 
        {
            int max = 0, c = 0;
            for (int i = 0; i < 128; i++) 
            {
                if (count[i] > max) 
                {
                    max = count[i];
                    c = i;
                }
            }
            count[c] = 0;
            int cur = 0;
            while (index < n and cur++ < max) result[index++] = (char)c;
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public String frequencySort(String s) {
        Map<Character, Integer> map = new HashMap<>();
        for (int i = 0; i < s.length(); i++) map.put(s.charAt(i), map.getOrDefault(s.charAt(i), 0) + 1);
        PriorityQueue<Character> pq = new PriorityQueue<>((o1, o2) -> map.get(o2) - map.get(o1));
        pq.addAll(map.keySet());
        StringBuilder result = new StringBuilder();
        while (!pq.isEmpty()) {
            char key = pq.poll();
            int value = map.get(key);
            for (int i = 0; i < value; i++) result.append(key);
        }
        return result.toString();
    }
}
```

__Python__:

```Python
class Solution:
    def frequencySort(self, s: str) -> str:
        return ''.join(c * k for c, k in (sorted(collections.Counter(s).items(), key=lambda x: x[1], reverse=True))) if s else ''
```
