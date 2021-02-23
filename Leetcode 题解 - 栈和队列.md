# Leetcode 题解 - 栈和队列
<!-- GFM-TOC -->
* [Leetcode 题解 - 栈和队列](#leetcode-题解---栈和队列)
    * [1. 用栈实现队列](#1-用栈实现队列)
    * [2. 用队列实现栈](#2-用队列实现栈)
    * [3. 最小值栈](#3-最小值栈)
    * [4. 用栈实现括号匹配](#4-用栈实现括号匹配)
    * [5. 数组中元素与下一个比它大的元素之间的距离](#5-数组中元素与下一个比它大的元素之间的距离)
    * [6. 循环数组中比当前元素大的下一个元素](#6-循环数组中比当前元素大的下一个元素)
<!-- GFM-TOC -->


## 1. 用栈实现队列

232\. Implement Queue using Stacks (Easy)

[Leetcode](https://leetcode.com/problems/implement-queue-using-stacks/description/) / [力扣](https://leetcode-cn.com/problems/implement-queue-using-stacks/description/)

栈的顺序为后进先出，而队列的顺序为先进先出。使用两个栈实现队列，一个元素需要经过两个栈才能出队列，在经过第一个栈时元素顺序被反转，经过第二个栈时再次被反转，此时就是先进先出顺序。

```cpp
class MyQueue {
public:
    stack<int> s1;
    stack<int> s2;
    /** Initialize your data structure here. */
    MyQueue() {

    }
    
    /** Push element x to the back of queue. */
    void push(int x) {
        while (!s2.empty()) {
            s1.push(s2.top());
            s2.pop();
        }
        s1.push(x);
    }
    
    /** Removes the element from in front of queue and returns that element. */
    int pop() {
        while (!s1.empty()) {
            s2.push(s1.top());
            s1.pop();
        }
        auto result = s2.top();
        s2.pop();
        return result;
    }
    
    /** Get the front element. */
    int peek() {
        while (!s1.empty()) {
            s2.push(s1.top());
            s1.pop();
        }
        return s2.top();
    }
    
    /** Returns whether the queue is empty. */
    bool empty() {
        return s1.empty() && s2.empty();
    }
};
``

## 2. 用队列实现栈

225\. Implement Stack using Queues (Easy)

[Leetcode](https://leetcode.com/problems/implement-stack-using-queues/description/) / [力扣](https://leetcode-cn.com/problems/implement-stack-using-queues/description/)

在将一个元素 x 插入队列时，为了维护原来的后进先出顺序，需要让 x 插入队列首部。而队列的默认插入顺序是队列尾部，因此在将 x 插入队列尾部之后，需要让除了 x 之外的所有元素出队列，再入队列。

```cpp
class MyStack {
public:
    queue<int> q1, q2;
    /** Initialize your data structure here. */
    MyStack() {

    }
    
    /** Push element x onto stack. */
    void push(int x) {
        if (q1.empty()) {
            q1.push(x);
            while (!q2.empty()) {
                q1.push(q2.front());
                q2.pop();
            }
        } else {
            q2.push(x);
            while (!q1.empty()) {
                q2.push(q1.front());
                q1.pop();
            }
        }
    }
    
    /** Removes the element on top of the stack and returns that element. */
    int pop() {
        int result = -1;
        if (!q1.empty()) {
            result = q1.front();
            q1.pop();
        }
        if (!q2.empty()) {
            result = q2.front();
            q2.pop();
        }
        return result;
    }
    
    /** Get the top element. */
    int top() {
        if (!q1.empty()) return q1.front();
        if (!q2.empty()) return q2.front();
        return -1;
    }
    
    /** Returns whether the stack is empty. */
    bool empty() {
        return q1.empty() && q2.empty();
    }
};
```

## 3. 最小值栈

155\. Min Stack (Easy)

[Leetcode](https://leetcode.com/problems/min-stack/description/) / [力扣](https://leetcode-cn.com/problems/min-stack/description/)
```cpp
class MinStack {
public:
    /** initialize your data structure here. */
    stack<int> s1, s2;
    MinStack() {

    }
    
    void push(int x) {
      s1.push(x);
      if (s2.empty() || x <= s2.top()) {
        s2.push(x);
      }
    }
    
    void pop() {
      if (s1.top() == s2.top()) {
        s2.pop();
      }
      s1.pop();
    }
    
    int top() {
      return s1.top();
    }
    
    int getMin() {
      return s2.top();
    }
};
```


对于实现最小值队列问题，可以先将队列使用栈来实现，然后就将问题转换为最小值栈，这个问题出现在 编程之美：3.7。

## 4. 用栈实现括号匹配

20\. Valid Parentheses (Easy)

[Leetcode](https://leetcode.com/problems/valid-parentheses/description/) / [力扣](https://leetcode-cn.com/problems/valid-parentheses/description/)

```html
"()[]{}"

Output : true
```
```cpp
class Solution {
public:
    bool isValid(string s) {
      stack<char> stk;
      for (auto c : s) {
        switch(c) {
          case ')':
            if (stk.empty() || stk.top() != '(') return false;
            stk.pop();
            break;
          case ']':
            if (stk.empty() || stk.top() != '[') return false;
            stk.pop();
            break;
          case '}':
            if (stk.empty() || stk.top() != '{') return false;
            stk.pop();
            break;
          default : 
            stk.push(c);
            break;
        }
      }
      return stk.empty();
    }
};
```

## 5. 数组中元素与下一个比它大的元素之间的距离

739\. Daily Temperatures (Medium)

[Leetcode](https://leetcode.com/problems/daily-temperatures/description/) / [力扣](https://leetcode-cn.com/problems/daily-temperatures/description/)

```html
Input: [73, 74, 75, 71, 69, 72, 76, 73]
Output: [1, 1, 4, 2, 1, 1, 0, 0]
```

在遍历数组时用栈把数组中的数存起来，如果当前遍历的数比栈顶元素来的大，说明栈顶元素的下一个比它大的数就是当前元素。

```cpp
class Solution {
public:
    vector<int> dailyTemperatures(vector<int>& T) {
      int n = T.size();
      vector<int> result(n, 0);
      stack<int> stk;
      for (int i = 0; i < n; i++) {
        if (stk.empty()) stk.push(i);
        else {
          while (!stk.empty() && T[stk.top()] < T[i]) {
            result[stk.top()] = i - stk.top();
            stk.pop();
          }
          stk.push(i);
        }
      }
      return result;
    }
};
```

## 6. 循环数组中比当前元素大的下一个元素

503\. Next Greater Element II (Medium)

[Leetcode](https://leetcode.com/problems/next-greater-element-ii/description/) / [力扣](https://leetcode-cn.com/problems/next-greater-element-ii/description/)

```text
Input: [1,2,1]
Output: [2,-1,2]
Explanation: The first 1's next greater number is 2;
The number 2 can't find next greater number;
The second 1's next greater number needs to search circularly, which is also 2.
```

与 739. Daily Temperatures (Medium) 不同的是，数组是循环数组，并且最后要求的不是距离而是下一个元素。

