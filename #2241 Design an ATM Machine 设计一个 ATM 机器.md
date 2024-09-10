# 2241 Design an ATM Machine 设计一个 ATM 机器

__Description:__

There is an ATM machine that stores banknotes of `5` denominations: `20`, `50`, `100`, `200`, and `500` dollars. Initially the ATM is empty. The user can use the machine to deposit or withdraw any amount of money.

When withdrawing, the machine prioritizes using banknotes of larger values.

- For example, if you want to withdraw `$300` and there are `2` `$50` banknotes, `1` `$100` banknote, and `1` `$200` banknote, then the machine will use the `$100` and `$200` banknotes.
- However, if you try to withdraw `$600` and there are `3` `$200` banknotes and `1` `$500` banknote, then the withdraw request will be rejected because the machine will first try to use the `$500` banknote and then be unable to use banknotes to complete the remaining `$100`. Note that the machine is __not__ allowed to use the `$200` banknotes instead of the `$500` banknote.

Implement the ATM class:

- `ATM()` Initializes the ATM object.
- `void deposit(int[] banknotesCount)` Deposits new banknotes in the order `$20`, `$50`, `$100`, `$200`, and `$500`.
- `int[] withdraw(int amount)` Returns an array of length `5` of the number of banknotes that will be handed to the user in the order `$20`, `$50`, `$100`, `$200`, and `$500`, and update the number of banknotes in the ATM after withdrawing. Returns `[-1]` if it is not possible (do __not__ withdraw any banknotes in this case).

__Example:__

Example 1:

```text
Input
["ATM", "deposit", "withdraw", "deposit", "withdraw", "withdraw"]
[[], [[0,0,1,2,1]], [600], [[0,1,0,1,1]], [600], [550]]
Output
[null, null, [0,0,1,0,1], null, [-1], [0,1,0,0,1]]

Explanation
```

```Java
ATM atm = new ATM();
atm.deposit([0,0,1,2,1]); // Deposits 1 $100 banknote, 2 $200 banknotes,
                          // and 1 $500 banknote.
atm.withdraw(600);        // Returns [0,0,1,0,1]. The machine uses 1 $100 banknote
                          // and 1 $500 banknote. The banknotes left over in the
                          // machine are [0,0,0,2,0].
atm.deposit([0,1,0,1,1]); // Deposits 1 $50, $200, and $500 banknote.
                          // The banknotes in the machine are now [0,1,0,3,1].
atm.withdraw(600);        // Returns [-1]. The machine will try to use a $500 banknote
                          // and then be unable to complete the remaining $100,
                          // so the withdraw request will be rejected.
                          // Since the request is rejected, the number of banknotes
                          // in the machine is not modified.
atm.withdraw(550);        // Returns [0,1,0,0,1]. The machine uses 1 $50 banknote
                          // and 1 $500 banknote.
```

__Constraints:__

- `banknotesCount.length == 5`
- `0 <= banknotesCount[i] <= 10 ^ 9`
- `1 <= amount <= 10 ^ 9`
- At most `5000` calls __in total__ will be made to `withdraw` and `deposit`.
- At least __one__ call will be made to each function `withdraw` and `deposit`.
- Sum of `banknotesCount[i]` in all deposits doesn't exceed `10 ^ 9`

__题目描述:__

一个 ATM 机器，存有 `5` 种面值的钞票: `20` ， `50` ， `100` ， `200` 和 `500` 美元。初始时，ATM 机是空的。用户可以用它存或者取任意数目的钱。

取款时，机器会优先取 较大 数额的钱。

- 比方说，你想取 `$300` ，并且机器里有 `2` 张 `$50` 的钞票， `1` 张 `$100` 的钞票和 `1` 张 `$200` 的钞票，那么机器会取出 `$100` 和 `$200` 的钞票。
- 但是，如果你想取 `$600` ，机器里有 `3` 张 `$200` 的钞票和 `1` 张 `$500` 的钞票，那么取款请求会被拒绝，因为机器会先取出 `$500` 的钞票，然后无法取出剩余的 `$100` 。注意，因为有 `$500` 钞票的存在，机器 __不能__ 取 `$200` 的钞票。

请你实现 ATM 类：

- `ATM()` 初始化 ATM 对象。
- `void deposit(int[] banknotesCount)` 分别存入 `$20` ， `$50`， `$100`， `$200` 和 `$500` 钞票的数目。
- `int[] withdraw(int amount)` 返回一个长度为 `5` 的数组，分别表示 `$20` ， `$50`， `$100` ， `$200` 和 `$500` 钞票的数目，并且更新 ATM 机里取款后钞票的剩余数量。如果无法取出指定数额的钱，请返回 `[-1]` （这种情况下 __不__ 取出任何钞票）。

__示例:__

示例 1：

```text
输入：
["ATM", "deposit", "withdraw", "deposit", "withdraw", "withdraw"]
[[], [[0,0,1,2,1]], [600], [[0,1,0,1,1]], [600], [550]]
输出：
[null, null, [0,0,1,0,1], null, [-1], [0,1,0,0,1]]

解释：
```

```Java
ATM atm = new ATM();
atm.deposit([0,0,1,2,1]); // 存入 1 张 $100 ，2 张 $200 和 1 张 $500 的钞票。
atm.withdraw(600);        // 返回 [0,0,1,0,1] 。机器返回 1 张 $100 和 1 张 $500 的钞票。机器里剩余钞票的数量为 [0,0,0,2,0] 。
atm.deposit([0,1,0,1,1]); // 存入 1 张 $50 ，1 张 $200 和 1 张 $500 的钞票。
                          // 机器中剩余钞票数量为 [0,1,0,3,1] 。
atm.withdraw(600);        // 返回 [-1] 。机器会尝试取出 $500 的钞票，然后无法得到剩余的 $100 ，所以取款请求会被拒绝。
                          // 由于请求被拒绝，机器中钞票的数量不会发生改变。
atm.withdraw(550);        // 返回 [0,1,0,0,1] ，机器会返回 1 张 $50 的钞票和 1 张 $500 的钞票。
```

__提示：__

- `banknotesCount.length == 5`
- `0 <= banknotesCount[i] <= 10 ^ 9`
- `1 <= amount <= 10 ^ 9`
- __总共__ 最多有 `5000` 次 `withdraw` 和 `deposit` 的调用。
- 函数 `withdraw` 和 `deposit` 至少各有 __一次__ 调用。

__思路:__

```text
模拟
用两个数组或者哈希表记录存入的钞票数量和面值
存款时直接累加面值对应的数量
取款时优先取较大面值的钞票
如果取完后剩余钞票数不为 amount, 返回 [-1]
否则减去取出的钞票数量
并返回取出的钞票数量
时间复杂度为 O(N), 空间复杂度为 O(1), 其中 N 为存款次数 + 取款次数, 只有 5 种钞票空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class ATM 
{
public:
    ATM() 
    {
        cash = vector<int>(5);
        value = {20, 50, 100, 200, 500};
    }
    
    void deposit(vector<int> banknotesCount) 
    {
        for (int i = 0; i < 5; i++) cash[i] += banknotesCount[i];
    }
    
    vector<int> withdraw(int amount) 
    {
        vector<int> result(5);
        for (int i = 4; i > -1; i--) 
        {
            result[i] += min(cash[i], amount / value[i]);
            amount -= value[i] * result[i];
        }
        if (amount != 0) return {-1};
        for (int i = 0; i < 5; i++) cash[i] -= result[i];
        return result;
    }
private:
    vector<int> cash;
    vector<int> value;
};

/**
 * Your ATM object will be instantiated and called as such:
 * ATM* obj = new ATM();
 * obj->deposit(banknotesCount);
 * vector<int> param_2 = obj->withdraw(amount);
 */
```

__Java__:

```Java
class ATM {
    private int[] cash = new int[5];
    private int[] value = new int[]{20, 50, 100, 200, 500};

    public ATM() {
        
    }
    
    public void deposit(int[] banknotesCount) {
        for (int i = 0; i < 5; i++) cash[i] += banknotesCount[i];
    }
    
    public int[] withdraw(int amount) {
        int[] result = new int[5];
        for (int i = 4; i > -1; i--) {
            result[i] += Math.min(cash[i], amount / value[i]);
            amount -= value[i] * result[i];
        }
        if (amount != 0) return new int[]{-1};
        for (int i = 0; i < 5; i++) cash[i] -= result[i];
        return result;
    }
}

/**
 * Your ATM object will be instantiated and called as such:
 * ATM obj = new ATM();
 * obj.deposit(banknotesCount);
 * int[] param_2 = obj.withdraw(amount);
 */
```

__Python__:

```Python
class ATM:

    def __init__(self):
        self.cash = [0] * 5
        self.value = [20, 50, 100, 200, 500]


    def deposit(self, banknotesCount: List[int]) -> None:
        for i in range(5):
            self.cash[i] += banknotesCount[i]


    def withdraw(self, amount: int) -> List[int]:
        result = [0] * 5
        for i in range(4, -1, -1):
            result[i] = min(self.cash[i], amount // self.value[i])
            amount -= result[i] * self.value[i]
        if amount:
            return [-1]
        for i in range(5):
            self.cash[i] -= result[i]
        return result



# Your ATM object will be instantiated and called as such:
# obj = ATM()
# obj.deposit(banknotesCount)
# param_2 = obj.withdraw(amount)
```
