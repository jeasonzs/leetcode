# Leetcode 题解 - 树
<!-- GFM-TOC -->
* [Leetcode 题解 - 树](#leetcode-题解---树)
    * [递归](#递归)
        * [1. 树的高度](#1-树的高度)
        * [2. 平衡树](#2-平衡树)
        * [3. 两节点的最长路径](#3-两节点的最长路径)
        * [4. 翻转树](#4-翻转树)
        * [5. 归并两棵树](#5-归并两棵树)
        * [6. 判断路径和是否等于一个数](#6-判断路径和是否等于一个数)
        * [7. 统计路径和等于一个数的路径数量](#7-统计路径和等于一个数的路径数量)
        * [8. 子树](#8-子树)
        * [9. 树的对称](#9-树的对称)
        * [10. 最小路径](#10-最小路径)
        * [11. 统计左叶子节点的和](#11-统计左叶子节点的和)
        * [12. 相同节点值的最大路径长度](#12-相同节点值的最大路径长度)
        * [13. 间隔遍历](#13-间隔遍历)
        * [14. 找出二叉树中第二小的节点](#14-找出二叉树中第二小的节点)
    * [层次遍历](#层次遍历)
        * [1. 一棵树每层节点的平均数](#1-一棵树每层节点的平均数)
        * [2. 得到左下角的节点](#2-得到左下角的节点)
    * [前中后序遍历](#前中后序遍历)
        * [1. 非递归实现二叉树的前序遍历](#1-非递归实现二叉树的前序遍历)
        * [2. 非递归实现二叉树的后序遍历](#2-非递归实现二叉树的后序遍历)
        * [3. 非递归实现二叉树的中序遍历](#3-非递归实现二叉树的中序遍历)
    * [BST](#bst)
        * [1. 修剪二叉查找树](#1-修剪二叉查找树)
        * [2. 寻找二叉查找树的第 k 个元素](#2-寻找二叉查找树的第-k-个元素)
        * [3. 把二叉查找树每个节点的值都加上比它大的节点的值](#3-把二叉查找树每个节点的值都加上比它大的节点的值)
        * [4. 二叉查找树的最近公共祖先](#4-二叉查找树的最近公共祖先)
        * [5. 二叉树的最近公共祖先](#5-二叉树的最近公共祖先)
        * [6. 从有序数组中构造二叉查找树](#6-从有序数组中构造二叉查找树)
        * [7. 根据有序链表构造平衡的二叉查找树](#7-根据有序链表构造平衡的二叉查找树)
        * [8. 在二叉查找树中寻找两个节点，使它们的和为一个给定值](#8-在二叉查找树中寻找两个节点，使它们的和为一个给定值)
        * [9. 在二叉查找树中查找两个节点之差的最小绝对值](#9-在二叉查找树中查找两个节点之差的最小绝对值)
        * [10. 寻找二叉查找树中出现次数最多的值](#10-寻找二叉查找树中出现次数最多的值)
    * [Trie](#trie)
        * [1. 实现一个 Trie](#1-实现一个-trie)
        * [2. 实现一个 Trie，用来求前缀和](#2-实现一个-trie，用来求前缀和)
<!-- GFM-TOC -->


## 递归

一棵树要么是空树，要么有两个指针，每个指针指向一棵树。树是一种递归结构，很多树的问题可以使用递归来处理。

### 1. 树的高度

104\. Maximum Depth of Binary Tree (Easy)

[Leetcode](https://leetcode.com/problems/maximum-depth-of-binary-tree/description/) / [力扣](https://leetcode-cn.com/problems/maximum-depth-of-binary-tree/description/)

```cpp
class Solution {
public:
    // int maxDepth(TreeNode* root) {
    //     if (root == nullptr) return 0;
    //     return 1 + max(maxDepth(root->left), maxDepth(root->right));
    // }
    int maxDepth(TreeNode* root) {
        if (root == nullptr) return 0;
        queue<TreeNode*> q;
        q.push(root);
        int depth = 0;
        while (!q.empty()) {
            depth++;
            for (int i = q.size(); i > 0; i--) {
                auto node = q.front();
                q.pop();
                if (node->left) q.push(node->left);
                if (node->right) q.push(node->right);
            }
        }
        return depth;
    }
};
```


### 2. 平衡树

110\. Balanced Binary Tree (Easy)

[Leetcode](https://leetcode.com/problems/balanced-binary-tree/description/) / [力扣](https://leetcode-cn.com/problems/balanced-binary-tree/description/)

```html
    3
   / \
  9  20
    /  \
   15   7
```

平衡树左右子树高度差都小于等于 1
```cpp
class Solution {
public:
    int depth(TreeNode* node) {
      if (!node) return 0;
      return max(depth(node->left), depth(node->right)) + 1;
    }
    bool isBalanced(TreeNode* root) {
      if (!root) return true;
      auto diff = abs(depth(root->left) - depth(root->right));
      return diff < 2 && isBalanced(root->left) && isBalanced(root->right);
    }
};
```

### 3. 两节点的最长路径

543\. Diameter of Binary Tree (Easy)

[Leetcode](https://leetcode.com/problems/diameter-of-binary-tree/description/) / [力扣](https://leetcode-cn.com/problems/diameter-of-binary-tree/description/)

```html
Input:

         1
        / \
       2  3
      / \
     4   5

Return 3, which is the length of the path [4,2,1,3] or [5,2,1,3].
```
```cpp
class Solution {
public:
    int mx = 0;
    int depth(TreeNode* node) {
        if (!node) return 0;
        auto l = depth(node->left);
        auto r = depth(node->right);
        mx = max(mx, l + r);
        return max(l, r) + 1;
    }
    int diameterOfBinaryTree(TreeNode* root) {
        depth(root);
        return mx;
    }
};
```
### 4. 翻转树

226\. Invert Binary Tree (Easy)

[Leetcode](https://leetcode.com/problems/invert-binary-tree/description/) / [力扣](https://leetcode-cn.com/problems/invert-binary-tree/description/)

```cpp
class Solution {
public:
    TreeNode* invertTree(TreeNode* root) {
      if (!root) return root;
      auto left = root->left;
      root->left = invertTree(root->right);
      root->right = invertTree(left);
      return root;
    }
};
```

### 5. 归并两棵树

617\. Merge Two Binary Trees (Easy)

[Leetcode](https://leetcode.com/problems/merge-two-binary-trees/description/) / [力扣](https://leetcode-cn.com/problems/merge-two-binary-trees/description/)

```html
Input:
       Tree 1                     Tree 2
          1                         2
         / \                       / \
        3   2                     1   3
       /                           \   \
      5                             4   7

Output:
         3
        / \
       4   5
      / \   \
     5   4   7
```
```cpp
class Solution {
public:
    TreeNode* mergeTrees(TreeNode* root1, TreeNode* root2) {
      if (!root1) return root2;
      if (!root2) return root1;
      auto left = mergeTrees(root1->left, root2->left);
      auto right = mergeTrees(root1->right, root2->right);
      return new TreeNode(root1->val + root2->val, left, right);
    }
};
```

### 6. 判断路径和是否等于一个数

Leetcdoe : 112. Path Sum (Easy)

[Leetcode](https://leetcode.com/problems/path-sum/description/) / [力扣](https://leetcode-cn.com/problems/path-sum/description/)

```html
Given the below binary tree and sum = 22,

              5
             / \
            4   8
           /   / \
          11  13  4
         /  \      \
        7    2      1

return true, as there exist a root-to-leaf path 5->4->11->2 which sum is 22.
```

路径和定义为从 root 到 leaf 的所有节点的和。
```cpp
class Solution {
public:
    bool hasPathSum(TreeNode* root, int targetSum) {
        if (root == nullptr) return false;
        if (root->left == nullptr && root->right == nullptr) return targetSum == root->val;
        return hasPathSum(root->left, targetSum - root->val) || hasPathSum(root->right, targetSum - root->val);
    }
};
```

### 7. 统计路径和等于一个数的路径数量

437\. Path Sum III (Easy)

[Leetcode](https://leetcode.com/problems/path-sum-iii/description/) / [力扣](https://leetcode-cn.com/problems/path-sum-iii/description/)

```html
root = [10,5,-3,3,2,null,11,3,-2,null,1], sum = 8

      10
     /  \
    5   -3
   / \    \
  3   2   11
 / \   \
3  -2   1

Return 3. The paths that sum to 8 are:

1.  5 -> 3
2.  5 -> 2 -> 1
3. -3 -> 11
```

路径不一定以 root 开头，也不一定以 leaf 结尾，但是必须连续。
```cpp
class Solution {
public:
    void traval(TreeNode* node, int sum, int& result) {
      if (!node) return;
      sum -= node->val;
      if (sum == 0) result++;
      traval(node->left, sum, result);
      traval(node->right, sum, result);

    }
    void pathSum(TreeNode* root, int sum, int& result) {
      if (!root) return;
      traval(root, sum, result);
      pathSum(root->left, sum, result);
      pathSum(root->right, sum, result);
    }
    int pathSum(TreeNode* root, int sum) {
        int result = 0;
        pathSum(root, sum, result);
        return result;
    }
};
```


### 8. 子树

572\. Subtree of Another Tree (Easy)

[Leetcode](https://leetcode.com/problems/subtree-of-another-tree/description/) / [力扣](https://leetcode-cn.com/problems/subtree-of-another-tree/description/)

```html
Given tree s:
     3
    / \
   4   5
  / \
 1   2

Given tree t:
   4
  / \
 1   2

Return true, because t has the same structure and node values with a subtree of s.

Given tree s:

     3
    / \
   4   5
  / \
 1   2
    /
   0

Given tree t:
   4
  / \
 1   2

Return false.
```
```cpp
class Solution {
public:
    bool isSame(TreeNode* s, TreeNode* t) {
      if (!s && !t) return true;
      if (!s || !t) return false;
      return s->val == t->val && isSame(s->left, t->left) && isSame(s->right, t->right);
    }
    bool isSubtree(TreeNode* s, TreeNode* t) {
      if (!s) return false;
      if (isSame(s, t)) return true;
      return isSubtree(s->left, t) || isSubtree(s->right, t);
    }
};
```

### 9. 树的对称

101\. Symmetric Tree (Easy)

[Leetcode](https://leetcode.com/problems/symmetric-tree/description/) / [力扣](https://leetcode-cn.com/problems/symmetric-tree/description/)

```html
    1
   / \
  2   2
 / \ / \
3  4 4  3
```
```cpp
class Solution {
public:
    bool isSymmetric(TreeNode* a, TreeNode* b) {
        if (!a && !b) return true;
        if (!a || !b || a->val != b->val) return false;
        return isSymmetric(a->left, b->right) && isSymmetric(a->right, b->left);
    }
    bool isSymmetric(TreeNode* root) {
        if (!root) return false;
        return isSymmetric(root->left, root->right);
    }
};
```


### 10. 最小路径

111\. Minimum Depth of Binary Tree (Easy)

[Leetcode](https://leetcode.com/problems/minimum-depth-of-binary-tree/description/) / [力扣](https://leetcode-cn.com/problems/minimum-depth-of-binary-tree/description/)

树的根节点到叶子节点的最小路径长度

```cpp
class Solution {
public:
    int minDepth(TreeNode* root) {
        if (!root) return 0;
        if (!root->left && !root->right) return 1;
        int min_depth = INT_MAX;
        if (root->left) min_depth = min(min_depth, minDepth(root->left));
        if (root->right) min_depth = min(min_depth, minDepth(root->right));
        return min_depth + 1;
    }
};
```

### 11. 统计左叶子节点的和

404\. Sum of Left Leaves (Easy)

[Leetcode](https://leetcode.com/problems/sum-of-left-leaves/description/) / [力扣](https://leetcode-cn.com/problems/sum-of-left-leaves/description/)

```html
    3
   / \
  9  20
    /  \
   15   7

There are two left leaves in the binary tree, with values 9 and 15 respectively. Return 24.
```
```cpp
class Solution {
public:
    int sumOfLeftLeaves(TreeNode* root) {
        if (!root) return 0;
        int result = 0;
        if (root->left && root->left->left == nullptr && root->left->right == nullptr) result += root->left->val;
        result += sumOfLeftLeaves(root->left);
        result += sumOfLeftLeaves(root->right);
        return result;
    }
};
```

### 12. 相同节点值的最大路径长度

687\. Longest Univalue Path (Easy)

[Leetcode](https://leetcode.com/problems/longest-univalue-path/) / [力扣](https://leetcode-cn.com/problems/longest-univalue-path/)

```html
             1
            / \
           4   5
          / \   \
         4   4   5

Output : 2
```
```cpp
class Solution {
public:
    int arrowLength(TreeNode* node, int& result) {
      if (!node) return 0;
      auto l = arrowLength(node->left, result);
      auto r = arrowLength(node->right, result);
      auto al = 0, ar = 0;
      if (node->left && node->val == node->left->val) al = l + 1;
      if (node->right && node->val == node->right->val) ar = r + 1;
      result = max(result, al + ar);
      return max(al, ar);
    }
    int longestUnivaluePath(TreeNode* root) {
      int result = 0;
      arrowLength(root, result);
      return result;
    }
};
```


### 13. 间隔遍历

337\. House Robber III (Medium)

[Leetcode](https://leetcode.com/problems/house-robber-iii/description/) / [力扣](https://leetcode-cn.com/problems/house-robber-iii/description/)

```html
     3
    / \
   2   3
    \   \
     3   1
Maximum amount of money the thief can rob = 3 + 3 + 1 = 7.
```
```cpp
class Solution {
public:
    int rob(TreeNode* root) {
      if(!root) return 0;
      int ls = 0, rs = 0;
      if (root->left) ls = rob(root->left->left) + rob(root->left->right);
      if (root->right) rs = rob(root->right->left) + rob(root->right->right);
      return max(ls + rs + root->val, rob(root->left) + rob(root->right));
    }
};
```

### 14. 找出二叉树中第二小的节点

671\. Second Minimum Node In a Binary Tree (Easy)

[Leetcode](https://leetcode.com/problems/second-minimum-node-in-a-binary-tree/description/) / [力扣](https://leetcode-cn.com/problems/second-minimum-node-in-a-binary-tree/description/)

```html
Input:
   2
  / \
 2   5
    / \
    5  7

Output: 5
```

一个节点要么具有 0 个或 2 个子节点，如果有子节点，那么根节点是最小的节点。


## 层次遍历

使用 BFS 进行层次遍历。不需要使用两个队列来分别存储当前层的节点和下一层的节点，因为在开始遍历一层的节点时，当前队列中的节点数就是当前层的节点数，只要控制遍历这么多节点数，就能保证这次遍历的都是当前层的节点。

### 1. 一棵树每层节点的平均数

637\. Average of Levels in Binary Tree (Easy)

[Leetcode](https://leetcode.com/problems/average-of-levels-in-binary-tree/description/) / [力扣](https://leetcode-cn.com/problems/average-of-levels-in-binary-tree/description/)
```cpp
class Solution {
public:
    vector<double> averageOfLevels(TreeNode* root) {
        queue<TreeNode*> q;
        vector<double> result;
        q.push(root);
        while (!q.empty()) {
            double sum = 0;
            int size = 0;
            for (int i = q.size(); i > 0; i--) {
                auto node = q.front();
                q.pop();
                if (!node) continue;
                sum += node->val;
                size++;
                q.push(node->left);
                q.push(node->right);
            }
            if (size) result.push_back(sum / size);
        }
        return result;
    }
};
```


### 2. 得到左下角的节点

513\. Find Bottom Left Tree Value (Easy)

[Leetcode](https://leetcode.com/problems/find-bottom-left-tree-value/description/) / [力扣](https://leetcode-cn.com/problems/find-bottom-left-tree-value/description/)

```html
Input:

        1
       / \
      2   3
     /   / \
    4   5   6
       /
      7

Output:
7
```
```cpp
class Solution {
public:
    int findBottomLeftValue(TreeNode* root) {
        queue<TreeNode*> q;
        q.push(root);
        int left = 0;
        while (!q.empty()) {
            bool is_first = true;
            for (int i = q.size(); i > 0; i--) {
                auto node = q.front();
                q.pop();
                if (!node) continue;
                if (is_first) left = node->val;
                is_first = false;
                q.push(node->left);
                q.push(node->right);
            }
        }
        return left;
    }
};
```

## 前中后序遍历

```html
    1
   / \
  2   3
 / \   \
4   5   6
```

- 层次遍历顺序：[1 2 3 4 5 6]
- 前序遍历顺序：[1 2 4 5 3 6]
- 中序遍历顺序：[4 2 5 1 3 6]
- 后序遍历顺序：[4 5 2 6 3 1]

层次遍历使用 BFS 实现，利用的就是 BFS 一层一层遍历的特性；而前序、中序、后序遍历利用了 DFS 实现。



### 1. 非递归实现二叉树的前序遍历

144\. Binary Tree Preorder Traversal (Medium)

[Leetcode](https://leetcode.com/problems/binary-tree-preorder-traversal/description/) / [力扣](https://leetcode-cn.com/problems/binary-tree-preorder-traversal/description/)
```cpp
class Solution {
public:
    // void traval(TreeNode* node, vector<int>& result) {
    //     if (!node) return;
    //     result.push_back(node->val);
    //     traval(node->left, result);
    //     traval(node->right, result);
    // }
    // vector<int> preorderTraversal(TreeNode* root) {
    //     vector<int> result;
    //     traval(root, result);
    //     return result;
    // }
    vector<int> preorderTraversal(TreeNode* root) {
        stack<TreeNode*> stk;
        vector<int> result;
        while (!stk.empty() || root) {
            while (root) {
                result.push_back(root->val);
                stk.push(root);
                root = root->left;
            }
            root = stk.top();
            stk.pop();
            root = root->right;
        }
        return result;
    }
};
```


### 2. 非递归实现二叉树的后序遍历

145\. Binary Tree Postorder Traversal (Medium)

[Leetcode](https://leetcode.com/problems/binary-tree-postorder-traversal/description/) / [力扣](https://leetcode-cn.com/problems/binary-tree-postorder-traversal/description/)

前序遍历为 root -\> left -\> right，后序遍历为 left -\> right -\> root。可以修改前序遍历成为 root -\> right -\> left，那么这个顺序就和后序遍历正好相反。


### 3. 非递归实现二叉树的中序遍历

94\. Binary Tree Inorder Traversal (Medium)

[Leetcode](https://leetcode.com/problems/binary-tree-inorder-traversal/description/) / [力扣](https://leetcode-cn.com/problems/binary-tree-inorder-traversal/description/)


## BST

二叉查找树（BST）：根节点大于等于左子树所有节点，小于等于右子树所有节点。

二叉查找树中序遍历有序。
```cpp
class Solution {
public:
                    
    // void traval(TreeNode* node, vector<int>& result) {
    //   if (!node) return;
    //   traval(node->left, result);
    //   result.push_back(node->val);
    //   traval(node->right, result);
    // }
    // vector<int> inorderTraversal(TreeNode* root) {
    //   vector<int> result;
    //   traval(root, result);
    //   return result;
    // }
    vector<int> inorderTraversal(TreeNode* root) {
      vector<int> result;
      stack<TreeNode*> stk;
      TreeNode* node = root;
      while (!stk.empty() || node) {
        while(node) {
          stk.push(node);
          node = node->left;
        }
        node = stk.top();
        stk.pop();
        result.push_back(node->val);
        node = node->right;
      }
      
      return result;
    }
};
```

### 1. 修剪二叉查找树

669\. Trim a Binary Search Tree (Easy)

[Leetcode](https://leetcode.com/problems/trim-a-binary-search-tree/description/) / [力扣](https://leetcode-cn.com/problems/trim-a-binary-search-tree/description/)

```html
Input:

    3
   / \
  0   4
   \
    2
   /
  1

  L = 1
  R = 3

Output:

      3
     /
   2
  /
 1
```

题目描述：只保留值在 L \~ R 之间的节点
```cpp
class Solution {
public:
    TreeNode* trimBST(TreeNode* root, int low, int high) {
      if (!root) return nullptr;
      if (root->val < low) return trimBST(root->right, low, high);
      if (root->val > high) return trimBST(root->left, low, high);
      root->left = trimBST(root->left, low, high);
      root->right = trimBST(root->right, low, high);
      return root;
    }
};
```

### 2. 寻找二叉查找树的第 k 个元素

230\. Kth Smallest Element in a BST (Medium)

[Leetcode](https://leetcode.com/problems/kth-smallest-element-in-a-bst/description/) / [力扣](https://leetcode-cn.com/problems/kth-smallest-element-in-a-bst/description/)



### 3. 把二叉查找树每个节点的值都加上比它大的节点的值

Convert BST to Greater Tree (Easy)

[Leetcode](https://leetcode.com/problems/convert-bst-to-greater-tree/description/) / [力扣](https://leetcode-cn.com/problems/convert-bst-to-greater-tree/description/)

```html
Input: The root of a Binary Search Tree like this:

              5
            /   \
           2     13

Output: The root of a Greater Tree like this:

             18
            /   \
          20     13
```

先遍历右子树。


### 4. 二叉查找树的最近公共祖先

235\. Lowest Common Ancestor of a Binary Search Tree (Easy)

[Leetcode](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-search-tree/description/) / [力扣](https://leetcode-cn.com/problems/lowest-common-ancestor-of-a-binary-search-tree/description/)

```html
        _______6______
      /                \
  ___2__             ___8__
 /      \           /      \
0        4         7        9
        /  \
       3   5

For example, the lowest common ancestor (LCA) of nodes 2 and 8 is 6. Another example is LCA of nodes 2 and 4 is 2, since a node can be a descendant of itself according to the LCA definition.
```

### 5. 二叉树的最近公共祖先

236\. Lowest Common Ancestor of a Binary Tree (Medium) 

[Leetcode](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-tree/description/) / [力扣](https://leetcode-cn.com/problems/lowest-common-ancestor-of-a-binary-tree/description/)

```html
       _______3______
      /              \
  ___5__           ___1__
 /      \         /      \
6        2       0        8
        /  \
       7    4

For example, the lowest common ancestor (LCA) of nodes 5 and 1 is 3. Another example is LCA of nodes 5 and 4 is 5, since a node can be a descendant of itself according to the LCA definition.
```


### 6. 从有序数组中构造二叉查找树

108\. Convert Sorted Array to Binary Search Tree (Easy)

[Leetcode](https://leetcode.com/problems/convert-sorted-array-to-binary-search-tree/description/) / [力扣](https://leetcode-cn.com/problems/convert-sorted-array-to-binary-search-tree/description/)
```cpp
class Solution {
public:
    TreeNode* sortedArrayToBST(vector<int>& nums, int s, int e) {
      if (s >= e) return nullptr;
      int mid = s + (e - s) / 2;
      auto left = sortedArrayToBST(nums, s, mid);
      auto right = sortedArrayToBST(nums, mid + 1, e);
      return new TreeNode(nums[mid], left, right);
    }
    TreeNode* sortedArrayToBST(vector<int>& nums) {
      return sortedArrayToBST(nums, 0, nums.size());
    }
};
```


### 7. 根据有序链表构造平衡的二叉查找树

109\. Convert Sorted List to Binary Search Tree (Medium)

[Leetcode](https://leetcode.com/problems/convert-sorted-list-to-binary-search-tree/description/) / [力扣](https://leetcode-cn.com/problems/convert-sorted-list-to-binary-search-tree/description/)

```html
Given the sorted linked list: [-10,-3,0,5,9],

One possible answer is: [0,-3,9,-10,null,5], which represents the following height balanced BST:

      0
     / \
   -3   9
   /   /
 -10  5
```
```cpp
class Solution {
public:
    TreeNode* toBST(vector<int>& nums, int s, int e) {
      if (s >= e) return nullptr;
      auto mid = s + (e - s) / 2;
      return new TreeNode(nums[mid], toBST(nums, s, mid), toBST(nums, mid + 1, e));
    }

    TreeNode* sortedListToBST(ListNode* head) {
      vector<int> nums;
      while (head) {
        nums.push_back(head->val);
        head = head->next;
      }
      return toBST(nums, 0, nums.size());
    }
};
```

### 8. 在二叉查找树中寻找两个节点，使它们的和为一个给定值

653\. Two Sum IV - Input is a BST (Easy)

[Leetcode](https://leetcode.com/problems/two-sum-iv-input-is-a-bst/description/) / [力扣](https://leetcode-cn.com/problems/two-sum-iv-input-is-a-bst/description/)

```html
Input:

    5
   / \
  3   6
 / \   \
2   4   7

Target = 9

Output: True
```

使用中序遍历得到有序数组之后，再利用双指针对数组进行查找。

应该注意到，这一题不能用分别在左右子树两部分来处理这种思想，因为两个待求的节点可能分别在左右子树中。
```cpp
class Solution {
public:
    void traval(vector<int>& nums, TreeNode* node) {
      if (!node) return;
      traval(nums, node->left);
      nums.push_back(node->val);
      traval(nums, node->right);
    }
    bool findTarget(TreeNode* root, int k) {
        vector<int> nums;
        traval(nums, root);
        int l = 0, r = nums.size() - 1;
        while (l < r) {
          auto v = nums[l] + nums[r];
          if (v > k) r--;
          else if (v < k) l++;
          else return true;
        }
        return false;
    }
};
```


### 9. 在二叉查找树中查找两个节点之差的最小绝对值

530\. Minimum Absolute Difference in BST (Easy)

[Leetcode](https://leetcode.com/problems/minimum-absolute-difference-in-bst/description/) / [力扣](https://leetcode-cn.com/problems/minimum-absolute-difference-in-bst/description/)

```html
Input:

   1
    \
     3
    /
   2

Output:

1
```

利用二叉查找树的中序遍历为有序的性质，计算中序遍历中临近的两个节点之差的绝对值，取最小值。
```cpp
class Solution {
public:
    void minDiff(TreeNode* node, int& mi, int& last) {
      if (!node) return;
      minDiff(node->left, mi, last);
      mi = min(abs(node->val - last), mi);
      last = node->val;
      minDiff(node->right, mi, last);
    }
    int getMinimumDifference(TreeNode* root) {
      int last = INT_MAX;
      int mi = INT_MAX;
      minDiff(root, mi, last);
      return mi;
    }
};
```

### 10. 寻找二叉查找树中出现次数最多的值

501\. Find Mode in Binary Search Tree (Easy)

[Leetcode](https://leetcode.com/problems/find-mode-in-binary-search-tree/description/) / [力扣](https://leetcode-cn.com/problems/find-mode-in-binary-search-tree/description/)

```html
   1
    \
     2
    /
   2

return [2].
```

答案可能不止一个，也就是有多个值出现的次数一样多。


## Trie

<div align="center"> <img src="https://cs-notes-1256109796.cos.ap-guangzhou.myqcloud.com/5c638d59-d4ae-4ba4-ad44-80bdc30f38dd.jpg"/> </div><br>

Trie，又称前缀树或字典树，用于判断字符串是否存在或者是否具有某种字符串前缀。

### 1. 实现一个 Trie

208\. Implement Trie (Prefix Tree) (Medium)

[Leetcode](https://leetcode.com/problems/implement-trie-prefix-tree/description/) / [力扣](https://leetcode-cn.com/problems/implement-trie-prefix-tree/description/)


### 2. 实现一个 Trie，用来求前缀和

677\. Map Sum Pairs (Medium)

[Leetcode](https://leetcode.com/problems/map-sum-pairs/description/) / [力扣](https://leetcode-cn.com/problems/map-sum-pairs/description/)

```html
Input: insert("apple", 3), Output: Null
Input: sum("ap"), Output: 3
Input: insert("app", 2), Output: Null
Input: sum("ap"), Output: 5
```
