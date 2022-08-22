# 1268 Search Suggestions System 搜索推荐系统

__Description:__

You are given an array of strings products and a string searchWord.

Design a system that suggests at most three product names from products after each character of searchWord is typed. Suggested products should have common prefix with searchWord. If there are more than three products with a common prefix return the three lexicographically minimums products.

Return a list of lists of the suggested products after each character of searchWord is typed.

__Example:__

Example 1:

Input: products = ["mobile","mouse","moneypot","monitor","mousepad"], searchWord = "mouse"
Output: [
["mobile","moneypot","monitor"],
["mobile","moneypot","monitor"],
["mouse","mousepad"],
["mouse","mousepad"],
["mouse","mousepad"]
]
Explanation: products sorted lexicographically = ["mobile","moneypot","monitor","mouse","mousepad"]
After typing m and mo all products match and we show user ["mobile","moneypot","monitor"]
After typing mou, mous and mouse the system suggests ["mouse","mousepad"]

Example 2:

Input: products = ["havana"], searchWord = "havana"
Output: [["havana"],["havana"],["havana"],["havana"],["havana"],["havana"]]

Example 3:

Input: products = ["bags","baggage","banner","box","cloths"], searchWord = "bags"
Output: [["baggage","bags","banner"],["baggage","bags","banner"],["baggage","bags"],["bags"]]

__Constraints:__

1 <= products.length <= 1000
1 <= products[i].length <= 3000
1 <= sum(products[i].length) <= 2 * 104
All the strings of products are unique.
products[i] consists of lowercase English letters.
1 <= searchWord.length <= 1000
searchWord consists of lowercase English letters.

__题目描述：__

给你一个产品数组 products 和一个字符串 searchWord ，products  数组中每个产品都是一个字符串。

请你设计一个推荐系统，在依次输入单词 searchWord 的每一个字母后，推荐 products 数组中前缀与 searchWord 相同的最多三个产品。如果前缀相同的可推荐产品超过三个，请按字典序返回最小的三个。

请你以二维列表的形式，返回在输入 searchWord 每个字母后相应的推荐产品的列表。

__示例：__

示例 1：

输入：products = ["mobile","mouse","moneypot","monitor","mousepad"], searchWord = "mouse"
输出：[
["mobile","moneypot","monitor"],
["mobile","moneypot","monitor"],
["mouse","mousepad"],
["mouse","mousepad"],
["mouse","mousepad"]
]
解释：按字典序排序后的产品列表是 ["mobile","moneypot","monitor","mouse","mousepad"]
输入 m 和 mo，由于所有产品的前缀都相同，所以系统返回字典序最小的三个产品 ["mobile","moneypot","monitor"]
输入 mou， mous 和 mouse 后系统都返回 ["mouse","mousepad"]

示例 2：

输入：products = ["havana"], searchWord = "havana"
输出：[["havana"],["havana"],["havana"],["havana"],["havana"],["havana"]]

示例 3：

输入：products = ["bags","baggage","banner","box","cloths"], searchWord = "bags"
输出：[["baggage","bags","banner"],["baggage","bags","banner"],["baggage","bags"],["bags"]]

示例 4：

输入：products = ["havana"], searchWord = "tatiana"
输出：[[],[],[],[],[],[],[]]

__提示：__

1 <= products.length <= 1000
1 <= Σ products[i].length <= 2 * 10^4
products[i] 中所有的字符都是小写英文字母。
1 <= searchWord.length <= 1000
searchWord 中所有字符都是小写英文字母。

__思路：__

1. 前缀树
将 products 中所有的字符串构建前缀数
每次查找从前缀树的根节点开始向下查找
找出字典序最小的三个字符串
可以使用堆(优先队列)存储已经遍历的字符串
时间复杂度为 O(mn + s), 空间复杂度为 O(mn), 其中 n 为 数组 products 的长度, m 为数组 products 中字符串的平均长度, s 为 searchWord 的长度
2. 排序 ➕ 二分查找
排序之后, products 都是按字典序, 所以选出最小的以 searchWord[:i] 为前缀的字符串即可
时间复杂度为 O((mn + s)lgn), 空间复杂度为 O(lgn), 其中 n 为 数组 products 的长度, m 为数组 products 中字符串的平均长度, s 为 searchWord 的长度

__代码__:

__C++__:

```C++
struct Trie 
{
    unordered_map<char, Trie*> child;
    priority_queue<string> words;
};

class Solution 
{
private:
    void add_word(Trie* root, const string& word) 
    {
        Trie* cur = root;
        for (const auto& c: word) 
        {
            if (!cur -> child.count(c)) cur -> child[c] = new Trie();
            cur = cur -> child[c];
            cur -> words.push(word);
            if (cur -> words.size() > 3) cur -> words.pop();
        }
    }
    
public:
    vector<vector<string>> suggestedProducts(vector<string>& products, string searchWord) 
    {
        Trie* root = new Trie();
        for (const auto& word: products) add_word(root, word);
        vector<vector<string>> result;
        Trie* cur = root;
        bool flag = false;
        for (const auto& c: searchWord) 
        {
            if (flag || !cur -> child.count(c)) 
            {
                result.emplace_back();
                flag = true;
            }
            else 
            {
                cur = cur -> child[c];
                vector<string> temp;
                while (!cur -> words.empty()) 
                {
                    temp.push_back(cur -> words.top());
                    cur -> words.pop();
                }
                reverse(temp.begin(), temp.end());
                result.push_back(move(temp));
            }
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public List<List<String>> suggestedProducts(String[] products, String searchWord) {
        List<List<String>> result = new ArrayList<>();
        Arrays.sort(products);
        for (int i = 1, n = searchWord.length(); i <= n; i++) {
            String substring = searchWord.substring(0, i);
            result.add(Arrays.stream(products).filter(s -> s.startsWith(substring)).limit(3).collect(Collectors.toList()));
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def suggestedProducts(self, products: List[str], searchWord: str) -> List[List[str]]:
        result, word, n = [], '', len(searchWord)
        products.sort()
        for i in range(n):
            word += searchWord[i]
            cur = []
            for product in products:
                if product[:i + 1] == word and len(cur) < 3:
                    cur.append(product)
            result.append(cur)
        return result
```
