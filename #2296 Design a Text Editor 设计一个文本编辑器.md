# 2296 Design a Text Editor 设计一个文本编辑器

__Description:__

Design a text editor with a cursor that can do the following:

- __Add__ text to where the cursor is.
- __Delete__ text from where the cursor is (simulating the backspace key).
- __Move__ the cursor either left or right.

When deleting text, only characters to the left of the cursor will be deleted. The cursor will also remain within the actual text and cannot be moved beyond it. More formally, we have that `0 <= cursor.position <= currentText.length` always holds.

Implement the `TextEditor` class:

- `TextEditor()` Initializes the object with empty text.
- `void addText(string text)` Appends `text` to where the cursor is. The cursor ends to the right of `text`.
- `int deleteText(int k)` Deletes `k` characters to the left of the cursor. Returns the number of characters actually deleted.
- `string cursorLeft(int k)` Moves the cursor to the left `k` times. Returns the last `min(10, len)` characters to the left of the cursor, where `len` is the number of characters to the left of the cursor.
- `string cursorRight(int k)` Moves the cursor to the right `k` times. Returns the last `min(10, len)` characters to the left of the cursor, where `len` is the number of characters to the left of the cursor.

__Example:__

Example 1:

```text
Input
["TextEditor", "addText", "deleteText", "addText", "cursorRight", "cursorLeft", "deleteText", "cursorLeft", "cursorRight"]
[[], ["leetcode"], [4], ["practice"], [3], [8], [10], [2], [6]]
Output
[null, null, 4, null, "etpractice", "leet", 4, "", "practi"]

Explanation
```

```Java
TextEditor textEditor = new TextEditor(); // The current text is "|". (The '|' character represents the cursor)
textEditor.addText("leetcode"); // The current text is "leetcode|".
textEditor.deleteText(4); // return 4
                          // The current text is "leet|". 
                          // 4 characters were deleted.
textEditor.addText("practice"); // The current text is "leetpractice|". 
textEditor.cursorRight(3); // return "etpractice"
                           // The current text is "leetpractice|". 
                           // The cursor cannot be moved beyond the actual text and thus did not move.
                           // "etpractice" is the last 10 characters to the left of the cursor.
textEditor.cursorLeft(8); // return "leet"
                          // The current text is "leet|practice".
                          // "leet" is the last min(10, 4) = 4 characters to the left of the cursor.
textEditor.deleteText(10); // return 4
                           // The current text is "|practice".
                           // Only 4 characters were deleted.
textEditor.cursorLeft(2); // return ""
                          // The current text is "|practice".
                          // The cursor cannot be moved beyond the actual text and thus did not move. 
                          // "" is the last min(10, 0) = 0 characters to the left of the cursor.
textEditor.cursorRight(6); // return "practi"
                           // The current text is "practi|ce".
                           // "practi" is the last min(10, 6) = 6 characters to the left of the cursor.
```

__Constraints:__

- `1 <= text.length, k <= 40`
- `text` consists of lowercase English letters.
- At most `2 * 10 ^ 4` calls __in total__ will be made to `addText`, `deleteText`, `cursorLeft` and `cursorRight`.

__Follow-up:__ Could you find a solution with time complexity of `O(k)` per call?

__题目描述:__

请你设计一个带光标的文本编辑器，它可以实现以下功能：

- __添加:__在光标所在处添加文本。
- __删除:__在光标所在处删除文本（模拟键盘的删除键）。
- __移动:__将光标往左或者往右移动。

当删除文本时，只有光标左边的字符会被删除。光标会留在文本内，也就是说任意时候 `0 <= cursor.position <= currentText.length` 都成立。

请你实现 `TextEditor` 类:

- `TextEditor()` 用空文本初始化对象。
- `void addText(string text)` 将 `text` 添加到光标所在位置。添加完后光标在 `text` 的右边。
- `int deleteText(int k)` 删除光标左边 `k` 个字符。返回实际删除的字符数目。
- `string cursorLeft(int k)` 将光标向左移动 `k` 次。返回移动后光标左边 `min(10, len)` 个字符，其中 `len` 是光标左边的字符数目。
- `string cursorRight(int k)` 将光标向右移动 `k` 次。返回移动后光标左边 `min(10, len)` 个字符，其中 `len` 是光标左边的字符数目。

__示例:__

示例 1：

```text
输入：
["TextEditor", "addText", "deleteText", "addText", "cursorRight", "cursorLeft", "deleteText", "cursorLeft", "cursorRight"]
[[], ["leetcode"], [4], ["practice"], [3], [8], [10], [2], [6]]
输出：
[null, null, 4, null, "etpractice", "leet", 4, "", "practi"]

解释：
```

```Java
TextEditor textEditor = new TextEditor(); // 当前 text 为 "|" 。（'|' 字符表示光标）
textEditor.addText("leetcode"); // 当前文本为 "leetcode|" 。
textEditor.deleteText(4); // 返回 4
                          // 当前文本为 "leet|" 。
                          // 删除了 4 个字符。
textEditor.addText("practice"); // 当前文本为 "leetpractice|" 。
textEditor.cursorRight(3); // 返回 "etpractice"
                           // 当前文本为 "leetpractice|". 
                           // 光标无法移动到文本以外，所以无法移动。
                           // "etpractice" 是光标左边的 10 个字符。
textEditor.cursorLeft(8); // 返回 "leet"
                          // 当前文本为 "leet|practice" 。
                          // "leet" 是光标左边的 min(10, 4) = 4 个字符。
textEditor.deleteText(10); // 返回 4
                           // 当前文本为 "|practice" 。
                           // 只有 4 个字符被删除了。
textEditor.cursorLeft(2); // 返回 ""
                          // 当前文本为 "|practice" 。
                          // 光标无法移动到文本以外，所以无法移动。
                          // "" 是光标左边的 min(10, 0) = 0 个字符。
textEditor.cursorRight(6); // 返回 "practi"
                           // 当前文本为 "practi|ce" 。
                           // "practi" 是光标左边的 min(10, 6) = 6 个字符。
```

__提示：__

- `1 <= text.length, k <= 40`
- `text` 只含有小写英文字母。
- 调用 `addText` ， `deleteText` ， `cursorLeft` 和 `cursorRight` 的 __总__ 次数不超过 `2 * 10 ^ 4` 次。

__进阶:__你能设计并实现一个每次调用时间复杂度为 `O(k)` 的解决方案吗？

__思路:__

```text
对顶堆
使用对顶堆的思想
用两个字符串 left 和 right 分别存储光标左边和右边的字符
添加文本时，将文本添加到 left 的末尾
删除文本时，删除 left 的末尾 k 个字符, 不足 k 个则删除全部, 返回实际删除的字符数目
光标左移时，将 left 的末尾字符添加到 right 的末尾, 直到 left 为空或者移动 k 次
光标右移时，将 right 的末尾字符添加到 left 的末尾, 直到 right 为空或者移动 k 次
并使用一个辅助函数 text() 返回 left 的最后 10 个字符, 不足 10 个则返回全部
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class TextEditor 
{
public:
    TextEditor() {}
    
    void addText(string text) 
    {
        left += text;
    }
    
    int deleteText(int k) 
    {
        left.resize(left.length() - (k = min(k, (int)left.length())));
        return k;
    }
    
    string cursorLeft(int k) 
    {
        for (; k and !left.empty(); k--) 
        {
            right += left.back();
            left.pop_back();
        }
        return text();
    }
    
    string cursorRight(int k) 
    {
        for (; k and !right.empty(); k--) 
        {
            left += right.back();
            right.pop_back();
        }
        return text();
    }
private:
    string left, right;

    string text() 
    {
        return left.substr(max((int)left.size() - 10, 0));
    }
};

/**
 * Your TextEditor object will be instantiated and called as such:
 * TextEditor* obj = new TextEditor();
 * obj->addText(text);
 * int param_2 = obj->deleteText(k);
 * string param_3 = obj->cursorLeft(k);
 * string param_4 = obj->cursorRight(k);
 */
```

__Java__:

```Java
class TextEditor {
    private StringBuilder left = new StringBuilder(), right = new StringBuilder();

    public TextEditor() {}
    
    public void addText(String text) {
        left.append(text);
    }
    
    public int deleteText(int k) {
        k = Math.min(k, left.length());
        left.setLength(left.length() - k);
        return k;
    }

    private String text() {
        return left.substring(Math.max(left.length() - 10, 0));
    }
    
    public String cursorLeft(int k) {
        for (; k > 0 && !left.isEmpty(); k--) {
            right.append(left.charAt(left.length() - 1));
            left.deleteCharAt(left.length() - 1);
        }
        return text();
    }
    
    public String cursorRight(int k) {
        for (; k > 0 && !right.isEmpty(); k--) {
            left.append(right.charAt(right.length() - 1));
            right.deleteCharAt(right.length() - 1);
        }
        return text();
    }
}

/**
 * Your TextEditor object will be instantiated and called as such:
 * TextEditor obj = new TextEditor();
 * obj.addText(text);
 * int param_2 = obj.deleteText(k);
 * String param_3 = obj.cursorLeft(k);
 * String param_4 = obj.cursorRight(k);
 */
```

__Python__:

```Python
class TextEditor:
    def __init__(self):
        self.left, self.right = [], []


    def addText(self, text: str) -> None:
        self.left.extend(list(text))


    def deleteText(self, k: int) -> int:
        pre = k
        while k and self.left:
            self.left.pop()
            k -= 1
        return pre - k


    def text(self) -> str:
        return ''.join(self.left[-10:])


    def cursorLeft(self, k: int) -> str:
        while k and self.left:
            self.right.append(self.left.pop())
            k -= 1
        return self.text()


    def cursorRight(self, k: int) -> str:
        while k and self.right:
            self.left.append(self.right.pop())
            k -= 1
        return self.text()
        


# Your TextEditor object will be instantiated and called as such:
# obj = TextEditor()
# obj.addText(text)
# param_2 = obj.deleteText(k)
# param_3 = obj.cursorLeft(k)
# param_4 = obj.cursorRight(k)
```
