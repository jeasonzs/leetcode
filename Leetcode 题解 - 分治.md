# Leetcode 题解 - 分治
<!-- GFM-TOC -->
* [Leetcode 题解 - 分治](#leetcode-题解---分治)
    * [1. 给表达式加括号](#1-给表达式加括号)
    * [2. 不同的二叉搜索树](#2-不同的二叉搜索树)
<!-- GFM-TOC -->


## 1. 给表达式加括号

241\. Different Ways to Add Parentheses (Medium)

[Leetcode](https://leetcode.com/problems/different-ways-to-add-parentheses/description/) / [力扣](https://leetcode-cn.com/problems/different-ways-to-add-parentheses/description/)

```html
Input: "2-1-1".

((2-1)-1) = 0
(2-(1-1)) = 2

Output : [0, 2]
```
```cpp
class Solution {
public:
    vector<int> diffWaysToCompute(string input) {
      vector<int> result;
      for (int i = 0; i < input.length(); i++) {
        auto c = input[i];
        if (c == '+' || c == '-' || c == '*') {
          auto left = diffWaysToCompute(input.substr(0, i));
          auto right = diffWaysToCompute(input.substr(i + 1, input.length() - i - 1));
          for (auto l : left) {
            for (auto r : right) {
              if (c == '+') result.push_back(l + r);
              else if (c == '-') result.push_back(l - r);
              else if (c == '*') result.push_back(l * r);
            }
          }
        }
      }
      if (result.empty()) result.push_back(stoi(input));
      return result;
    }
};
```


## 2. 不同的二叉搜索树

95\. Unique Binary Search Trees II (Medium)

[Leetcode](https://leetcode.com/problems/unique-binary-search-trees-ii/description/) / [力扣](https://leetcode-cn.com/problems/unique-binary-search-trees-ii/description/)

给定一个数字 n，要求生成所有值为 1...n 的二叉搜索树。

```html
Input: 3
Output:
[
  [1,null,3,2],
  [3,2,null,1],
  [3,1,null,null,2],
  [2,1,3],
  [1,null,2,null,3]
]
Explanation:
The above output corresponds to the 5 unique BST's shown below:

   1         3     3      2      1
    \       /     /      / \      \
     3     2     1      1   3      2
    /     /       \                 \
   2     1         2                 3
```
```cpp
class Solution {
public:
    vector<TreeNode*> generateTrees(int s, int e) {
      vector<TreeNode*> result;
      for (int i = s; i <= e; i++) {
        auto left = generateTrees(s, i - 1);
        auto right = generateTrees(i + 1, e);
        for (auto l : left) {
          for (auto r : right) {
            result.push_back(new TreeNode(i, l, r));
          }
        }
      }
      if (result.empty()) result.push_back(nullptr);
      return result;
    }
    vector<TreeNode*> generateTrees(int n) {
      return generateTrees(1, n);
    }
};
```
