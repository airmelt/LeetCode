__Description__:
You are given a data structure of employee information, which includes the employee's unique id, his importance value and his direct subordinates' id.

For example, employee 1 is the leader of employee 2, and employee 2 is the leader of employee 3. They have importance value 15, 10 and 5, respectively. Then employee 1 has a data structure like [1, 15, [2]], and employee 2 has [2, 10, [3]], and employee 3 has [3, 5, []]. Note that although employee 3 is also a subordinate of employee 1, the relationship is not direct.

Now given the employee information of a company, and an employee id, you need to return the total importance value of this employee and all his subordinates.

__Example:__
Example 1:

Input: [[1, 5, [2, 3]], [2, 3, []], [3, 3, []]], 1
Output: 11
Explanation:
Employee 1 has importance value 5, and he has two direct subordinates: employee 2 and employee 3. They both have importance value 3. So the total importance value of employee 1 is 5 + 3 + 3 = 11.
 

__Note:__

One employee has at most one direct leader and may have several subordinates.
The maximum number of employees won't exceed 2000.

__题目描述__:
给定一个保存员工信息的数据结构，它包含了员工唯一的id，重要度 和 直系下属的id。

比如，员工1是员工2的领导，员工2是员工3的领导。他们相应的重要度为15, 10, 5。那么员工1的数据结构是[1, 15, [2]]，员工2的数据结构是[2, 10, [3]]，员工3的数据结构是[3, 5, []]。注意虽然员工3也是员工1的一个下属，但是由于并不是直系下属，因此没有体现在员工1的数据结构中。

现在输入一个公司的所有员工信息，以及单个员工id，返回这个员工和他所有下属的重要度之和。

__示例 :__
示例 1:

输入: [[1, 5, [2, 3]], [2, 3, []], [3, 3, []]], 1
输出: 11
解释:
员工1自身的重要度是5，他有两个直系下属2和3，而且2和3的重要度均为3。因此员工1的总重要度是 5 + 3 + 3 = 11。

__注意:__ 

一个员工最多有一个直系领导，但是可以有多个直系下属
员工数量不超过2000。

__思路__:
关键是用 map存储员工的 id和对应的数据结构方便查找
1. 递归, 找到所有下属求重要性之和
2. 迭代, DFS的思想, 用队列或者栈存储员工的下属, 依次遍历
时间复杂度O(n), 空间复杂度O(n)

__代码__:
__C++__:
```
/*
// Employee info
class Employee {
public:
    // It's the unique ID of each node.
    // unique id of this employee
    int id;
    // the importance value of this employee
    int importance;
    // the id of direct subordinates
    vector<int> subordinates;
};
*/
class Solution {
public:
    int getImportance(vector<Employee*> employees, int id) {
        map<int, Employee*> m;
        for (Employee* e : employees) m[e -> id] = e;
        stack<Employee*> s;
        s.push(m[id]);
        int result = 0;
        while (s.size()) {
            Employee* e = s.top();
            s.pop();
            result += e -> importance;
            for (int subordinate_id : e -> subordinates) s.push(m[subordinate_id]);
        }
        return result;
    }
};
```

__Java__:
```
/*
// Employee info
class Employee {
    // It's the unique id of each node;
    // unique id of this employee
    public int id;
    // the importance value of this employee
    public int importance;
    // the id of direct subordinates
    public List<Integer> subordinates;
};
*/
class Solution {
    public int getImportance(List<Employee> employees, int id) {
        Map<Integer, Employee> map = new HashMap<>();
        for (Employee e : employees) map.put(e.id, e);
        Queue<Employee> queue = new LinkedList<>();
        queue.offer(map.get(id));
        int result = 0;
        while (!queue.isEmpty()) {
            Employee e = queue.poll();
            result += e.importance;
            for (int subordinateId : e.subordinates) queue.offer(map.get(subordinateId));
        }
        return result;
    }
}
```

__Python__:
```
"""
# Employee info
class Employee:
    def __init__(self, id, importance, subordinates):
        # It's the unique id of each node.
        # unique id of this employee
        self.id = id
        # the importance value of this employee
        self.importance = importance
        # the id of direct subordinates
        self.subordinates = subordinates
"""
class Solution:
    def getImportance(self, employees, id):
        """
        :type employees: Employee
        :type id: int
        :rtype: int
        """
        d = {e.id: e for e in employees}
        def get_total(id: int) -> int:
            e = d[id]
            return e.importance + sum(map(get_total, e.subordinates))
        return get_total(id)
```