# 721 Accounts Merge 账户合并

__Description__:
Given a list of accounts where each element accounts[i] is a list of strings, where the first element accounts[i][0] is a name, and the rest of the elements are emails representing emails of the account.

Now, we would like to merge these accounts. Two accounts definitely belong to the same person if there is some common email to both accounts. Note that even if two accounts have the same name, they may belong to different people as people could have the same name. A person can have any number of accounts initially, but all of their accounts definitely have the same name.

After merging the accounts, return the accounts in the following format: the first element of each account is the name, and the rest of the elements are emails in sorted order. The accounts themselves can be returned in any order.

__Example:__

Example 1:

Input: accounts = [["John","johnsmith@mail.com","john_newyork@mail.com"],["John","johnsmith@mail.com","john00@mail.com"],["Mary","mary@mail.com"],["John","johnnybravo@mail.com"]]
Output: [["John","john00@mail.com","john_newyork@mail.com","johnsmith@mail.com"],["Mary","mary@mail.com"],["John","johnnybravo@mail.com"]]
Explanation:
The first and third John's are the same person as they have the common email "johnsmith@mail.com".
The second John and Mary are different people as none of their email addresses are used by other accounts.
We could return these lists in any order, for example the answer [['Mary', 'mary@mail.com'], ['John', 'johnnybravo@mail.com'],
['John', 'john00@mail.com', 'john_newyork@mail.com', 'johnsmith@mail.com']] would still be accepted.

Example 2:

Input: accounts = [["Gabe","Gabe0@m.co","Gabe3@m.co","Gabe1@m.co"],["Kevin","Kevin3@m.co","Kevin5@m.co","Kevin0@m.co"],["Ethan","Ethan5@m.co","Ethan4@m.co","Ethan0@m.co"],["Hanzo","Hanzo3@m.co","Hanzo1@m.co","Hanzo0@m.co"],["Fern","Fern5@m.co","Fern1@m.co","Fern0@m.co"]]
Output: [["Ethan","Ethan0@m.co","Ethan4@m.co","Ethan5@m.co"],["Gabe","Gabe0@m.co","Gabe1@m.co","Gabe3@m.co"],["Hanzo","Hanzo0@m.co","Hanzo1@m.co","Hanzo3@m.co"],["Kevin","Kevin0@m.co","Kevin3@m.co","Kevin5@m.co"],["Fern","Fern0@m.co","Fern1@m.co","Fern5@m.co"]]

__Constraints:__

1 <= accounts.length <= 1000
2 <= accounts[i].length <= 10
1 <= accounts[i][j] <= 30
accounts[i][0] consists of English letters.
accounts[i][j] (for j > 0) is a valid email.

__题目描述__:
给定一个列表 accounts，每个元素 accounts[i] 是一个字符串列表，其中第一个元素 accounts[i][0] 是 名称 (name)，其余元素是 emails 表示该账户的邮箱地址。

现在，我们想合并这些账户。如果两个账户都有一些共同的邮箱地址，则两个账户必定属于同一个人。请注意，即使两个账户具有相同的名称，它们也可能属于不同的人，因为人们可能具有相同的名称。一个人最初可以拥有任意数量的账户，但其所有账户都具有相同的名称。

合并账户后，按以下格式返回账户：每个账户的第一个元素是名称，其余元素是按字符 ASCII 顺序排列的邮箱地址。账户本身可以以任意顺序返回。

__示例 :__

示例 1：

输入：
accounts = [["John", "johnsmith@mail.com", "john00@mail.com"], ["John", "johnnybravo@mail.com"], ["John", "johnsmith@mail.com", "john_newyork@mail.com"], ["Mary", "mary@mail.com"]]
输出：
[["John", 'john00@mail.com', 'john_newyork@mail.com', 'johnsmith@mail.com'],  ["John", "johnnybravo@mail.com"], ["Mary", "mary@mail.com"]]
解释：
第一个和第三个 John 是同一个人，因为他们有共同的邮箱地址 "johnsmith@mail.com"。
第二个 John 和 Mary 是不同的人，因为他们的邮箱地址没有被其他帐户使用。
可以以任何顺序返回这些列表，例如答案 [['Mary'，'mary@mail.com']，['John'，'johnnybravo@mail.com']，
['John'，'john00@mail.com'，'john_newyork@mail.com'，'johnsmith@mail.com']] 也是正确的。

__提示:__

accounts的长度将在[1，1000]的范围内。
accounts[i]的长度将在[1，10]的范围内。
accounts[i][j]的长度将在[1，30]的范围内。

__思路__:

并查集
将 name 在 accounts 中的下标(id)看作 parent
用一个哈希表记录电子邮件对应的 id, 如果 id 已经存在说明可以加入并查集中
将 parent 下的电子邮件加入一个哈希表对应的 set(有序) 中
将后一个的哈希表中的邮件按 name 加入到结果中
时间复杂度为 O(nlgn), 空间复杂度为 O(n)

__代码__:
__C++__:

```C++
class UF 
{
public:
    UF(int n)
    {
        parent = vector<int>(n);
        for (int i = 0; i < n; i++) parent[i] = i;
    }
    
    void connect(int p, int q) 
    {
        p = find(p);
        q = find(q);
        parent[p] = parent[q] = min(p, q);
    }
    
    int find(int p) 
    {
        while (parent[p] != p) 
        {
            parent[p] = parent[parent[p]];
            p = parent[p];
        }
        return p;
    }
private:
    vector<int> parent;
};

class Solution 
{
public:
    vector<vector<string>> accountsMerge(vector<vector<string>>& accounts) 
    {
        vector<vector<string>> result;
        int n = accounts.size();
        UF *uf = new UF(n);
        unordered_map<string, int> mail_id;
        unordered_map<int, set<string>> id_user;
        for (int i = 0; i < n; i++) 
        {
            for (int j = 1; j < accounts[i].size(); j++) 
            {
                string mail = accounts[i][j];
                if (mail_id.count(mail)) uf -> connect(i, mail_id[mail]);
                else mail_id[mail] = i;
            }
        }
        for (auto& [k, v] : mail_id) id_user[uf -> find(v)].emplace(k);
        for (auto& [k, v] : id_user) 
        {
            vector<string> cur;
            cur.emplace_back(accounts[k].front());
            for (auto mail : v) cur.emplace_back(mail);
            result.emplace_back(cur);
        }
        return result;
    }
};
```

__Java__:

```Java
class UF {
    private int parent[];
    
    public UF(int n) {
        parent = new int[n];
        for (int i = 0; i < n; i++) parent[i] = i; 
    }
    
    public void connect(int p, int q) {
        p = find(p);
        q = find(q);
        parent[p] = parent[q] = Math.min(p, q);
    }
    
    public int find(int p) {
        while (parent[p] != p) {
            parent[p] = parent[parent[p]];
            p = parent[p];
        }
        return p;
    }
}

class Solution {
    public List<List<String>> accountsMerge(List<List<String>> accounts) {
        int n = accounts.size();
        UF uf = new UF(n);
        Map<String, Integer> mailId = new HashMap<>();
        Map<Integer, Set<String>> idUser = new HashMap<>();
        for (int i = 0; i < n; i++) {
            for (int j = 1; j < accounts.get(i).size(); j++) {
                String mail = accounts.get(i).get(j);
                if (mailId.containsKey(mail)) uf.connect(i, mailId.get(mail));
                else mailId.put(mail, i);
            }
        }
        for (Map.Entry<String, Integer> entry : mailId.entrySet()) {
            int i = uf.find(entry.getValue());
            if (!idUser.containsKey(i)) idUser.put(i, new HashSet<String>());
            idUser.get(i).add(entry.getKey());
        }
        List<List<String>> result = new ArrayList<>();
        for (Map.Entry<Integer, Set<String>> entry : idUser.entrySet()) {
            List<String> cur = new ArrayList<>();
            for (String mail : entry.getValue()) cur.add(mail);
            Collections.sort(cur);
            cur.add(0, accounts.get(entry.getKey()).get(0));
            result.add(cur);
        }
        return result;
    }
}
```

__Python__:

```Python
class UF:
    def __init__(self, n: int) -> None:
        self.parent = list(range(n))


    def connect(self, p: int, q: int) -> None:
        self.parent[p] = self.parent[q] = min((p := self.find(p)), (q := self.find(q)))


    def find(self, p: int) -> int:
        while self.parent[p] < p:
            self.parent[p] = self.parent[self.parent[p]]
            p = self.parent[p]
        return p


class Solution:
    def accountsMerge(self, accounts: List[List[str]]) -> List[List[str]]:
        uf, mail_id, id_user = UF((n := len(accounts))), {}, {}
        for i in range(n):
            for mail in accounts[i][1:]:
                if mail not in mail_id:
                    mail_id[mail] = i
                else:
                    uf.connect(i, mail_id[mail])
        for mail, i in mail_id.items():
            i = uf.find(i)
            if i not in id_user:
                id_user[i] = [accounts[i][0], set()]
            id_user[i][1].add(mail)
        result = []
        for name, emails in id_user.values():
            result.append([name] + list(sorted(list(emails))))
        return result
```
