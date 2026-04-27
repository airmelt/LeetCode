# 2043 Simple Bank System 简易银行系统

__Description:__

You have been tasked with writing a program for a popular bank that will automate all its incoming transactions (transfer, deposit, and withdraw). The bank has `n` accounts numbered from `1` to `n`. The initial balance of each account is stored in a __0-indexed__ integer array `balance`, with the `(i + 1) ^ th` account having an initial balance of `balance[i]`.

Execute all the valid transactions. A transaction is valid if:

- The given account number(s) are between `1` and `n`, and
- The amount of money withdrawn or transferred from is __less than or equal__ to the balance of the account.

Implement the `Bank` class:

- `Bank(long[] balance)` Initializes the object with the __0-indexed__ integer array `balance`.
- `boolean transfer(int account1, int account2, long money)` Transfers `money` dollars from the account numbered `account1` to the account numbered `account2`. Return `true` if the transaction was successful, `false` otherwise.
- `boolean deposit(int account, long money)` Deposit `money` dollars into the account numbered `account`. Return `true` if the transaction was successful, `false` otherwise.
- `boolean withdraw(int account, long money)` Withdraw `money` dollars from the account numbered `account`. Return `true` if the transaction was successful, `false` otherwise.

__Example:__

Example 1:

```text
Input
["Bank", "withdraw", "transfer", "deposit", "transfer", "withdraw"]
[[[10, 100, 20, 50, 30]], [3, 10], [5, 1, 20], [5, 20], [3, 4, 15], [10, 50]]
Output
[null, true, true, true, false, false]

Explanation
```

```Java
Bank bank = new Bank([10, 100, 20, 50, 30]);
bank.withdraw(3, 10);    // return true, account 3 has a balance of $20, so it is valid to withdraw $10.
                         // Account 3 has $20 - $10 = $10.
bank.transfer(5, 1, 20); // return true, account 5 has a balance of $30, so it is valid to transfer $20.
                         // Account 5 has $30 - $20 = $10, and account 1 has $10 + $20 = $30.
bank.deposit(5, 20);     // return true, it is valid to deposit $20 to account 5.
                         // Account 5 has $10 + $20 = $30.
bank.transfer(3, 4, 15); // return false, the current balance of account 3 is $10,
                         // so it is invalid to transfer $15 from it.
bank.withdraw(10, 50);   // return false, it is invalid because account 10 does not exist.
```

__Constraints:__

- `n == balance.length`
- `1 <= n, account, account1, account2 <= 10 ^ 5`
- `0 <= balance[i], money <= 10 ^ 12`
- At most `10 ^ 4` calls will be made to __each__ function `transfer`, `deposit`, `withdraw`.

__题目描述:__

你的任务是为一个很受欢迎的银行设计一款程序，以自动化执行所有传入的交易（转账，存款和取款）。银行共有 `n` 个账户，编号从 `1` 到 `n` 。每个账号的初始余额存储在一个下标从 __0__ 开始的整数数组 `balance` 中，其中第 `(i + 1)` 个账户的初始余额是 `balance[i]` 。

请你执行所有 有效的 交易。如果满足下面全部条件，则交易 有效 ：

- 指定的账户数量在 `1` 和 `n` 之间，且
- 取款或者转账需要的钱的总数 __小于或者等于__ 账户余额。

实现 `Bank` 类:

- `Bank(long[] balance)` 使用下标从 __0__ 开始的整数数组 `balance` 初始化该对象。
- `boolean transfer(int account1, int account2, long money)` 从编号为 `account1` 的账户向编号为 `account2` 的账户转帐 `money` 美元。如果交易成功，返回 `true` ，否则，返回 `false` 。
- `boolean deposit(int account, long money)` 向编号为 `account` 的账户存款 `money` 美元。如果交易成功，返回 `true` ；否则，返回 `false` 。
- `boolean withdraw(int account, long money)` 从编号为 `account` 的账户取款 `money` 美元。如果交易成功，返回 `true` ；否则，返回 `false` 。

__示例:__

示例：

```text
输入：
["Bank", "withdraw", "transfer", "deposit", "transfer", "withdraw"]
[[[10, 100, 20, 50, 30]], [3, 10], [5, 1, 20], [5, 20], [3, 4, 15], [10, 50]]
输出：
[null, true, true, true, false, false]

解释：
```

```Java
Bank bank = new Bank([10, 100, 20, 50, 30]);
bank.withdraw(3, 10);    // 返回 true ，账户 3 的余额是 $20 ，所以可以取款 $10 。
                         // 账户 3 余额为 $20 - $10 = $10 。
bank.transfer(5, 1, 20); // 返回 true ，账户 5 的余额是 $30 ，所以可以转账 $20 。
                         // 账户 5 的余额为 $30 - $20 = $10 ，账户 1 的余额为 $10 + $20 = $30 。
bank.deposit(5, 20);     // 返回 true ，可以向账户 5 存款 $20 。
                         // 账户 5 的余额为 $10 + $20 = $30 。
bank.transfer(3, 4, 15); // 返回 false ，账户 3 的当前余额是 $10 。
                         // 所以无法转账 $15 。
bank.withdraw(10, 50);   // 返回 false ，交易无效，因为账户 10 并不存在。
```

__提示：__

- `n == balance.length`
- `1 <= n, account, account1, account2 <= 10 ^ 5`
- `0 <= balance[i], money <= 10 ^ 12`
- `transfer`, `deposit`, `withdraw` 三个函数，__每个__ 最多调用 `10 ^ 4` 次

__思路:__

```text
模拟
记录余额数组和账户数量
转账和取钱的时候需要判断余额是否足够
存钱的时候直接加上即可
时间复杂度为 O(1), 空间复杂度为 O(1), 仅初始化需要 O(N)
```

__代码:__

__C++__:

```C++
class Bank 
{
public:
    Bank(vector<long long>& balance) 
    {
        this -> balance = balance;
        n = balance.size();
    }
    
    bool transfer(int account1, int account2, long long money) 
    {
        if (account1 > n or account2 > n or balance[account1 - 1] < money) return false;
        balance[account1 - 1] -= money;
        balance[account2 - 1] += money;
        return true;
    }
    
    bool deposit(int account, long long money) 
    {
        if (account > n) return false;
        balance[account - 1] += money;
        return true;
    }
    
    bool withdraw(int account, long long money) 
    {
        if (account > n or balance[account - 1] < money) return false;
        balance[account - 1] -= money;
        return true;
    }
private:
    vector<long long> balance;
    int n;
};

/**
 * Your Bank object will be instantiated and called as such:
 * Bank* obj = new Bank(balance);
 * bool param_1 = obj->transfer(account1,account2,money);
 * bool param_2 = obj->deposit(account,money);
 * bool param_3 = obj->withdraw(account,money);
 */
```

__Java__:

```Java
class Bank {
    private long[] balance;
    private int n;

    public Bank(long[] balance) {
        this.balance = balance;
        this.n = balance.length;
    }
    
    public boolean transfer(int account1, int account2, long money) {
        if (account1 > n || account2 > n || balance[account1 - 1] < money) return false;
        balance[account1 - 1] -= money;
        balance[account2 - 1] += money;
        return true;
    }
    
    public boolean deposit(int account, long money) {
        if (account > n) return false;
        balance[account - 1] += money;
        return true;
    }
    
    public boolean withdraw(int account, long money) {
        if (account > n || balance[account - 1] < money) return false;
        balance[account - 1] -= money;
        return true;
    }
}

/**
 * Your Bank object will be instantiated and called as such:
 * Bank obj = new Bank(balance);
 * boolean param_1 = obj.transfer(account1,account2,money);
 * boolean param_2 = obj.deposit(account,money);
 * boolean param_3 = obj.withdraw(account,money);
 */
```

__Python__:

```Python
class Bank:
    def __init__(self, balance: List[int]):
        self.balance = balance
        self.n = len(balance)

    def transfer(self, account1: int, account2: int, money: int) -> bool:
        if account1 > self.n or account2 > self.n or self.balance[account1 - 1] < money:
            return False
        self.balance[account1 - 1] -= money
        self.balance[account2 - 1] += money
        return True

    def deposit(self, account: int, money: int) -> bool:
        if account > self.n:
            return False
        self.balance[account - 1] += money
        return True

    def withdraw(self, account: int, money: int) -> bool:
        if account > self.n or self.balance[account - 1] < money:
            return False
        self.balance[account - 1] -= money
        return True


# Your Bank object will be instantiated and called as such:
# obj = Bank(balance)
# param_1 = obj.transfer(account1,account2,money)
# param_2 = obj.deposit(account,money)
# param_3 = obj.withdraw(account,money)
```
