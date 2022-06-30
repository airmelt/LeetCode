# 1169 Invalid Transactions 查询无效交易

__Description__:
A transaction is possibly invalid if:

the amount exceeds $1000, or;
if it occurs within (and including) 60 minutes of another transaction with the same name in a different city.
You are given an array of strings transaction where transactions[i] consists of comma-separated values representing the name, time (in minutes), amount, and city of the transaction.

Return a list of transactions that are possibly invalid. You may return the answer in any order.

__Example:__

Example 1:

Input: transactions = ["alice,20,800,mtv","alice,50,100,beijing"]
Output: ["alice,20,800,mtv","alice,50,100,beijing"]
Explanation: The first transaction is invalid because the second transaction occurs within a difference of 60 minutes, have the same name and is in a different city. Similarly the second one is invalid too.

Example 2:

Input: transactions = ["alice,20,800,mtv","alice,50,1200,mtv"]
Output: ["alice,50,1200,mtv"]

Example 3:

Input: transactions = ["alice,20,800,mtv","bob,50,1200,mtv"]
Output: ["bob,50,1200,mtv"]

__Constraints:__
transactions.length <= 1000
Each transactions[i] takes the form "{name},{time},{amount},{city}"
Each {name} and {city} consist of lowercase English letters, and have lengths between 1 and 10.
Each {time} consist of digits, and represent an integer between 0 and 1000.
Each {amount} consist of digits, and represent an integer between 0 and 2000.

__题目描述__:
如果出现下述两种情况，交易 可能无效：

交易金额超过 $1000
或者，它和 另一个城市 中 同名 的另一笔交易相隔不超过 60 分钟（包含 60 分钟整）
给定字符串数组交易清单 transaction 。每个交易字符串 transactions[i] 由一些用逗号分隔的值组成，这些值分别表示交易的名称，时间（以分钟计），金额以及城市。

返回 transactions，返回可能无效的交易列表。你可以按 任何顺序 返回答案。

__示例 :__

示例 1：

输入：transactions = ["alice,20,800,mtv","alice,50,100,beijing"]
输出：["alice,20,800,mtv","alice,50,100,beijing"]
解释：第一笔交易是无效的，因为第二笔交易和它间隔不超过 60 分钟、名称相同且发生在不同的城市。同样，第二笔交易也是无效的。

示例 2：

输入：transactions = ["alice,20,800,mtv","alice,50,1200,mtv"]
输出：["alice,50,1200,mtv"]

示例 3：

输入：transactions = ["alice,20,800,mtv","bob,50,1200,mtv"]
输出：["bob,50,1200,mtv"]

__提示:__

transactions.length <= 1000
每笔交易 transactions[i] 按 "{name},{time},{amount},{city}" 的格式进行记录
每个交易名称 {name} 和城市 {city} 都由小写英文字母组成，长度在 1 到 10 之间
每个交易时间 {time} 由一些数字组成，表示一个 0 到 1000 之间的整数
每笔交易金额 {amount} 由一些数字组成，表示一个 0 到 2000 之间的整数

__思路__:

模拟
先按照逗号将字符串分割为子串
可以用一个类或者结构体存储
先将交易大于 1000 的筛选出来
然后按照名字将交易存放在哈希表中进行筛选
或者使用排序和滑动窗口筛选
时间复杂度为 O(n ^ 2), 空间复杂度为 O(n)

__代码__:
__C++__:

```C++
struct Transaction
{
    int id;
    int time;
    int amount;
    string city;
    string origin;

    Transaction(int id, int time, int amount, string& city, string& origin) : id(id), time(time), amount(amount), city(city), origin(origin)
    {}
};

class Solution 
{
private:
    vector<string> split(const string& str)
    {
        vector<string> result;
        stringstream ss(str);
        string cur;
        while(std::getline(ss, cur, ',')) result.push_back(cur);
        return result;
    }
public:
    vector<string> invalidTransactions(vector<string>& transactions) {
        unordered_map<string, vector<Transaction>> m;
        vector<string> result;
        int n = transactions.size();
        vector<bool> flag(n);
        for (int i = 0; i < n; ++i)
        {
            vector<string> strs = split(transactions[i]);
            string name = strs[0];
            int time = stoi(strs[1]);
            int amount = stoi(strs[2]);
            string city = strs[3];
            if (amount > 1000)
            {
                flag[i] = true;
                result.push_back(transactions[i]);
            }
            for (Transaction& t : m[name])
            {
                if (abs(time - t.time) <= 60 and city != t.city)
                {
                    if (!flag[t.id])
                    {
                        flag[t.id] = true;
                        result.push_back(t.origin);
                    }
                    if (!flag[i])
                    {
                        flag[i] = true;
                        result.push_back(transactions[i]);
                    }
                }
            }
            m[name].emplace_back(Transaction(i, time, amount, city, transactions[i]));
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public List<String> invalidTransactions(String[] transactions) {
        List<String> result = new ArrayList<>();
        boolean[] flag = new boolean[1001];
        Map<String, List<Transaction>> map = new HashMap<>();
        int id = 0;
        for (String transaction : transactions) {
            String[] items = transaction.split(",");
            String name = items[0];
            int time = Integer.parseInt(items[1]);
            int money = Integer.parseInt(items[2]);
            String address = items[3];
            if (money > 1000) {
                flag[id] = true;
                result.add(transaction);
            }
            List<Transaction> set = map.getOrDefault(name, new ArrayList<>());
            for (Transaction t : set) {
                if (Math.abs(t.time - time) <= 60 && !t.address.equals(address)) {
                    if (!flag[t.id]) {
                        flag[t.id] = true;
                        result.add(t.origin);
                    }
                    if (!flag[id]) {
                        result.add(transaction);
                        flag[id] = true;
                    }
                }
            }
            set.add(new Transaction(id, time, money, address, transaction));
            map.put(name, set);
            id++;
        }
        return result;
    }
    
    static class Transaction {
        int time;
        int money;
        String address;
        String origin;
        int id;

        Transaction(int id, int time, int money, String address, String origin) {
            this.id = id;
            this.time = time;
            this.money = money;
            this.address = address;
            this.origin = origin;
        }
    }
}
```

__Python__:

```Python
class Solution:
    def invalidTransactions(self, transactions: List[str]) -> List[str]:
        n, result, transactions = len(transactions), [], [t.split(',') for t in transactions]
        transactions.sort(key=lambda x: int(x[1]))
        flag, queue = [int(transactions[i][2]) > 1000 for i in range(n)], []
        for i, t in enumerate(transactions):
            t += [i]
            while queue and int(t[1]) - int(queue[0][1]) > 60:
                queue.pop(0)
            queue.append(t)
            for v in queue:
                if v[0] == t[0] and v[3] != t[3]:
                    flag[v[4]] = flag[t[4]] = True
        return [','.join(transactions[i][:-1]) for i in range(n) if flag[i]]
```
