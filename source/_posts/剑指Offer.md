---
title: 	剑指Offer
date: 2021-02-10 00:08:28
tags:
categories:
 - 算法
img: https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/02/11/lcof.png
---

### [剑指 Offer 03\. 数组中重复的数字](https://leetcode-cn.com/problems/shu-zu-zhong-zhong-fu-de-shu-zi-lcof/)

Difficulty: **简单**


找出数组中重复的数字。

在一个长度为 n 的数组 nums 里的所有数字都在 0～n-1 的范围内。数组中某些数字是重复的，但不知道有几个数字重复了，也不知道每个数字重复了几次。请找出数组中任意一个重复的数字。

**示例 1：**

```
输入：
[2, 3, 1, 0, 2, 5, 3]
输出：2 或 3 
```

**限制：**

`2 <= n <= 100000`


#### Solution

Language: ****

```
​
```

### [剑指 Offer 04\. 二维数组中的查找](https://leetcode-cn.com/problems/er-wei-shu-zu-zhong-de-cha-zhao-lcof/)

Difficulty: **中等**


在一个 n * m 的二维数组中，每一行都按照从左到右递增的顺序排序，每一列都按照从上到下递增的顺序排序。请完成一个高效的函数，输入这样的一个二维数组和一个整数，判断数组中是否含有该整数。

**示例:**

现有矩阵 matrix 如下：

```
[
  [1,   4,  7, 11, 15],
  [2,   5,  8, 12, 19],
  [3,   6,  9, 16, 22],
  [10, 13, 14, 17, 24],
  [18, 21, 23, 26, 30]
]
```

给定 target = `5`，返回 `true`。

给定 target = `20`，返回 `false`。

**限制：**

`0 <= n <= 1000`

`0 <= m <= 1000`

**注意：**本题与主站 240 题相同：


####  菜鸡第一印象-暴力法
O(nm)
```c++
​class Solution {
public:
    bool findNumberIn2DArray(vector<vector<int>>& matrix, int target)
    {
        if (matrix.empty() || matrix[0].empty())
        {
            return false;
        }

        for (const auto& vec : matrix)
        {
            if (vec[0] > target)
            {
                return false;
            }
            for (const auto& num : vec)
            {
                if (num == target)
                {
                    return true;
                }
                if (num > target)
                {
                    break;
                }
            }
        }
        return false;
    }
};
```

#### 大神点拨-站在右上角看,这个矩阵其实就像是一个BST

```c++
class Solution {
public:
    bool findNumberIn2DArray(vector<vector<int>>& matrix, int target)
    {
        if (matrix.empty() || matrix[0].empty())
        {
            return false;
        }
        
        int row_max = matrix.size();

        int column = matrix[0].size() - 1;
        int row = 0;

        while (row < row_max && column >= 0)
        {
            if (matrix[row][column] == target)
            {
                return true;
            }
            else if (matrix[row][column] > target)
            {
                column--;
            }
            else
            {
                row++;
            }
        }
        return false;
    }
};
```

#### 总结

初看题目, 还是没有详细审题漏掉了`每一列都按照从上到下递增的顺序排序`. 然后按照暴力法做了出来, 看了眼评论第一上面那句话让我翻回去看了下绝了.


### [剑指 Offer 05\. 替换空格](https://leetcode-cn.com/problems/ti-huan-kong-ge-lcof/)

Difficulty: **简单**


请实现一个函数，把字符串 `s` 中的每个空格替换成"%20"。

**示例 1：**

```
输入：s = "We are happy."
输出："We%20are%20happy."
```

**限制：**

`0 <= s 的长度 <= 10000`


#### Solution - 额外字符串reserve版本

```c++
​class Solution {
public:
    string replaceSpace(string s)
    {
        string result;
        result.reserve(s.size() * 3);

        for (char c : s)
        {
            if (c == ' ')
            {
                result.append("%20");
            }
            else
            {
                result += c;
            }
        }

        return result;
    }
};
```

#### Solution - 精打细算 resize原地

```c++
class Solution {
public:
    string replaceSpace(string s)
    {
        int num = 0;
        for (int i = 0; i < s.size(); ++i)
        {
            if (s[i] == ' ')
            {
                num++;
            }
        }

        s.resize(num * 2 + s.size());

        int sub = num * 2 + s.size();
        for (int i = s.size() - 1; i >= 0; --i)
        {
            if (s[i] != ' ')
            {
                sub--;
                s[sub] = s[i];
            }
            else
            {
                sub -= 3;
                s[sub] = '%';
                s[sub + 1] = '2';
                s[sub + 2] = '0';
            }
        }
        return s;
    }
};
```

### [剑指 Offer 06\. 从尾到头打印链表](https://leetcode-cn.com/problems/cong-wei-dao-tou-da-yin-lian-biao-lcof/)

Difficulty: **简单**


输入一个链表的头节点，从尾到头反过来返回每个节点的值（用数组返回）。

**示例 1：**

```
输入：head = [1,3,2]
输出：[2,3,1]
```

**限制：**

`0 <= 链表长度 <= 10000`


#### Solution - 两次遍历
```c++
class Solution {
public:
    vector<int> reversePrint(ListNode* head)
    {
        if (!head)
        {
            return vector<int>();
        }

        int length = 0;
        ListNode* temp = head;
        while (temp)
        {
            length++;
            temp = temp->next;
        }

        vector<int> result;
        result.resize(length);
        for (int i = length - 1; i >= 0; --i)
        {
            result[i] = head->val;
            head = head->next;
        }
        return result;
    }
};
```

#### Solution - 递归
```c++
class Solution {
public:
    vector<int> reversePrint(ListNode* head)
    {
        vector<int> result;
        reversePrint(head, &result);
        return result;
    }

    void reversePrint(ListNode* head, vector<int>* result)
    {
        if (!head)
        {
            return;
        }

        reversePrint(head->next, result);
        result->push_back(head->val);
    } 
};
```

#### Solution - 栈
```c++
class Solution {
public:
    vector<int> reversePrint(ListNode* head)
    {
        stack<ListNode*> sta;

        ListNode* temp = head;
        while (temp)
        {
            sta.push(temp);
            temp = temp->next;
        }

        vector<int> result(sta.size());
        int sub = 0;
        while (!sta.empty())
        {
            result[sub++] = sta.top()->val;
            sta.pop();
        }
        return result;
    }
};
```

### [剑指 Offer 07\. 重建二叉树](https://leetcode-cn.com/problems/zhong-jian-er-cha-shu-lcof/)

Difficulty: **中等**


输入某二叉树的前序遍历和中序遍历的结果，请重建该二叉树。假设输入的前序遍历和中序遍历的结果中都不含重复的数字。

例如，给出

```
前序遍历 preorder = [3,9,20,15,7]
中序遍历 inorder = [9,3,15,20,7]
```

返回如下的二叉树：

```
    3
   / \
  9  20
    /  \
   15   7
```

**限制：**

`0 <= 节点个数 <= 5000`

**注意**：本题与主站 105 题重复：


#### Solution

```c++
​class Solution {
public:
    TreeNode* buildTree(vector<int>& preorder, vector<int>& inorder)
    {
        return buildTree(preorder, inorder, 0, 0, inorder.size());
    }

    TreeNode* buildTree(vector<int>& preorder, vector<int>& inorder, int pre_l, int in_l, int in_r)
    {
        if (in_l == in_r)
        {
            return nullptr;
        }
        
        TreeNode* node = new TreeNode(preorder[pre_l]);

        for (int i = in_l; i < in_r; ++i)
        {
            if (inorder[i] == preorder[pre_l])
            {
                node->left = buildTree(preorder, inorder, pre_l + 1, in_l, i);
                node->right = buildTree(preorder, inorder, pre_l + 1 + i - in_l, i + 1, in_r);
            }
        }

        return node;
    }
};
```

### [剑指 Offer 09\. 用两个栈实现队列](https://leetcode-cn.com/problems/yong-liang-ge-zhan-shi-xian-dui-lie-lcof/)

Difficulty: **简单**


用两个栈实现一个队列。队列的声明如下，请实现它的两个函数 `appendTail` 和 `deleteHead` ，分别完成在队列尾部插入整数和在队列头部删除整数的功能。(若队列中没有元素，`deleteHead` 操作返回 -1 )

**示例 1：**

```
输入：
["CQueue","appendTail","deleteHead","deleteHead"]
[[],[3],[],[]]
输出：[null,null,3,-1]
```

**示例 2：**

```
输入：
["CQueue","deleteHead","appendTail","appendTail","deleteHead","deleteHead"]
[[],[],[5],[2],[],[]]
输出：[null,-1,null,null,5,2]
```

**提示：**

*   `1 <= values <= 10000`
*   `最多会对 appendTail、deleteHead 进行 10000 次调用`

#### Solution - 版本1 拷贝多

```c++
class CQueue {
public:
    CQueue() {

    }
    
    void appendTail(int value)
    {
        while (!core_stack.empty())
        {
            int temp = core_stack.top();
            core_stack.pop();
            temp_stack.push(temp);
        }

        temp_stack.push(value);
    }
    
    int deleteHead()
    {
        while (!temp_stack.empty())
        {
            int temp = temp_stack.top();
            temp_stack.pop();
            core_stack.push(temp);
        }
        
        int result = -1;
        if (!core_stack.empty())
        {
            result = core_stack.top();
            core_stack.pop();
        }
        return result;
    }

    stack<int> temp_stack;
    stack<int> core_stack;
};
```

#### Solution - 版本2 减少了拷贝

```c++
​class CQueue {
public:
    CQueue() {

    }
    
    void appendTail(int value)
    {
        temp_stack.push(value);
    }
    
    int deleteHead()
    {
        int result = -1;

        if (!core_stack.empty())
        {
            result = core_stack.top();
            core_stack.pop();
        }
        else
        {
            while (!temp_stack.empty())
            {
                int temp = temp_stack.top();
                temp_stack.pop();
                core_stack.push(temp);
            }

            if (!core_stack.empty())
            {
                result = core_stack.top();
                core_stack.pop();
            }
        }
        
        return result;
    }

    stack<int> temp_stack;
    stack<int> core_stack;
};
```
### [剑指 Offer 10- I. 斐波那契数列](https://leetcode-cn.com/problems/fei-bo-na-qi-shu-lie-lcof/)

Difficulty: **简单**


写一个函数，输入 `n` ，求斐波那契（Fibonacci）数列的第 `n` 项（即 `F(N)`）。斐波那契数列的定义如下：

```
F(0) = 0,   F(1) = 1
F(N) = F(N - 1) + F(N - 2), 其中 N > 1.
```

斐波那契数列由 0 和 1 开始，之后的斐波那契数就是由之前的两数相加而得出。

答案需要取模 1e9+7（1000000007），如计算初始结果为：1000000008，请返回 1。

**示例 1：**

```
输入：n = 2
输出：1
```

**示例 2：**

```
输入：n = 5
输出：5
```

**提示：**

*   `0 <= n <= 100`


#### Solution

```c++
class Solution {
public:
    int fib(int n)
    {
        int arr[max(n + 1, 2)];
        arr[0] = 0;
        arr[1] = 1;

        for (int i = 2; i <= n; ++i)
        {
            arr[i] = (arr[i - 1] + arr[i - 2]) % 1000000007;
        }

        return arr[n];
    }
};

```

### [剑指 Offer 10- II. 青蛙跳台阶问题](https://leetcode-cn.com/problems/qing-wa-tiao-tai-jie-wen-ti-lcof/)

Difficulty: **简单**


一只青蛙一次可以跳上1级台阶，也可以跳上2级台阶。求该青蛙跳上一个 `n` 级的台阶总共有多少种跳法。

答案需要取模 1e9+7（1000000007），如计算初始结果为：1000000008，请返回 1。

**示例 1：**

```
输入：n = 2
输出：2
```

**示例 2：**

```
输入：n = 7
输出：21
```

**示例 3：**

```
输入：n = 0
输出：1
```

**提示：**

*   `0 <= n <= 100`

注意：本题与主站 70 题相同：


#### Solution

```c++
​class Solution {
public:
    int numWays(int n)
    {
        int dp[max(n + 1, 2)];
        dp[0] = 1;
        dp[1] = 1;

        for (int i = 2; i <= n; ++i)
        {
            dp[i] = (dp[i - 1] + dp[i - 2]) % 1000000007;
        }

        return dp[n];
    }
};
```

### [剑指 Offer 11\. 旋转数组的最小数字](https://leetcode-cn.com/problems/xuan-zhuan-shu-zu-de-zui-xiao-shu-zi-lcof/)

Difficulty: **简单**


把一个数组最开始的若干个元素搬到数组的末尾，我们称之为数组的旋转。输入一个递增排序的数组的一个旋转，输出旋转数组的最小元素。例如，数组 `[3,4,5,1,2]` 为 `[1,2,3,4,5]` 的一个旋转，该数组的最小值为1。  

**示例 1：**

```
输入：[3,4,5,1,2]
输出：1
```

**示例 2：**

```
输入：[2,2,2,0,1]
输出：0
```

注意：本题与主站 154 题相同：


#### Solution - 暴力法

```c++
class Solution {
public:
    int minArray(vector<int>& numbers)
    {
        int result = numbers[0];
        for (int i = 0; i < numbers.size() - 1; ++i)
        {
            if (numbers[i] > numbers[i + 1])
            {
                result = numbers[i + 1];
                break;
            }
        }

        return result;
    }
};​
```

#### Solution - 二分 (一个排序好的数组, 虽然打乱一次 但仍应该首先考虑二分)

```c++
class Solution {
public:
    int minArray(vector<int>& numbers)
    {
        int low = 0;
        int high = numbers.size() - 1;

        while (low < high)
        {
            int mid = (low + high) / 2;

            if (numbers[mid] > numbers[high])
            {
                low = mid + 1;
            }
            else if (numbers[mid] < numbers[high])
            {
                high = mid;
            }
            else
            {
                high--;
            }
        }
        return numbers[low];
    }
};
```

### [剑指 Offer 12\. 矩阵中的路径](https://leetcode-cn.com/problems/ju-zhen-zhong-de-lu-jing-lcof/)

Difficulty: **中等**


请设计一个函数，用来判断在一个矩阵中是否存在一条包含某字符串所有字符的路径。路径可以从矩阵中的任意一格开始，每一步可以在矩阵中向左、右、上、下移动一格。如果一条路径经过了矩阵的某一格，那么该路径不能再次进入该格子。例如，在下面的3×4的矩阵中包含一条字符串“bfce”的路径（路径中的字母用加粗标出）。

[["a","**b**","c","e"],  
["s","**f**","**c**","s"],  
["a","d","**e**","e"]]

但矩阵中不包含字符串“abfb”的路径，因为字符串的第一个字符b占据了矩阵中的第一行第二个格子之后，路径不能再次进入这个格子。

**示例 1：**

```
输入：board = [["A","B","C","E"],["S","F","C","S"],["A","D","E","E"]], word = "ABCCED"
输出：true
```

**示例 2：**

```
输入：board = [["a","b"],["c","d"]], word = "abcd"
输出：false
```

**提示：**

*   `1 <= board.length <= 200`
*   `1 <= board[i].length <= 200`

注意：本题与主站 79 题相同：


#### Solution - dfs

Language: ****

```c++
​class Solution {
public:
    bool exist(vector<vector<char>>& board, string word)
    {
        for (int i = 0; i < board.size(); ++i)
        {
            for (int j = 0; j < board[0].size(); ++j)
            {
                if (word[0] == board[i][j])
                {
                    if (exist(board, word, i, j, 0))
                    {
                        return true;
                    }
                }
            }
        }
        return false;
    }

    bool exist(vector<vector<char>>& board, const string& word, int x, int y, int sub)
    {
        if (sub >= word.size())
        {
            return true;
        }

        if (x < 0 || y < 0 || x >= board.size() || y >= board[0].size())
        {
            return false;
        }

        if (board[x][y] == ' ')
        {
            return false;
        }

        if (board[x][y] != word[sub])
        {
            return false;
        }

        char back = board[x][y];
        board[x][y] = ' ';

        if (exist(board, word, x + 1, y, sub + 1) || 
                exist(board, word, x, y + 1, sub + 1) ||
                exist(board, word, x - 1, y, sub + 1) ||
                exist(board, word, x, y - 1, sub + 1))
        {
            return true;
        }

        board[x][y] = back;

        return false;
    }
};
```

### [剑指 Offer 13\. 机器人的运动范围](https://leetcode-cn.com/problems/ji-qi-ren-de-yun-dong-fan-wei-lcof/)

Difficulty: **中等**


地上有一个m行n列的方格，从坐标 `[0,0]` 到坐标 `[m-1,n-1]` 。一个机器人从坐标 `[0, 0]` 的格子开始移动，它每次可以向左、右、上、下移动一格（不能移动到方格外），也不能进入行坐标和列坐标的数位之和大于k的格子。例如，当k为18时，机器人能够进入方格 [35, 37] ，因为3+5+3+7=18。但它不能进入方格 [35, 38]，因为3+5+3+8=19。请问该机器人能够到达多少个格子？

**示例 1：**

```
输入：m = 2, n = 3, k = 1
输出：3
```

**示例 2：**

```
输入：m = 3, n = 1, k = 0
输出：1
```

**提示：**

*   `1 <= n,m <= 100`
*   `0 <= k <= 20`


#### Solution - dfs

```c++
​class Solution {
public:
    bool Check(int x, int y, int k)
    {
        int temp = 0;

        while (x != 0)
        {
            temp += x % 10;
            x /= 10;
        }
        while (y != 0)
        {
            temp += y % 10;
            y /= 10;
        }
        return temp <= k;
    }

    int movingCount(int m, int n, int k)
    {
        vector<vector<int>> vec(m, vector<int>(n, 0));
        return movingCount(vec, 0, 0, k);
    }

    constexpr static int SS = 4;
    int xx[SS] = {1, -1, 0, 0};
    int yy[SS] = {0, 0, 1, -1};

    int movingCount(vector<vector<int>>& vec, int x, int y, int k)
    {
        if (!Check(x, y, k))
        {
            return 0;
        }

        if (x < 0 || y < 0 || x >= vec.size() || y >= vec[0].size())
        {
            return 0;
        }

        if (vec[x][y] == 1)
        {
            return 0;
        }
        vec[x][y] = 1;
        int result = 1;

        for (int i = 0; i < SS; ++i)
        {
            result += movingCount(vec, x + xx[i], y + yy[i], k);
        }

        return result;
    }
};
```

### [剑指 Offer 14- I. 剪绳子](https://leetcode-cn.com/problems/jian-sheng-zi-lcof/)

Difficulty: **中等**


给你一根长度为 `n` 的绳子，请把绳子剪成整数长度的 `m` 段（m、n都是整数，n>1并且m>1），每段绳子的长度记为 `k[0],k[1]...k[m-1]` 。请问 `k[0]*k[1]*...*k[m-1]` 可能的最大乘积是多少？例如，当绳子的长度是8时，我们把它剪成长度分别为2、3、3的三段，此时得到的最大乘积是18。

**示例 1：**

```
输入: 2
输出: 1
解释: 2 = 1 + 1, 1 × 1 = 1
```

**示例 2:**

```
输入: 10
输出: 36
解释: 10 = 3 + 3 + 4, 3 × 3 × 4 = 36
```

**提示：**

*   `2 <= n <= 58`

注意：本题与主站 343 题相同：


#### Solution - dp

```c++
​class Solution
{
public:
    int cuttingRope(int n)
    {
        int* dp = new int[n + 1]{};
        // 初始状态
        dp[2] = 1;
        // 状态转移方程
        for(int i = 3; i < n + 1; i++)
        {
            for(int j = 2; j < i; j++)
            {
                dp[i] = max(dp[i], max(j * dp[i-j], j * (i - j)));
            }
        }
        // 返回值
        return dp[n];
    }
};
```
### [剑指 Offer 15\. 二进制中1的个数](https://leetcode-cn.com/problems/er-jin-zhi-zhong-1de-ge-shu-lcof/)

Difficulty: **简单**


请实现一个函数，输入一个整数（以二进制串形式），输出该数二进制表示中 1 的个数。例如，把 9 表示成二进制是 1001，有 2 位是 1。因此，如果输入 9，则该函数输出 2。

**示例 1：**

```
输入：00000000000000000000000000001011
输出：3
解释：输入的二进制串 00000000000000000000000000001011 中，共有三位为 '1'。
```

**示例 2：**

```
输入：00000000000000000000000010000000
输出：1
解释：输入的二进制串 00000000000000000000000010000000 中，共有一位为 '1'。
```

**示例 3：**

```
输入：11111111111111111111111111111101
输出：31
解释：输入的二进制串 11111111111111111111111111111101 中，共有 31 位为 '1'。
```

**提示：**

*   输入必须是长度为 `32` 的 **二进制串** 。

注意：本题与主站 191 题相同：


#### Solution - 右移运算符 循环M次 M为二进制位数和 负数会出错导致死循环

```c++
​class Solution {
public:
    int hammingWeight(uint32_t n)
    {
        int result = 0;

        while (n != 0)
        {
            result += n & 1;
            n = n >> 1;
        }
        return result;
    }
};
```

#### Solution - 解决负数的一种方法

```c++
class Solution
{
public:
    int hammingWeight(uint32_t n)
    {

    }
}
```

#### Solution - 更加高效的 循环M次 M为二进制1的位数

```c++
class Solution {
public:
    int hammingWeight(uint32_t n)
    {
        int result = 0;

        while (n != 0)
        {
            result++;
            n &= n - 1;
        }
        return result;
    }
};
```

把一个整数减去1之后再和原来的整数作`&`运算, 得到的结果相当于将二进制表示中的最右边一个1变成0

### [剑指 Offer 16\. 数值的整数次方](https://leetcode-cn.com/problems/shu-zhi-de-zheng-shu-ci-fang-lcof/)

Difficulty: **中等**


实现函数double Power(double base, int exponent)，求base的exponent次方。不得使用库函数，同时不需要考虑大数问题。

**示例 1:**

```
输入: 2.00000, 10
输出: 1024.00000
```

**示例 2:**

```
输入: 2.10000, 3
输出: 9.26100
```

**示例 3:**

```
输入: 2.00000, -2
输出: 0.25000
解释: 2-2 = 1/22 = 1/4 = 0.25
```

**说明:**

*   -100.0 < _x_ < 100.0
*   _n_ 是 32 位有符号整数，其数值范围是 [−2<sup>31</sup>, 2<sup>31 </sup>− 1] 。

注意：本题与主站 50 题相同：


#### Solution - 快速幂

```c++
​class Solution {
public:
    double myPow(double x, int n)
    {
        if (n == 0)
        {
            return 1;
        }

        bool flag = true;
        if (n > 0)
        {
            flag = false;
            n = -n;
        }

        double result = x;
        vector<double> ex;
        while (n < -1)
        {
            if (n % 2 == -1)
            {
                ex.push_back(result);
            }
            result *= result;
            n = n / 2; 
        }

        while (!ex.empty())
        {
            result *= ex.back();
            ex.pop_back();
        }

        if (flag)
        {
            result = 1 / result;
        }

        return result;
    }
};
```

#### Solution - 快速幂 减少空间 去除vector

```c++
class Solution {
public:
    double myPow(double x, int n)
    {
        if (n == 0)
        {
            return 1;
        }

        bool flag = true;
        if (n > 0)
        {
            flag = false;
            n = -n;
        }

        double result = x;
        double ex = 1.0;
        while (n < -1)
        {
            if (n % 2 == -1)
            {
                ex *= result;
            }
            result *= result;
            n = n / 2; 
        }

        result *= ex;

        if (flag)
        {
            result = 1 / result;
        }

        return result;
    }
};
```

### [剑指 Offer 18\. 删除链表的节点](https://leetcode-cn.com/problems/shan-chu-lian-biao-de-jie-dian-lcof/)

Difficulty: **简单**


给定单向链表的头指针和一个要删除的节点的值，定义一个函数删除该节点。

返回删除后的链表的头节点。

**注意：**此题对比原题有改动

**示例 1:**

```
输入: head = [4,5,1,9], val = 5
输出: [4,1,9]
解释: 给定你链表中值为 5 的第二个节点，那么在调用了你的函数之后，该链表应变为 4 -> 1 -> 9.
```

**示例 2:**

```
输入: head = [4,5,1,9], val = 1
输出: [4,5,9]
解释: 给定你链表中值为 1 的第三个节点，那么在调用了你的函数之后，该链表应变为 4 -> 5 -> 9.
```

**说明：**

*   题目保证链表中节点的值互不相同
*   若使用 C 或 C++ 语言，你不需要 `free` 或 `delete` 被删除的节点


#### Solution 爆栈Warning

```c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode* deleteNode(ListNode* head, int val)
    {
        if (!head)
        {
            return nullptr;
        }

        if (head->val == val)
        {
            return head->next;
        }
        
        head->next = deleteNode(head->next, val);
        return head;
    }
};
```


#### Solution - 保存last

```c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode* deleteNode(ListNode* head, int val)
    {
        if (!head)
        {
            return nullptr;
        }

        if (head->val == val)
        {
            return head->next;
        }

        ListNode* last = head;
        ListNode* temp = head->next;

        while (temp)
        {
            if (temp->val == val)
            {
                last->next = temp->next;
            }
            last = temp;
            temp = temp->next;
        }

        return head;
    }
};
```

### [剑指 Offer 20\. 表示数值的字符串](https://leetcode-cn.com/problems/biao-shi-shu-zhi-de-zi-fu-chuan-lcof/)

Difficulty: **中等**


请实现一个函数用来判断字符串是否表示数值（包括整数和小数）。例如，字符串"+100"、"5e2"、"-123"、"3.1416"、"-1E-16"、"0123"都表示数值，但"12e"、"1a3.14"、"1.2.3"、"+-5"及"12e+5.4"都不是。


#### Solution if else - 我愿称其为最强题目 尝试了N遍才过

```c++
​class Solution {
public:
    bool isNumber(string s)
    {
        int begin = 0;
        int end = s.size();
        bool num = false;

        // 去除开头结尾空格
        while (begin < end && s[begin] == ' ')
        {
            begin++;
        }
        while (end > begin && s[end - 1] == ' ')
        {
            end--;
        }
        int sub = begin;
    
        // + - 打头直接去掉
        if (s[sub] == '+' || s[sub] == '-')
        {
            sub++;
        }

        // 判断+-之后是否有小数点
        bool has_point = false;
        if (s[sub] == '.')
        {
            has_point = true;
            sub++;
        }
        
        // 正常数字判断
        while (sub < end)
        {
            if (s[sub] == '.')
            {
                if (has_point)
                {
                    return false; // 小数点只能出现一次
                }
                has_point = true;
            }
            else if (s[sub] >= '0' && s[sub] <= '9')
            {
                num = true; // 是一个数字
            }
            else
            {
                break;
            }
            sub++;
        }

        // 科学计数法判断 E或e 不作为第一个或者最后一个字符
        if (sub > begin && sub < end - 1 && (s[sub] == 'e' || s[sub] == 'E'))
        {
            sub++;

            // 去掉E e之后紧接着的 +- 必须 +-只有还有数字
            if ((s[sub] == '+' || s[sub] == '-') && sub < end - 1)
            {
                sub++;
            }

            while (sub < end && s[sub] >= '0' && s[sub] <= '9')
            {
                sub++;
            }
        }
        return num && sub == end; // 含数字且已经遍历完毕
    }
};
```

#### Solution -  自动状态机

```c++

```

### [剑指 Offer 21\. 调整数组顺序使奇数位于偶数前面](https://leetcode-cn.com/problems/diao-zheng-shu-zu-shun-xu-shi-qi-shu-wei-yu-ou-shu-qian-mian-lcof/)

Difficulty: **简单**


输入一个整数数组，实现一个函数来调整该数组中数字的顺序，使得所有奇数位于数组的前半部分，所有偶数位于数组的后半部分。

**示例：**

```
输入：nums = [1,2,3,4]
输出：[1,3,2,4] 
注：[3,1,2,4] 也是正确的答案之一。
```

**提示：**

1.  `0 <= nums.length <= 50000`
2.  `1 <= nums[i] <= 10000`


#### Solution - 代码不简洁版本 双指针

```c++
​class Solution {
public:
    vector<int> exchange(vector<int>& nums)
    {
        if (nums.empty())
        {
            return nums;
        }

        int begin = 0;
        int end = nums.size() - 1;

        while (begin < end)
        {
            while (nums[end] % 2 == 0)
            {
                end--;
                if (begin > end)
                {
                    return nums;
                }
            }

            if (nums[begin] % 2 == 0)
            {
                swap(nums[begin], nums[end--]);
            }
            else
            {
                begin++;
            }
        }
        return nums;
    }
};
```

#### Solution - 使用continue简化边界判断 双指针

```c++
class Solution {
public:
    vector<int> exchange(vector<int>& nums)
    {
        int left = 0; 
        int right = nums.size() - 1;

        while (left < right)
        {
            if ((nums[left] & 1) != 0)
            {
                left ++;
                continue;
            }
            if ((nums[right] & 1) != 1)
            {
                right --;
                continue;
            }
            swap(nums[left++], nums[right--]);
        }
        return nums;
    }
};
```


#### Solution - 快慢指针

```c++
class Solution {
public:
    vector<int> exchange(vector<int>& nums)
    {
        int slow = 0;
        int fast = 0;

        while (fast < nums.size())
        {
            if ((nums[fast] & 1) == 0)
            {
                fast++;
                continue;
            }
            swap(nums[slow++], nums[fast++]);
        }
        return nums;
    }
};
```
慢指针负责指向偶数, 当快指针寻找到奇数时 进行替换 然后两者都向前移动

不过这个代码存在原地tp的问题 极端情况下如果全是奇数则会替换size次

### [剑指 Offer 22\. 链表中倒数第k个节点](https://leetcode-cn.com/problems/lian-biao-zhong-dao-shu-di-kge-jie-dian-lcof/)

Difficulty: **简单**


输入一个链表，输出该链表中倒数第k个节点。为了符合大多数人的习惯，本题从1开始计数，即链表的尾节点是倒数第1个节点。

例如，一个链表有 `6` 个节点，从头节点开始，它们的值依次是 `1、2、3、4、5、6`。这个链表的倒数第 `3` 个节点是值为 `4` 的节点。

**示例：**

```
给定一个链表: 1->2->3->4->5, 和 k = 2.

返回链表 4->5.
```


#### Solution - 依旧是解决链表问题的通用方法 快慢指针

```c++
​class Solution {
public:
    ListNode* getKthFromEnd(ListNode* head, int k)
    {
        if (!head)
        {
            return nullptr;
        }

        ListNode* fast = head;
        for (int i = 0; i < k; ++i)
        {
            fast = fast->next;
        }

        ListNode* slow = head;
        while (fast)
        {
            slow = slow->next;
            fast = fast->next;
        }
        return slow;
    }
};
```

判断链表环依然采用快慢指针, 快指针一次走两步 注意判断两次fast不为nullptr 当fast==low的时候说明有环

判断环长度, 第一次相遇后 到 第二次相遇 慢指针走的次数即为长度. 快指针在相遇后正好比慢指针多走一圈


1->2->3->4

1->2->3->4->5

取中间节点 快指针走两步 慢指针走一步 当快指针无法走两步的时候 慢指针所指为中间

### [剑指 Offer 24\. 反转链表](https://leetcode-cn.com/problems/fan-zhuan-lian-biao-lcof/)

Difficulty: **简单**


定义一个函数，输入一个链表的头节点，反转该链表并输出反转后链表的头节点。

**示例:**

```
输入: 1->2->3->4->5->NULL
输出: 5->4->3->2->1->NULL
```

**限制：**

`0 <= 节点个数 <= 5000`

**注意**：本题与主站 206 题相同：


#### Solution1 - 代码过长的递归


```c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode* reverseList(ListNode* head)
    {
        if (!head)
        {
            return nullptr;
        }
        
        ListNode* new_head = nullptr;
        ListNode* front = reverse(head, &new_head);
        front->next = nullptr;

        return new_head;
    }

    ListNode* reverse(ListNode* node, ListNode** head)
    {
        if (!node)
        {
            return nullptr;
        }

        ListNode* front = reverse(node->next, head);
        if (front)
        {
            front->next = node;
        }
        else
        {
            *head = node;
        }
        return node;
    }
};
```

#### Solution2 - 代码简洁的递归

```c++
class Solution {
public:
    ListNode* reverseList(ListNode* head)
    {
        if (!head || !head->next)
        {
            return head;
        }

        ListNode* ret = reverseList(head->next);

        head->next->next = head;
        head->next = nullptr;
        return ret;
    }
};
```
首先S2开头的`!head->next`的判断减少了一次了递归 同时也简化了S1中的`front`的判断 不会有空指针.

然后S1使用的返回值 返回的下一个节点. 然而实际上下一个节点已经可以通过本节点的`next`访问 通过`next->next`来倒转指向

这样将返回值省了出来 返回原链表的尾结点 作为新的头结点

#### Solution - 双指针 又双叒叕

```c++
class Solution {
public:
    ListNode* reverseList(ListNode* head)
    {
        if (!head)
        {
            return nullptr;
        }

        ListNode* node2 = head->next;
        ListNode* node1 = head;

        node1->next = nullptr;

        ListNode* back = nullptr;
        while (node2)
        {
            back = node2->next;

            node2->next = node1;
            node1 = node2;

            node2 = back;
        }
        return node1;
    }
};
```

### [剑指 Offer 25\. 合并两个排序的链表](https://leetcode-cn.com/problems/he-bing-liang-ge-pai-xu-de-lian-biao-lcof/)

Difficulty: **简单**


输入两个递增排序的链表，合并这两个链表并使新链表中的节点仍然是递增排序的。

**示例1：**

```
输入：1->2->4, 1->3->4
输出：1->1->2->3->4->4
```

**限制：**

`0 <= 链表长度 <= 1000`

注意：本题与主站 21 题相同：


#### Solution - 未使用虚拟头结点 代码较长且重复

```c++
class Solution {
public:
    ListNode* mergeTwoLists(ListNode* l1, ListNode* l2)
    {
        if (!l1)
        {
            return l2;
        }

        if (!l2)
        {
            return l1;
        }

        ListNode* ret = nullptr;
        if (l1->val < l2->val)
        {
            ret = l1;
            l1 = l1->next;
        }
        else
        {
            ret = l2;
            l2 = l2->next;
        }

        ListNode* node = ret;
        while (l1 || l2)
        {
            if (!l1)
            {
                node->next = l2;
                break;
            }
            if (!l2)
            {
                node->next = l1;
                break;
            }

            if (l1->val < l2->val)
            {
                node->next = l1;
                l1 = l1->next;
            }
            else
            {
                node->next = l2;
                l2 = l2->next;
            }
            node = node->next;
        }
        return ret;
    }
};
```

#### Solution - 使用虚拟头结点 代码大幅简洁 去除了重复片段

```c++
class Solution {
public:
    ListNode* mergeTwoLists(ListNode* l1, ListNode* l2)
    {
        ListNode* dumy = new ListNode(0);

        ListNode* node = dumy;
        while (l1 || l2)
        {
            if (!l1)
            {
                node->next = l2;
                break;
            }
            if (!l2)
            {
                node->next = l1;
                break;
            }

            if (l1->val < l2->val)
            {
                node->next = l1;
                l1 = l1->next;
            }
            else
            {
                node->next = l2;
                l2 = l2->next;
            }
            node = node->next;
        }
        return dumy->next;
    }
};
```

使用了虚拟头结点 直接简化了重复片段 tql

### [剑指 Offer 26\. 树的子结构](https://leetcode-cn.com/problems/shu-de-zi-jie-gou-lcof/)

Difficulty: **中等**


输入两棵二叉树A和B，判断B是不是A的子结构。(约定空树不是任意一个树的子结构)

B是A的子结构， 即 A中有出现和B相同的结构和节点值。

例如:  
给定的树 A:

`     3  
    / \  
   4   5  
  / \  
 1   2`  
给定的树 B：

`   4   
  /  
 1`  
返回 true，因为 B 与 A 的一个子树拥有相同的结构和节点值。

**示例 1：**

```
输入：A = [1,2,3], B = [3,1]
输出：false
```

**示例 2：**

```
输入：A = [3,4,5,1,2], B = [4,1]
输出：true
```

**限制：**

`0 <= 节点个数 <= 10000`


#### Solution - 先序遍历

```c++
​class Solution {
public:
    bool isSubStructure(TreeNode* A, TreeNode* B)
    {
        if (!A || !B)
        {
            return false;
        }

        if (A->val == B->val)
        {
            if (cmpTree(A, B))
            {
                return true;
            }
        }

        return isSubStructure(A->left, B) ||
        isSubStructure(A->right, B);
    }

    bool cmpTree(TreeNode* A, TreeNode* B)
    {
        if (!B)
        {
            return true;
        }
        if (!A)
        {
            return false;
        }

        if (A->val == B->val)
        {
            return cmpTree(A->left, B->left) && cmpTree(A->right, B->right);
        }
        return false;
    }
};
```

#### Solution - 简洁了一些 但我认为一个ifelse比一个函数调用更加高效

```c++
class Solution {
public:
    bool isSubStructure(TreeNode* A, TreeNode* B)
    {
        if (!A || !B)
        {
            return false;
        }

        return cmpTree(A, B) || isSubStructure(A->left, B) ||
        isSubStructure(A->right, B);
    }

    bool cmpTree(TreeNode* A, TreeNode* B)
    {
        if (!B)
        {
            return true;
        }
        if (!A)
        {
            return false;
        }

        return (A->val == B->val) && cmpTree(A->left, B->left) && cmpTree(A->right, B->right);
    }
};
```

### [剑指 Offer 27\. 二叉树的镜像](https://leetcode-cn.com/problems/er-cha-shu-de-jing-xiang-lcof/)

Difficulty: **简单**


请完成一个函数，输入一个二叉树，该函数输出它的镜像。

例如输入：

`     4  
   /   \  
  2     7  
 / \   / \  
1   3 6   9`  
镜像输出：

`     4  
   /   \  
  7     2  
 / \   / \  
9   6 3   1`

**示例 1：**

```
输入：root = [4,2,7,1,3,6,9]
输出：[4,7,2,9,6,3,1]
```

**限制：**

`0 <= 节点个数 <= 1000`

注意：本题与主站 226 题相同：


#### Solution - 递归

```c++
​class Solution {
public:
    TreeNode* mirrorTree(TreeNode* root)
    {
        if (!root)
        {
            return nullptr;
        }
        
        swap(root->left, root->right);

        mirrorTree(root->left);
        mirrorTree(root->right);

        return root;
    }
};
```

#### Solution - 迭代

```c++
class Solution {
public:
    TreeNode* mirrorTree(TreeNode* root)
    {
        if (!root)
        {
            return nullptr;
        }

        deque<TreeNode*> nodes;
        nodes.push_back(root);

        while (!nodes.empty())
        {
            TreeNode* node = nodes.front();
            nodes.pop_front();
            swap(node->left, node->right);

            if (node->left)
            {
                nodes.push_back(node->left);
            }
            if (node->right)
            {
                nodes.push_back(node->right);
            }
        }

        return root;
    }
};
```

### [剑指 Offer 28\. 对称的二叉树](https://leetcode-cn.com/problems/dui-cheng-de-er-cha-shu-lcof/)

Difficulty: **简单**


请实现一个函数，用来判断一棵二叉树是不是对称的。如果一棵二叉树和它的镜像一样，那么它是对称的。

例如，二叉树 [1,2,2,3,4,4,3] 是对称的。

`    1  
   / \  
  2   2  
 / \ / \  
3  4 4  3`  
但是下面这个 [1,2,2,null,3,null,3] 则不是镜像对称的:

`    1  
   / \  
  2   2  
   \   \  
   3    3`

**示例 1：**

```
输入：root = [1,2,2,3,4,4,3]
输出：true
```

**示例 2：**

```
输入：root = [1,2,2,null,3,null,3]
输出：false
```

**限制：**

`0 <= 节点个数 <= 1000`

注意：本题与主站 101 题相同：


#### Solution - 递归

```c++
​class Solution {
public:
    bool isSymmetric(TreeNode* root)
    {
        if (!root)
        {
            return true;
        }

        return solve(root->left, root->right);
    }

    bool solve(TreeNode* lhs, TreeNode* rhs)
    {
        if (!lhs && !rhs)
        {
            return true;
        }
        if (!lhs || !rhs)
        {
            return false;
        }

        if (lhs->val == rhs->val)
        {
            return solve(lhs->left, rhs->right) && solve(lhs->right, rhs->left);
        }
        return false;
    }
};
```

#### Solution - 迭代

```c++
class Solution {
public:
    bool isSymmetric(TreeNode* root)
    {
        if (!root)
        {
            return true;
        }

        list<TreeNode*> nodes;
        nodes.push_back(root->left);
        nodes.push_back(root->right);
        
        while (!nodes.empty())
        {
            TreeNode* node1 = nodes.front();
            nodes.pop_front();
            TreeNode* node2 = nodes.front();
            nodes.pop_front();

            if (!node1 && !node2)
            {
                continue;
            }
            else if (node1 && node2)
            {
                if (node1->val != node2->val)
                {
                    return false;
                }
                nodes.push_back(node1->left);
                nodes.push_back(node2->right);
                nodes.push_back(node1->right);
                nodes.push_back(node2->left);
            }
            else
            {
                return false;
            }
        }
        return true;
    }
};
```

### [剑指 Offer 29\. 顺时针打印矩阵](https://leetcode-cn.com/problems/shun-shi-zhen-da-yin-ju-zhen-lcof/)

Difficulty: **简单**


输入一个矩阵，按照从外向里以顺时针的顺序依次打印出每一个数字。

**示例 1：**

```
输入：matrix = [[1,2,3],[4,5,6],[7,8,9]]
输出：[1,2,3,6,9,8,7,4,5]
```

**示例 2：**

```
输入：matrix = [[1,2,3,4],[5,6,7,8],[9,10,11,12]]
输出：[1,2,3,4,8,12,11,10,9,5,6,7]
```

**限制：**

*   `0 <= matrix.length <= 100`
*   `0 <= matrix[i].length <= 100`

注意：本题与主站 54 题相同：


#### Solution - 额外MN空间

```c++
class Solution {
public:
    vector<int> spiralOrder(vector<vector<int>>& matrix)
    {
        if (matrix.empty() || matrix[0].empty())
        {
            return vector<int>();
        }

        const int MX = matrix.size();
        const int MY = matrix[0].size();
        vector<vector<int>> flags(MX, vector<int>(MY, 0));

        int x = 0, y = 0;

        vector<int> result;
        result.reserve(MX * MY);

        int xx[] = {0, 1, 0, -1};
        int yy[] = {1, 0, -1, 0};

        int i = 0;

        while (x >= 0 && y >= 0 && x < MX && y < MY && flags[x][y] != 1)
        {
            for (;x >= 0 && y >= 0 && x < MX && y < MY && flags[x][y] != 1; 
                x += xx[i], y += yy[i])
            {
                flags[x][y] = 1;
                result.push_back(matrix[x][y]);
            }

            x -= xx[i];
            y -= yy[i];

            i = (i + 1) % 4;

            x += xx[i];
            y += yy[i];
        }
        return result;
    }
};
```

#### Solution - 边界控制 不使用额外空间

```c++
class Solution {
public:
    vector<int> spiralOrder(vector<vector<int>>& matrix)
    {
        if (matrix.empty() || matrix[0].empty())
        {
            return vector<int>();
        }

        const int MX = matrix.size();
        const int MY = matrix[0].size();

        vector<int> result;
        result.reserve(MX * MY);

        int xx[] = {0, 1, 0, -1};
        int yy[] = {1, 0, -1, 0};

        // int nn[] = {1, 0, 0, 0};
        // int ee[] = {0, -1, 0, 0};
        // int ss[] = {0, 0, -1, 0};
        // int ww[] = {0, 0, 0, 1};

        int i = 0;
        int x = 0, y = 0;

        int e = MY, s = MX, w = 0, n = 0;

        while (x >= n && y >= w && x < s && y < e)
        {
            for (;x >= n && y >= w && x < s && y < e; 
                x += xx[i], y += yy[i])
            {
                result.push_back(matrix[x][y]);
            }

            x -= xx[i];
            y -= yy[i];
            

            // n += nn[i];
            // e += ee[i];
            // s += ss[i]; 更短的写法
            // w += ww[i];

            if (i == 0)
            {
                n++;
            }
            else if (i == 1)
            {
                e--;
            }
            else if (i == 2)
            {
                s--;
            }
            else
            {
                w++;
            }

            i = (i + 1) % 4;
            x += xx[i];
            y += yy[i];
        }
        return result;
    }
};
```

### [剑指 Offer 31\. 栈的压入、弹出序列](https://leetcode-cn.com/problems/zhan-de-ya-ru-dan-chu-xu-lie-lcof/)

Difficulty: **中等**


输入两个整数序列，第一个序列表示栈的压入顺序，请判断第二个序列是否为该栈的弹出顺序。假设压入栈的所有数字均不相等。例如，序列 {1,2,3,4,5} 是某栈的压栈序列，序列 {4,5,3,2,1} 是该压栈序列对应的一个弹出序列，但 {4,3,5,1,2} 就不可能是该压栈序列的弹出序列。

**示例 1：**

```
输入：pushed = [1,2,3,4,5], popped = [4,5,3,2,1]
输出：true
解释：我们可以按以下顺序执行：
push(1), push(2), push(3), push(4), pop() -> 4,
push(5), pop() -> 5, pop() -> 3, pop() -> 2, pop() -> 1
```

**示例 2：**

```
输入：pushed = [1,2,3,4,5], popped = [4,3,5,1,2]
输出：false
解释：1 不能在 2 之前弹出。
```

**提示：**

1.  `0 <= pushed.length == popped.length <= 1000`
2.  `0 <= pushed[i], popped[i] < 1000`
3.  `pushed` 是 `popped` 的排列。

注意：本题与主站 946 题相同：


#### Solution1 - X一样长的代码

```c++
class Solution {
public:
    bool validateStackSequences(vector<int>& pushed, vector<int>& popped)
    {
        stack<int> lstack;

        int i = 0, j = 0;

        while (i < pushed.size() && j < popped.size())
        {
            while (!lstack.empty())
            {
                if (lstack.top() == popped[j])
                {
                    lstack.pop();
                    j++;
                }
                else
                {
                    break;
                }
            }
            if (pushed[i] == popped[j])
            {
                i++;
                j++;
            }
            else
            {
                lstack.push(pushed[i]);
                i++;
            }
        }

        while (!lstack.empty())
        {
            if (lstack.top() == popped[j])
            {
                lstack.pop();
                j++;
            }
            else
            {
                return false;
            }
        }
        return true;
    }
};
```


#### Solution2 - 大佬精简的代码

```c++
class Solution {
public:
    bool validateStackSequences(vector<int>& pushed, vector<int>& popped)
    {
        stack<int> lstack;

        int j = 0;
        for (int num : pushed)
        {
            lstack.push(num);

            while (!lstack.empty() && lstack.top() == popped[j])
            {
                lstack.pop();
                j++;
            }
        }
        return j == popped.size();
    }
};
```

**精简思路**

源代码是栈不为空的时候 比较栈顶和`popped[i]`

如果相等则出栈

如果不相等 转去判断`pushed当前元素`和`popped[i]`

如果两者相等则后移

如果两者不相等则入栈当前元素.

*实际上这里就可以直接入栈元素然后再判断,如果相等了就出栈 如果不相等也恰好入栈了*

### [剑指 Offer 32 - I. 从上到下打印二叉树](https://leetcode-cn.com/problems/cong-shang-dao-xia-da-yin-er-cha-shu-lcof/)

Difficulty: **中等**


从上到下打印出二叉树的每个节点，同一层的节点按照从左到右的顺序打印。

例如:  
给定二叉树: `[3,9,20,null,null,15,7]`,

```
    3
   / \
  9  20
    /  \
   15   7
```

返回：

```
[3,9,20,15,7]
```

**提示：**

1.  `节点总数 <= 1000`


#### Solution - deque

```c++
​class Solution {
public:
    vector<int> levelOrder(TreeNode* root)
    {
        if (!root)
        {
            return {};
        }
        deque<TreeNode*> nodes;

        nodes.push_back(root);

        vector<int> result;
        while (!nodes.empty())
        {
            TreeNode* node = nodes.front();
            nodes.pop_front();

            result.push_back(node->val);

            if (node->left)
            {
                nodes.push_back(node->left);
            }
            if (node->right)
            {
                nodes.push_back(node->right);
            }
        }
        return result;
    }
};
```

### [剑指 Offer 32 - II. 从上到下打印二叉树 II](https://leetcode-cn.com/problems/cong-shang-dao-xia-da-yin-er-cha-shu-ii-lcof/)

Difficulty: **简单**


从上到下按层打印二叉树，同一层的节点按从左到右的顺序打印，每一层打印到一行。

例如:  
给定二叉树: `[3,9,20,null,null,15,7]`,

```
    3
   / \
  9  20
    /  \
   15   7
```

返回其层次遍历结果：

```
[
  [3],
  [9,20],
  [15,7]
]
```

**提示：**

1.  `节点总数 <= 1000`

注意：本题与主站 102 题相同：


#### Solution - 使用nullptr标记

```c++
class Solution {
public:
    vector<vector<int>> levelOrder(TreeNode* root)
    {
        if (!root)
        {
            return {};
        }

        deque<TreeNode*> nodes;

        nodes.push_back(root);
        nodes.push_back(nullptr);

        vector<vector<int>> result;
        result.push_back(vector<int>());

        while (!nodes.empty())
        {
            TreeNode* node = nodes.front();
            nodes.pop_front();

            if (node)
            {
                result.back().push_back(node->val);
                if (node->left)
                {
                    nodes.push_back(node->left);
                }
                if (node->right)
                {
                    nodes.push_back(node->right);
                }
            }
            else
            {
                if (!nodes.empty())
                {
                    result.push_back(vector<int>());
                    nodes.push_back(nullptr);
                }
            }
        }

        return result;
    }
};
```
首先想出的就是这个方法

#### - 使用for循环 提前保存变量解决size会改变的问题

```c++
class Solution {
public:
    vector<vector<int>> levelOrder(TreeNode* root)
    {
        if (!root)
        {
            return {};
        }

        deque<TreeNode*> nodes;

        nodes.push_back(root);

        vector<vector<int>> result;

        while (!nodes.empty())
        {
            result.push_back(vector<int>());

            int ss = nodes.size();
            for (int i = 0; i < ss; ++i)
            {
                TreeNode* node = nodes.front();
                nodes.pop_front();

                result.back().push_back(node->val);
                if (node->left)
                {
                    nodes.push_back(node->left);
                }
                if (node->right)
                {
                    nodes.push_back(node->right);
                }
            }
        }

        return result;
    }
};
```

写完S1就去看了看题解 没想到还有这种方法, 妙蛙种子在米奇妙妙屋吃妙脆角

### [剑指 Offer 32 - III. 从上到下打印二叉树 III](https://leetcode-cn.com/problems/cong-shang-dao-xia-da-yin-er-cha-shu-iii-lcof/)

Difficulty: **中等**


请实现一个函数按照之字形顺序打印二叉树，即第一行按照从左到右的顺序打印，第二层按照从右到左的顺序打印，第三行再按照从左到右的顺序打印，其他行以此类推。

例如:  
给定二叉树: `[3,9,20,null,null,15,7]`,

```
    3
   / \
  9  20
    /  \
   15   7
```

返回其层次遍历结果：

```
[
  [3],
  [20,9],
  [15,7]
]
```

**提示：**

1.  `节点总数 <= 1000`


#### Solution - 设置标志位

```c++
class Solution {
public:
    vector<vector<int>> levelOrder(TreeNode* root) 
    {
        if (!root)
        {
            return {};
        }

        deque<TreeNode*> nodes;
        nodes.push_back(root);

        bool right = true;

        vector<vector<int>> result;

        while (!nodes.empty())
        {
            result.emplace_back();

            int SS = nodes.size();

            if (right)
            {
                right = false;
                for (int i = 0; i < SS; ++i)
                {
                    TreeNode* node = nodes.front();
                    nodes.pop_front();
                    result.back().push_back(node->val);

                    if (node->left)
                    {
                        nodes.push_back(node->left);
                    }
                    if (node->right)
                    {
                        nodes.push_back(node->right);
                    }
                }
            }
            else
            {
                right = true;
                for (int i = SS - 1; i >= 0; --i)
                {
                    TreeNode* node = nodes.back();
                    nodes.pop_back();
                    result.back().push_back(node->val);
                    
                    if (node->right)
                    {
                        nodes.push_front(node->right);
                    }
                    if (node->left)
                    {
                        nodes.push_front(node->left);
                    }
                }
            }
        }
        return result;
    }
};​
```

#### Solution - 使用reverse

```c++
class Solution {
public:
    vector<vector<int>> levelOrder(TreeNode* root) 
    {
        if (!root)
        {
            return {};
        }

        deque<TreeNode*> nodes;
        nodes.push_back(root);

        vector<vector<int>> result;
        bool even = false;

        while (!nodes.empty())
        {
            result.emplace_back();

            int SS = nodes.size();

            vector<int>& tmp = result.back();
            for (int i = 0; i < SS; ++i)
            {
                TreeNode* node = nodes.front();
                nodes.pop_front();
                tmp.push_back(node->val);

                if (node->left)
                {
                    nodes.push_back(node->left);
                }
                if (node->right)
                {
                    nodes.push_back(node->right);
                }
            }

            if (even)
            {
                std::reverse(tmp.begin(), tmp.end());
            }
            even = !even;
        }
        return result;
    }
};
```

### [剑指 Offer 33\. 二叉搜索树的后序遍历序列](https://leetcode-cn.com/problems/er-cha-sou-suo-shu-de-hou-xu-bian-li-xu-lie-lcof/)

Difficulty: **中等**


输入一个整数数组，判断该数组是不是某二叉搜索树的后序遍历结果。如果是则返回 `true`，否则返回 `false`。假设输入的数组的任意两个数字都互不相同。

参考以下这颗二叉搜索树：

```
     5
    / \
   2   6
  / \
 1   3
```

**示例 1：**

```
输入: [1,6,3,2,5]
输出: false
```

**示例 2：**

```
输入: [1,3,2,6,5]
输出: true
```

**提示：**

1.  `数组长度 <= 1000`


#### Solution - 递归

```c++
​class Solution {
public:
    bool verifyPostorder(vector<int>& postorder)
    {
        return verifyPostorder(postorder, 0, postorder.size());
    }

    bool verifyPostorder(vector<int>& postorder, int begin, int end)
    {
        if (end - begin <= 2)
        {
            return true;
        }

        int root_sub = end - 1;
        int r_begin = begin;
        for (int i = begin; i < root_sub; ++i)
        {
            if (postorder[i] < postorder[root_sub])
            {
                r_begin++;
            }
            else
            {
                break;
            }
        }

        for (int i = r_begin; i < root_sub; ++i)
        {
            if (postorder[i] < postorder[root_sub])
            {
                return false;
            }
        }
        return verifyPostorder(postorder, begin, r_begin) && 
            verifyPostorder(postorder, r_begin, root_sub);
    }
};
```

睡前来一道, 这是我第一次 一次通过的剑指里面的中等题目-_- 睡了睡了

#### Solution - 迭代 栈


### [剑指 Offer 34\. 二叉树中和为某一值的路径](https://leetcode-cn.com/problems/er-cha-shu-zhong-he-wei-mou-yi-zhi-de-lu-jing-lcof/)

Difficulty: **中等**


输入一棵二叉树和一个整数，打印出二叉树中节点值的和为输入整数的所有路径。从树的根节点开始往下一直到叶节点所经过的节点形成一条路径。

**示例:**  
给定如下二叉树，以及目标和 `sum = 22`，

```
              5
             / \
            4   8
           /   / \
          11  13  4
         /  \    / \
        7    2  5   1
```

返回:

```
[
   [5,4,11,2],
   [5,8,4,5]
]
```

**提示：**

1.  `节点总数 <= 10000`

注意：本题与主站 113 题相同：


#### Solution - 回溯法

```c++
​class Solution {
public:

    vector<int> path;
    vector<vector<int>> result;

    vector<vector<int>> pathSum(TreeNode* root, int sum)
    {
        dfs(root, sum);

        return result; 
    }

    void dfs(TreeNode* root, int sum)
    {
        if (!root)
        {
            return;
        }

        path.push_back(root->val);
        if (sum == root->val && !root->left && !root->right)
        {
            result.push_back(path);
        }
        else
        {
            dfs(root->left, sum - root->val);
            dfs(root->right, sum - root->val);
        }
        path.pop_back();
    }
};
```



### [剑指 Offer 37\. 序列化二叉树](https://leetcode-cn.com/problems/xu-lie-hua-er-cha-shu-lcof/)

Difficulty: **困难**


请实现两个函数，分别用来序列化和反序列化二叉树。

**示例: **

```
你可以将以下二叉树：

    1
   / \
  2   3
     / \
    4   5

序列化为 "[1,2,3,null,null,4,5]"
```

注意：本题与主站 297 题相同：


#### Solution


```c++
class Codec {
public:

    // Encodes a tree to a single string.
    string serialize(TreeNode* root)
    {
        if (!root)
        {
            return "[null]";
        }

        deque<TreeNode*> nodes;
        nodes.push_back(root);

        string result;
        while (!nodes.empty())
        {
            TreeNode* node = nodes.front();
            nodes.pop_front();
            if (node)
            {
                result += "," + to_string(node->val);
                nodes.push_back(node->left);
                nodes.push_back(node->right);
            }
            else
            {
                result += ",null";
            }
        }

        result[0] = '[';
        result += ']';
        return result;
    }

    // Decodes your encoded data to tree.
    TreeNode* deserialize(string data)
    {
        deque<TreeNode*> nodes;
        ParseData(data, nodes);

        TreeNode* ret = nodes.front();
        nodes.pop_front();

        if (nodes.size() == 0)
        {
            return ret;
        }
        
        deque<TreeNode*> temp_nodes;
        temp_nodes.push_back(ret);

        while (!temp_nodes.empty())
        {
            const int SS = temp_nodes.size();

            if (nodes.size() < SS * 2)
            {
                break;
            }

            for (int i = 0; i < SS; ++i)
            {
                TreeNode* node = temp_nodes.front();
                temp_nodes.pop_front();

                node->left = nodes.front();
                nodes.pop_front();
                node->right = nodes.front();
                nodes.pop_front();

                if (node->left)
                {
                    temp_nodes.push_back(node->left);
                }
                if (node->right)
                {
                    temp_nodes.push_back(node->right);
                }
            }
        }

        return ret;
    }

    void ParseData(const string& data, deque<TreeNode*>& nodes)
    {
        int i = 1;

        int num = 0;
        bool empty = false;
        int flag = 1; // -

        while (i < data.size())
        {
            char c = data[i++];

            if (c >= '0' && c <= '9')
            {
                num = num * 10 + c - '0';
            }
            else if (c == ',' || c == ']')
            {
                if (empty)
                {
                    nodes.push_back(nullptr);
                    empty = false;
                }
                else
                {
                    nodes.push_back(new TreeNode(num * flag));
                    num = 0;
                    flag = 1;
                }
            }
            else if (c == 'n')
            {
                empty = true;
            }
            else if (c == '-')
            {
                flag = -1;
            }
        }
    }
};​
```

### [剑指 Offer 38\. 字符串的排列](https://leetcode-cn.com/problems/zi-fu-chuan-de-pai-lie-lcof/) - 30MIN

Difficulty: **中等**


输入一个字符串，打印出该字符串中字符的所有排列。

你可以以任意顺序返回这个字符串数组，但里面不能有重复元素。

**示例:**

```
输入：s = "abc"
输出：["abc","acb","bac","bca","cab","cba"]
```

**限制：**

`1 <= s 的长度 <= 8`


#### Solution

```c++
​class Solution {
public:

    vector<string> result;

    vector<string> permutation(string s)
    {
        std::string temp;
        temp.resize(s.size());
        
        permutation(s, 0, temp);
        return result;
    }

    void permutation(string& s, int sub, std::string& temp)
    {
        if (sub == s.size())
        {
            result.push_back(temp);
            return;
        }

        char flag['z' - 'A' + 1]{};
        for (int i = 0; i < s.size(); ++i)
        {
            char back = s[i];
            if (back != '#')
            {
                if (flag[back - 'A'] == 0)
                {
                    flag[back - 'A'] = 1;
                    temp[sub] = s[i];

                    s[i] = '#';
                    permutation(s, sub + 1, temp);
                    s[i] = back;
                }
            }
        }
    }
};
```

代码中防止重复的代码太巧了, 自己一开始使用的set去重. 然后看到了题解的剪枝方法 在每一层中一个字母仅能出现一次, 时间消耗大幅下降了

#### Solution - 改进版 - 源字符串上swap

```c++
class Solution {
public:

    vector<string> result;

    vector<string> permutation(string s)
    {        
        permutation(s, 0);
        return result;
    }

    void permutation(string& s, int sub)
    {
        if (sub == s.size() - 1) // 仅剩一个字符 该字符正好位于末尾 直接保存
        {
            result.push_back(s);
            return;
        }

        char flag['z' - 'A' + 1]{};
        for (int i = sub; i < s.size(); ++i) // 从本层开始 sub之前的字符已经被使用了
        {
            char back = s[i];

            if (flag[back - 'A'] == 0)
            {
                flag[back - 'A'] = 1;

                swap(s[i], s[sub]); // 交换未使用元素到本层位置
                permutation(s, sub + 1);
                swap(s[i], s[sub]);
            }
        }
    }
};
```

由于是全排列, 其实可以将未使用元素挨个的交换到本层位置 这样本层位置之前的字符是使用过的.

### [剑指 Offer 39\. 数组中出现次数超过一半的数字](https://leetcode-cn.com/problems/shu-zu-zhong-chu-xian-ci-shu-chao-guo-yi-ban-de-shu-zi-lcof/) - 20MIN

Difficulty: **简单**


数组中有一个数字出现的次数超过数组长度的一半，请找出这个数字。

你可以假设数组是非空的，并且给定的数组总是存在多数元素。

**示例 1:**

```
输入: [1, 2, 3, 2, 2, 2, 5, 4, 2]
输出: 2
```

**限制：**

`1 <= 数组长度 <= 50000`

注意：本题与主站 169 题相同：


#### Solution - Map统计次数

```c++
​int majorityElement(vector<int>& nums)
{
    map<int, int> times;

    int end_time = nums.size() / 2;

    for (auto& num : nums)
    {
        times[num]++;
        if (times[num] > end_time)
        {
            return num;
        }
    }
    return 0;
}
```

#### Solution - sort排序 中间位置元素为结果

```c++
int majorityElement(vector<int>& nums)
{
    sort(nums.begin(), nums.end());

    return nums[nums.size() / 2];
}
```

#### Solution - 摩尔投票法

```c++
int majorityElement(vector<int>& nums)
{
    int vote = nums[0];
    int sum = 0;

    for (auto& num : nums)
    {
        sum += vote == num ? 1 : -1;
        if (sum < 0)
        {
            vote = num;
            sum = 1;
        }
    }

    // int times = 0;
    // for (auto& num : nums)
    // {
    //     times += num == vote ? 1 : 0;
    // }
    // 如果可能不存在结果 则需要进行验证
    return vote;
}
```

### [剑指 Offer 40\. 最小的k个数](https://leetcode-cn.com/problems/zui-xiao-de-kge-shu-lcof/)

Difficulty: **简单**


输入整数数组 `arr` ，找出其中最小的 `k` 个数。例如，输入4、5、1、6、2、7、3、8这8个数字，则最小的4个数字是1、2、3、4。

**示例 1：**

```
输入：arr = [3,2,1], k = 2
输出：[1,2] 或者 [2,1]
```

**示例 2：**

```
输入：arr = [0,1,2,1], k = 1
输出：[0]
```

**限制：**

*   `0 <= k <= arr.length <= 10000`
*   `0 <= arr[i] <= 10000`


#### Solution - 维护最大堆 O(nlogk) O(k)

```c++
​class Solution {
public:

    priority_queue<int> min_heap;

    void TryToAdd(int num, int k)
    {
        if (min_heap.size() < k)
        {
            min_heap.push(num);
        }
        else
        {
            if (num < min_heap.top())
            {
                min_heap.pop();
                min_heap.push(num);
            }
        }
    }

    vector<int> getLeastNumbers(vector<int>& arr, int k)
    {
        if (k <= 0)
        {
            return {};
        }

        for (auto num : arr)
        {
            TryToAdd(num, k);
        }

        vector<int> result(k);
        for (int i = 0; i < k; ++i)
        {
            result[i] = min_heap.top();
            min_heap.pop();
        }
        return result;
    }
};
```

#### Solution - 基于快排分区思想 O(n) O(logn)

```c++
class Solution {
public:

    vector<int> getLeastNumbers(vector<int>& arr, int k)
    {
        if (k == 0)
        {
            return {};
        }
        Partition(arr, 0, arr.size() - 1, k);

        return vector<int>(arr.begin(), arr.begin() + k);
    }

    int Partition(vector<int>& arr, int left , int right, int k)
    {
        if (right - left <= 0)
        {
            return left;
        }
        int ret = left;

        int index = ret + 1;

        for (int i = index; i <= right; ++i)
        {
            if (arr[i] < arr[ret])
            {
                swap(arr[index], arr[i]);
                index++;
            }
        }
        swap(arr[index - 1], arr[ret]);
        int part = index - 1;

        if (part == k)
        {
            return part;
        }
        else if (part < k)
        {
            return Partition(arr, part + 1, right, k);
        }
        else
        {
            return Partition(arr, left, part - 1, k);
        }
    }
};
```

### [剑指 Offer 41\. 数据流中的中位数](https://leetcode-cn.com/problems/shu-ju-liu-zhong-de-zhong-wei-shu-lcof/) - 30MIN

Difficulty: **困难**


如何得到一个数据流中的中位数？如果从数据流中读出奇数个数值，那么中位数就是所有数值排序之后位于中间的数值。如果从数据流中读出偶数个数值，那么中位数就是所有数值排序之后中间两个数的平均值。

例如，

[2,3,4] 的中位数是 3

[2,3] 的中位数是 (2 + 3) / 2 = 2.5

设计一个支持以下两种操作的数据结构：

*   void addNum(int num) - 从数据流中添加一个整数到数据结构中。
*   double findMedian() - 返回目前所有元素的中位数。

**示例 1：**

```
输入：
["MedianFinder","addNum","addNum","findMedian","addNum","findMedian"]
[[],[1],[2],[],[3],[]]
输出：[null,null,null,1.50000,null,2.00000]
```

**示例 2：**

```
输入：
["MedianFinder","addNum","findMedian","addNum","findMedian"]
[[],[2],[],[3],[]]
输出：[null,null,2.00000,null,2.50000]
```

**限制：**

*   最多会对 `addNum、findMedian` 进行 `50000` 次调用。

注意：本题与主站 295 题相同：


#### Solution 最大堆 + 最小堆 求中位数

```c++
​class MedianFinder {
public:

    priority_queue<int, vector<int>, greater<int>> min_heap;
    priority_queue<int> max_heap;
    int size = 0;

    /** initialize your data structure here. */
    MedianFinder() {
        
    }
    
    void addNum(int num)
    {
        if (size == 0)
        {
            min_heap.push(num);
        }
        else
        {
            if (size % 2 == 0)
            {
                if (num > max_heap.top())
                {
                    min_heap.push(num);
                }
                else
                {
                    max_heap.push(num);
                    min_heap.push(max_heap.top());
                    max_heap.pop();
                }
            }
            else
            {
                if (num < min_heap.top())
                {
                    max_heap.push(num);
                }
                else
                {
                    min_heap.push(num);
                    max_heap.push(min_heap.top());
                    min_heap.pop();
                }
            }
        }
        size++;
    }
    
    double findMedian()
    {
        if (size % 2 == 0)
        {
            return (max_heap.top() + min_heap.top()) / 2.0;
        }
        else
        {
            return min_heap.top();
        }
    }
};
```

最开始写的时候堆的调整写在了findMedian里面, 实际应该写在addNum这样函数的功能才分工明确 而且由于两个堆大小基本平衡效率更高


### [剑指 Offer 42\. 连续子数组的最大和](https://leetcode-cn.com/problems/lian-xu-zi-shu-zu-de-zui-da-he-lcof/) - 5MIN

Difficulty: **简单**


输入一个整型数组，数组中的一个或连续多个整数组成一个子数组。求所有子数组的和的最大值。

要求时间复杂度为O(n)。

**示例1:**

```
输入: nums = [-2,1,-3,4,-1,2,1,-5,4]
输出: 6
解释: 连续子数组 [4,-1,2,1] 的和最大，为 6。
```

**提示：**

*   `1 <= arr.length <= 10^5`
*   `-100 <= arr[i] <= 100`

注意：本题与主站 53 题相同：


#### Solution

```c++
​class Solution {
public:
    int maxSubArray(vector<int>& nums)
    {
        int sum = 0;
        int ret = nums[0];
        for (auto& num : nums)
        {
            if (sum >= 0)
            {
                sum += num;
            }
            else
            {
                sum = num;
            }
            ret = max(sum, ret);
        }
        return ret;
    }
};
```

### [剑指 Offer 45\. 把数组排成最小的数](https://leetcode-cn.com/problems/ba-shu-zu-pai-cheng-zui-xiao-de-shu-lcof/)

Difficulty: **中等**


输入一个非负整数数组，把数组里所有数字拼接起来排成一个数，打印能拼接出的所有数字中最小的一个。

**示例 1:**

```
输入: [10,2]
输出: "102"
```

**示例 2:**

```
输入: [3,30,34,5,9]
输出: "3033459"
```

**提示:**

*   `0 < nums.length <= 100`

**说明:**

*   输出结果可能非常大，所以你需要返回一个字符串而不是整数
*   拼接起来的数字可能会有前导 0，最后结果不需要去掉前导 0


#### Solution - 遇事不决dfs 超时..

```c++
​class Solution {
public:

    string minNumber(vector<int>& nums)
    {
        string result;
        string temp;
        dfs(nums, 0, result, temp);
        return result;
    }

    void dfs(vector<int>& nums, int sub, string& result, string& temp)
    {
        if (!result.empty() && result.compare(temp) < 0) // 剪枝
        {
            return;
        }

        if (sub == nums.size())
        {
            if (result.empty() || result.compare(temp) > 0)
            {
                result = temp;
            }
            return;
        }

        set<int> exist;
        for (int i = sub; i < nums.size(); ++i)
        {
            auto iter = exist.lower_bound(nums[i]); // 剪枝
            if (iter == exist.end() || *iter != nums[i])
            {
                exist.insert(iter, nums[i]);

                swap(nums[i], nums[sub]);

                string num = to_string(nums[sub]);
                temp += num;

                dfs(nums, sub + 1, result, temp);

                temp.erase(temp.end() - num.size(), temp.end());

                swap(nums[i], nums[sub]);
            }
        }
    }
};
```

#### Solution - 特殊的排序(另类的comp函数)

```c++
class Solution {
public:

    void QuickSort(vector<string>& strs, int left, int right)
    {
        if (right - left <= 0)
        {
            return;
        }

        auto comp =
            [](const string& lhs, const string& rhs)
            {
                return lhs + rhs < rhs + lhs;
            };

        int pivot = left;

        int index = left + 1;
        for (int i = index; i <= right; ++i)
        {
            if (comp(strs[i], strs[pivot]))
            {
                swap(strs[i], strs[index]);
                index++;
            }
        }
        swap(strs[pivot], strs[index - 1]);

        // int le = left;
        // int ri = right;

        // while (le < ri)
        // {
        //     while (le < ri && !comp(strs[ri], strs[pivot]))
        //     {
        //         ri--;
        //     }
        //     while (le < ri && comp(strs[le], strs[pivot]))
        //     {
        //         le++;
        //     }
        //     swap(strs[le], strs[ri]);
        // }
        // swap(strs[le], strs[pivot]);

        QuickSort(strs, left, index - 2);
        QuickSort(strs, index, right);
    }

    string minNumber(vector<int>& nums)
    {
        vector<string> strs;
        strs.reserve(nums.size());

        for (int num : nums)
        {
            strs.push_back(to_string(num));
        }
    
        QuickSort(strs, 0, strs.size() - 1);

        string result;
        for (const auto& str : strs)
        {
            result += str;
        }
        return result;
    }
};
```

### [剑指 Offer 46\. 把数字翻译成字符串](https://leetcode-cn.com/problems/ba-shu-zi-fan-yi-cheng-zi-fu-chuan-lcof/)

Difficulty: **中等**


给定一个数字，我们按照如下规则把它翻译为字符串：0 翻译成 “a” ，1 翻译成 “b”，……，11 翻译成 “l”，……，25 翻译成 “z”。一个数字可能有多个翻译。请编程实现一个函数，用来计算一个数字有多少种不同的翻译方法。

**示例 1:**

```
输入: 12258
输出: 5
解释: 12258有5种不同的翻译，分别是"bccfi", "bwfi", "bczi", "mcfi"和"mzi"
```

**提示：**

*   `0 <= num < 2<sup>31</sup>`


#### Solution - 遇事不决dfs

```c++
​class Solution {
public:
    int translateNum(int num)
    {
        string num_str = to_string(num);

        return dfs(num_str, 0);
    }

    int dfs(const string& str, int sub)
    {
        if (sub > str.size())
        {
            return 0;
        }
        else if (sub >= str.size() - 1)
        {
            return 1;
        }

        int time = 0;
        if (sub < str.size() - 1 && str[sub] == '1')
        {
            time += dfs(str, sub + 2);
        }
        else if (sub < str.size() - 1 && str[sub] == '2' && 
                str[sub + 1] >= '0' && str[sub + 1] <= '5')
        {
            time += dfs(str, sub + 2);
        }

        time += dfs(str, sub + 1);

        return time;
    }
};
```

#### Solution - DP

```c++
class Solution {
public:
    int translateNum(int num)
    {
        string num_str = to_string(num);

        vector<int> dp(num_str.size() + 1);
        dp[0] = 1;
        dp[1] = 1;

        for (int i = 2; i <= num_str.size(); ++i)
        {
            dp[i] = dp[i - 1];

            if (num_str[i - 2] == '1')
            {
                dp[i] += dp[i - 2];
            }
            else if (num_str[i - 2] == '2' && num_str[i - 1] >= '0' && num_str[i - 1] <= '5')
            {
                dp[i] += dp[i - 2];
            }
        }
        return dp[num_str.size()];
    }
};
```

可以对dp数组进行降维打击 易读性也被降维打击了
```c++
class Solution {
public:
    int translateNum(int num)
    {
        string num_str = to_string(num);

        int a = 1; // i - 2
        int b = 1; // i - 1
        int dp = b; // i

        for (int i = 2; i <= num_str.size(); ++i)
        {
            dp = b;

            if (num_str[i - 2] == '1')
            {
                dp += a;
            }
            else if (num_str[i - 2] == '2' && num_str[i - 1] >= '0' && num_str[i - 1] <= '5')
            {
                dp += a;
            }
            a = b;
            b = dp;
        }
        return dp;
    }
};
```

`dp[0]=1`这点没有考虑到, 本来设置的为0. 

然而当num为25的时候`dp[2]`应该为2, 所以必须把`dp[0]`设置为1

状态转移方程`dp[i] = dp[i - 1] + dp[i - 2](i-1和i-2字符需要能连接)` 是因为如果把`i-1和i-2`连接有`dp[i-2]`种

如果不连接则有`dp[i - 1]`种 所以最终是二者之和


可以对str字符串进行降维打击 易读性又双被降维打击了

```c++
class Solution {
public:
    int translateNum(int num)
    {
        int a = 1; // i - 2
        int b = 1; // i - 1
        int dp = b; // i
        
        int last_s = num % 10;
        int now_s;
        num /= 10;

        while (num != 0)
        {
            now_s = num % 10;
            num /= 10;

            dp = b;

            int temp = now_s * 10 + last_s;
            if (temp >= 10 && temp <= 25)
            {
                dp += a;
            }

            a = b;
            b = dp;

            last_s = now_s;
        }
        return dp;
    }
};
```

### [剑指 Offer 47\. 礼物的最大价值](https://leetcode-cn.com/problems/li-wu-de-zui-da-jie-zhi-lcof/)

Difficulty: **中等**


在一个 m*n 的棋盘的每一格都放有一个礼物，每个礼物都有一定的价值（价值大于 0）。你可以从棋盘的左上角开始拿格子里的礼物，并每次向右或者向下移动一格、直到到达棋盘的右下角。给定一个棋盘及其上面的礼物的价值，请计算你最多能拿到多少价值的礼物？

**示例 1:**

```
输入: 
[
  [1,3,1],
  [1,5,1],
  [4,2,1]
]
输出: 12
解释: 路径 1→3→5→2→1 可以拿到最多价值的礼物
```

提示：

*   `0 < grid.length <= 200`
*   `0 < grid[0].length <= 200`


#### Solution - 备忘录 接近超时

```c++
​class Solution {
public:
    int maxValue(vector<vector<int>>& grid)
    {
        return dfs(grid, 0, 0);
    }

    std::map<std::string, int> bwl;
    int dfs(const vector<vector<int>>& grid, int x, int y)
    {
        if (x == grid.size() - 1 && y == grid[0].size() - 1)
        {
            return grid[x][y];
        }

        std::string key = to_string(x) + "," + to_string(y);
        auto iter = bwl.find(key);
        if (iter != bwl.end())
        {
            return iter->second;
        }

        int r = 0;
        int d = 0;
        if (x < grid.size() - 1 && y < grid[0].size())
        {
            r = dfs(grid, x + 1, y);
        }
        if (y < grid[0].size() - 1 && x < grid.size())
        {
            d = dfs(grid, x, y + 1);
        }

        int result = max(r, d) + grid[x][y];
        bwl[key] = result;
        return result;
    }
};
```

#### Solution - dp table 接近100%

```c++
class Solution {
public:
    int maxValue(vector<vector<int>>& grid)
    {
        const int XS = grid.size();
        const int YS = grid[0].size();

        vector<vector<int>> dp(XS + 1, vector<int>(YS + 1, 0));

        for (int x = 1; x <= XS; ++x)
        {
            for (int y = 1; y <= YS; ++y)
            {
                dp[x][y] = max(dp[x - 1][y], dp[x][y - 1]) + grid[x - 1][y - 1];
            }
        }
        return dp[XS][YS];
    }
};
```

### [剑指 Offer 48\. 最长不含重复字符的子字符串](https://leetcode-cn.com/problems/zui-chang-bu-han-zhong-fu-zi-fu-de-zi-zi-fu-chuan-lcof/)

Difficulty: **中等**


请从字符串中找出一个最长的不包含重复字符的子字符串，计算该最长子字符串的长度。

**示例 1:**

```
输入: "abcabcbb"
输出: 3 
解释: 因为无重复字符的最长子串是 "abc"，所以其长度为 3。
```

**示例 2:**

```
输入: "bbbbb"
输出: 1
解释: 因为无重复字符的最长子串是 "b"，所以其长度为 1。
```

**示例 3:**

```
输入: "pwwkew"
输出: 3
解释: 因为无重复字符的最长子串是 "wke"，所以其长度为 3。
     请注意，你的答案必须是 子串 的长度，"pwke" 是一个子序列，不是子串。
```

提示：

*   `s.length <= 40000`

注意：本题与主站 3 题相同：


#### Solution - 双指针 + hash表

```c++
​class Solution {
public:
    int lengthOfLongestSubstring(string s) 
    {
        if (s.empty())
        {
            return 0;
        }

        int begin = 0;
        int end = 0;
        int hash[128]{};
        for (int i = 0; i < 128; ++i)
        {
            hash[i] = -1;
        }

        int ret = 1;
        for (;end < s.size(); ++end)
        {
            char c = s[end];
            if (hash[c] >= begin)
            {
                int len = end - begin;
                ret = max(len, ret);
                begin = hash[c] + 1;
            }
            hash[c] = end;
        }
        ret = max(end - begin, ret);
        return ret;
    }
};
```

```c++
​class Solution {
public:
    int lengthOfLongestSubstring(string s) 
    {
        int begin = 0;
        int end = 0;
        int hash[128]{};
        for (int i = 0; i < 128; ++i)
        {
            hash[i] = -1;
        }

        int ret = 0;
        for (;end < s.size(); ++end)
        {
            char c = s[end];
            if (hash[c] >= begin)
            {
                begin = hash[c] + 1;
            }
            ret = max(end - begin + 1, ret);
            hash[c] = end;
        }
        return ret;
    }
};
```
简化版, `hash[c] >= begin`主要是更细begin指针, 而更新ret的可以放一起. 原版是end在重复的字符上进行取长度, 新版则是end未在重复字符上 所以需要长度 + 1

#### Solution - Dp + hash表

```c++
class Solution {
public:
    int lengthOfLongestSubstring(string s) 
    {
        int hash[128]{};
        vector<int> dp(s.size() + 1);
        dp[0] = 0;
        int begin = 1;
        int ret = 0;
        for(int i = 1; i <= s.size(); ++i)
        {
            char c = s[i - 1];
            if (hash[c] >= begin)
            {
                dp[i] = i - hash[c];
                begin = hash[c] + 1;
            } 
            else
            {
                dp[i] = dp[i - 1] + 1;
            }
            hash[c] = i;
            ret = max(ret, dp[i]);
        }
        return ret;
    }
};
```

### [剑指 Offer 49\. 丑数](https://leetcode-cn.com/problems/chou-shu-lcof/)

Difficulty: **中等**


我们把只包含质因子 2、3 和 5 的数称作丑数（Ugly Number）。求按从小到大的顺序的第 n 个丑数。

**示例:**

```
输入: n = 10
输出: 12
解释: 1, 2, 3, 4, 5, 6, 8, 9, 10, 12 是前 10 个丑数。
```

**说明: ** 

1.  `1` 是丑数。
2.  `n` **不超过**1690。

注意：本题与主站 264 题相同：


#### Solution - 逐个计算 超时

```c++
​class Solution {
public:
    int nthUglyNumber(int n)
    {
        if (n <= 1)
        {
            return 1;
        }

        unordered_set<int> ugly_num;
        vector<int> bases = {2, 3, 5};
        ugly_num.insert(1);

        int k = 1;
        int num = 1;
        while (k < n)
        {
            num++;
            for (int base : bases)
            {
                if (num % base == 0 && ugly_num.find(num / base) != ugly_num.end())
                {
                    ugly_num.insert(num);
                    k++;
                    break;
                }
            }
        }
        return num;
    }
};
```

#### Solution - 生成丑数

```c++
class Solution {
public:
    int nthUglyNumber(int n)
    {
        int dp[n];
        dp[0] = 1;
        int a = 0, b = 0, c = 0;
        for (int i = 1; i < n; ++i)
        {
            int numa = dp[a] * 2;
            int numb = dp[b] * 3;
            int numc = dp[c] * 5;

            int min_abc = min(numa, min(numb, numc));
            dp[i] = min_abc;
            if (min_abc == numa)
            {
                a++;
            }
            if (min_abc == numb)
            {
                b++;
            }
            if (min_abc == numc)
            {
                c++;
            }
        }
        return dp[n - 1];
    }
};
```

每个新的丑数都是已有丑数的2 3 5倍, 需要按循序将这些丑数排好. 如果一个数字已经提供过了新的丑数, 那么就应该有这个新的丑数尝试去提供, 否则一定是重复的 对应了

代码中的a++ b++ c++, 每次取出最小的丑数添加到结果中


### [剑指 Offer 50\. 第一个只出现一次的字符](https://leetcode-cn.com/problems/di-yi-ge-zhi-chu-xian-yi-ci-de-zi-fu-lcof/)

Difficulty: **简单**


在字符串 s 中找出第一个只出现一次的字符。如果没有，返回一个单空格。 s 只包含小写字母。

**示例:**

```
s = "abaccdeff"
返回 "b"

s = "" 
返回 " "
```

**限制：**

`0 <= s 的长度 <= 50000`


#### Solution

```c++
​class Solution {
public:
    char firstUniqChar(string s)
    {
        if (s.empty())
        {
            return ' ';
        }

        int hash2[26]{};
        for (int i = 0; i < 26; ++i)
        {
            hash2[i] = -1;
        }

        for (int i = 0; i < s.size(); ++i)
        {
            int c = s[i] - 'a';

            if (hash2[c] == -1)
            {
                hash2[c] = i; // 标记第一次出现的位置
            }
            else
            {
                hash2[c] = -2;
            }
        }

        int min_sub = -1;
        int min = INT_MAX;
        for (int i = 0; i < 26; ++i)
        {
            if (hash2[i] >= 0 && hash2[i] < min)
            {
                min = hash2[i];
                min_sub = i;  
            }
        }
        if (min_sub == -1)
        {
            return ' ';
        }

        char result = 'a' + min_sub;
        return result;
    }
};
```

### [剑指 Offer 51\. 数组中的逆序对](https://leetcode-cn.com/problems/shu-zu-zhong-de-ni-xu-dui-lcof/)

Difficulty: **困难**


在数组中的两个数字，如果前面一个数字大于后面的数字，则这两个数字组成一个逆序对。输入一个数组，求出这个数组中的逆序对的总数。

**示例 1:**

```
输入: [7,5,6,4]
输出: 5
```

**限制：**

`0 <= 数组长度 <= 50000`


#### Solution - 归并排序 求逆序对

```c++
class Solution {
public:
    int reversePairs(vector<int>& nums)
    {
        vector<int> temp(nums.size());
        return MergeSort(nums, temp, 0, nums.size() - 1);
    }

    int MergeSort(vector<int>& nums, vector<int>& temp, int left, int right)
    {
        if (left >= right)
        {
            return 0;
        }

        int mid = (left + right) / 2;

        int result = 0;

        result += MergeSort(nums, temp, left, mid);
        result += MergeSort(nums, temp, mid + 1, right);

        int temp_i = left;
        int i = left;
        int j = mid + 1;
        int begin_j = j;
        while (i <= mid && j <= right)
        {
            if (nums[i] <= nums[j])
            {
                temp[temp_i++] = nums[i];
                i++;
                result += (j - begin_j);

            }
            else
            {
                temp[temp_i++] = nums[j];
                j++;
            }
        }
        for (;i <= mid; ++i)
        {
            temp[temp_i++] = nums[i];
            result += (j - begin_j);
        }
        for (; j <= right; ++j)
        {
            temp[temp_i++] = nums[j];
        }
        std::copy(temp.begin() + left, temp.begin() + right + 1, nums.begin() + left);

        return result;
    }
};
```

### [剑指 Offer 52\. 两个链表的第一个公共节点](https://leetcode-cn.com/problems/liang-ge-lian-biao-de-di-yi-ge-gong-gong-jie-dian-lcof/)

Difficulty: **简单**


输入两个链表，找出它们的第一个公共节点。

如下面的两个链表**：**

在节点 c1 开始相交。

**示例 1：**

```
输入：intersectVal = 8, listA = [4,1,8,4,5], listB = [5,0,1,8,4,5], skipA = 2, skipB = 3
输出：Reference of the node with value = 8
输入解释：相交节点的值为 8 （注意，如果两个列表相交则不能为 0）。从各自的表头开始算起，链表 A 为 [4,1,8,4,5]，链表 B 为 [5,0,1,8,4,5]。在 A 中，相交节点前有 2 个节点；在 B 中，相交节点前有 3 个节点。
```

**示例 2：**

```
输入：intersectVal = 2, listA = [0,9,1,2,4], listB = [3,2,4], skipA = 3, skipB = 1
输出：Reference of the node with value = 2
输入解释：相交节点的值为 2 （注意，如果两个列表相交则不能为 0）。从各自的表头开始算起，链表 A 为 [0,9,1,2,4]，链表 B 为 [3,2,4]。在 A 中，相交节点前有 3 个节点；在 B 中，相交节点前有 1 个节点。
```

**示例 3：**

```
输入：intersectVal = 0, listA = [2,6,4], listB = [1,5], skipA = 3, skipB = 2
输出：null
输入解释：从各自的表头开始算起，链表 A 为 [2,6,4]，链表 B 为 [1,5]。由于这两个链表不相交，所以 intersectVal 必须为 0，而 skipA 和 skipB 可以是任意值。
解释：这两个链表不相交，因此返回 null。
```

**注意：**

*   如果两个链表没有交点，返回 `null`.
*   在返回结果后，两个链表仍须保持原有的结构。
*   可假定整个链表结构中没有循环。
*   程序尽量满足 O(_n_) 时间复杂度，且仅用 O(_1_) 内存。
*   本题与主站 160 题相同：


#### Solution - 相遇

```c++
class Solution {
public:
    ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) 
    {
        if (!headA || !headB)
        {
            return nullptr;
        }
        ListNode* temp_a = headA;
        ListNode* temp_b = headB;

        while (temp_a->next || temp_b->next)
        {
            if (temp_a == temp_b)
            {
                return temp_a;
            }
            temp_a = temp_a->next ? temp_a->next : headB;
            
            temp_b = temp_b->next ? temp_b->next : headA; 
        }
        return temp_a == temp_b ? temp_a : nullptr;
    }
};
```

这里踩了一个坑, 我开始是修改了next指针 首先题目不允许修改 其次修改之后会影响另一个指针的判断, 因为修改的这个node很可能是共享的node

```c++
​class Solution {
public:
    ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) 
    {
        ListNode* temp_a = headA;
        ListNode* temp_b = headB;

        while (temp_a != temp_b)
        {
            temp_a = temp_a ? temp_a->next : headB;
            temp_b = temp_b ? temp_b->next : headA; 
        }
        return temp_a;
    }
};
```

退出条件优化了, 两种可能一种是二者相遇了 一种是二者都为空指针 这样代码能简化很多

### [剑指 Offer 53 - II. 0～n-1中缺失的数字](https://leetcode-cn.com/problems/que-shi-de-shu-zi-lcof/)

Difficulty: **简单**


一个长度为n-1的递增排序数组中的所有数字都是唯一的，并且每个数字都在范围0～n-1之内。在范围0～n-1内的n个数字中有且只有一个数字不在该数组中，请找出这个数字。

**示例 1:**

```
输入: [0,1,3]
输出: 2
```

**示例 2:**

```
输入: [0,1,2,3,4,5,6,7,9]
输出: 8
```

**限制：**

`1 <= 数组长度 <= 10000`


#### Solution - 二分法

```c++
​class Solution {
public:
    int missingNumber(vector<int>& nums)
    {
        if (nums[0] != 0)
        {
            return 0;
        }

        int ret = find(nums, 0, nums.size() - 1);
        if (ret == -1)
        {
            return nums[nums.size() - 1] + 1;
        }
        return ret;
    }

    int find(vector<int>& nums, int left, int right)
    {
        if (left >= right)
        {
            return -1;
        }

        int mid = (left + right) / 2;
        if (mid + 1 <= right && nums[mid] == nums[mid + 1] - 2)
        {
            return nums[mid] + 1;
        }
        else if (mid - 1 >= left && nums[mid] == nums[mid - 1] + 2)
        {
            return nums[mid] - 1;
        }
        else
        {
            int ret = find(nums, left, mid - 1);
            if (ret == -1)
            {
                ret = find(nums, mid + 1, right);
            }
            return ret;
        }
    }
};
```

没错写了这么长的二分....  第一印象没有想到迭代版本 如何收缩区间. 索性就两侧都搜索. 实际看了题解才发现

漏掉了一个条件, 当`num[i] > i`的时候就说明了缺失的数字在左边 反过来就是在右边

```c++
class Solution {
public:
    int missingNumber(vector<int>& nums)
    {
        int left = 0;
        int right = nums.size() - 1;

        while (left <= right)
        {
            int mid = (left + right) / 2;
            if (nums[mid] > mid)
            {
                right = mid - 1;
            }
            else if (nums[mid] == mid)
            {
                left = mid + 1;
            }
        }
        return left;
    }
};
```

题解上面还多了一个判断点 循环结束的时候left位于右子数组的开始 right位于左子数组的结束

结果为右子数组开始元素的下标

```c++
0 1 2 3 4 5

0 1 2 3 5 6 // 右子数组 56 左子数组 0 1 2 3  5对应的下标4位答案 
```

### [剑指 Offer 54\. 二叉搜索树的第k大节点](https://leetcode-cn.com/problems/er-cha-sou-suo-shu-de-di-kda-jie-dian-lcof/)

Difficulty: **简单**


给定一棵二叉搜索树，请找出其中第k大的节点。

**示例 1:**

```
输入: root = [3,1,4,null,2], k = 1
   3
  / \
 1   4
  \
   2
输出: 4
```

**示例 2:**

```
输入: root = [5,3,6,2,4,null,null,1], k = 3
       5
      / \
     3   6
    / \
   2   4
  /
 1
输出: 4
```

**限制：**

1 ≤ k ≤ 二叉搜索树元素个数


#### Solution - 中序遍历得到递增序列

```c++
​class Solution {
public:
    int kthLargest(TreeNode* root, int k)
    {
        vector<int> temp;
        dfs(root, k, temp);
        return temp[temp.size() - k];
    }

    void dfs(TreeNode* root, int k, vector<int>& temp)
    {
        if (!root)
        {
            return 0;
        }
        dfs(root->left, k, temp);
        temp.push_back(root->val);
        dfs(root->right, k, temp);
    }
};
```

#### Solution - 先右再左的中序遍历 得到的是递减序列!!

```c++
class Solution {
public:
    int result;
    int kthLargest(TreeNode* root, int k)
    {
        dfs(root, k);
        return result;
    }

    void dfs(TreeNode* root, int& k)
    {
        if (!root)
        {
            return;
        }
        dfs(root->right, k);
        if (k == 0)
        {
            return; // 提前返回这里也非常妙啊
        }
        if (--k == 0)
        {
            result = root->val;
        }
        dfs(root->left, k);
    }
};
```

看到题解这句话直接惊呆我了`先右再左的中序遍历 得到的是递减序列`

还有提前返回的判断

### [剑指 Offer 55 - I. 二叉树的深度](https://leetcode-cn.com/problems/er-cha-shu-de-shen-du-lcof/)

Difficulty: **简单**


输入一棵二叉树的根节点，求该树的深度。从根节点到叶节点依次经过的节点（含根、叶节点）形成树的一条路径，最长路径的长度为树的深度。

例如：

给定二叉树 `[3,9,20,null,null,15,7]`，

```
    3
   / \
  9  20
    /  \
   15   7
```

返回它的最大深度 3 。

**提示：**

1.  `节点总数 <= 10000`

注意：本题与主站 104 题相同：


#### Solution - bfs + nullptr

```c++
​class Solution {
public:
    int maxDepth(TreeNode* root)
    {
        if (!root)
        {
            return 0;
        }

        deque<TreeNode*> nodes;
        nodes.push_back(root);
        nodes.push_back(nullptr);
        int result = 0;

        while (!nodes.empty())
        {
            TreeNode* node = nodes.front();
            nodes.pop_front();

            if (node == nullptr)
            {
                if (!nodes.empty())
                {
                    nodes.push_back(nullptr);
                }
                result++;
                continue;
            }
            if (node->left)
            {
                nodes.push_back(node->left);
            }
            if (node->right)
            {
                nodes.push_back(node->right);
            }
        }
        return result;
    }
};
```

第一印象还是使用了nullptr作为层之间的区分, 前面做过类似的第一印象也是nullptr实际上应该用

for循环更加好 提前取出循环次数, 这样就算更改deque的size也不影响循环次数了

```c++
class Solution {
public:
    int maxDepth(TreeNode* root)
    {
        if (!root)
        {
            return 0;
        }

        deque<TreeNode*> nodes;
        nodes.push_back(root);
        int result = 0;

        while (!nodes.empty())
        {
            result++;
            const int SS = nodes.size();
            for (int i = 0; i < SS; ++i)
            {
                TreeNode* node = nodes.front();
                nodes.pop_front();
                if (node->left)
                {
                    nodes.push_back(node->left);
                }
                if (node->right)
                {
                    nodes.push_back(node->right);
                }
            }
        }
        return result;
    }
};
```

#### dfs 第一个想出来的竟然不是这个...

```c++
class Solution {
public:
    int maxDepth(TreeNode* root)
    {
        if (!root)
        {
            return 0;
        }
        return max(maxDepth(root->left), maxDepth(root->right)) + 1;
    }
};
```

### [剑指 Offer 55 - II. 平衡二叉树](https://leetcode-cn.com/problems/ping-heng-er-cha-shu-lcof/)

Difficulty: **简单**


输入一棵二叉树的根节点，判断该树是不是平衡二叉树。如果某二叉树中任意节点的左右子树的深度相差不超过1，那么它就是一棵平衡二叉树。

**示例 1:**

给定二叉树 `[3,9,20,null,null,15,7]`

```
    3
   / \
  9  20
    /  \
   15   7
```

返回 `true` 。  

**示例 2:**

给定二叉树 `[1,2,2,3,3,null,null,4,4]`

```
       1
      / \
     2   2
    / \
   3   3
  / \
 4   4
```

返回 `false` 。

**限制：**

*   `0 <= 树的结点个数 <= 10000`

注意：本题与主站 110 题相同：


#### Solution

```c++
​class Solution {
public:
    bool isBalanced(TreeNode* root)
    {
        return depth(root) != -1;
    }

    int depth(TreeNode* node)
    {
        if (!node)
        {
            return 0;
        }
        int left = depth(node->left);
        if (left == -1)
        {
            return -1;
        }
 
        int right = depth(node->right);
        if (right == -1)
        {
            return -1;
        }
        if (abs(left - right) > 1)
        {
            return -1;
        }
        return max(left, right) + 1;
    }
};
```

### [剑指 Offer 56 - I. 数组中数字出现的次数](https://leetcode-cn.com/problems/shu-zu-zhong-shu-zi-chu-xian-de-ci-shu-lcof/)

Difficulty: **中等**


一个整型数组 `nums` 里除两个数字之外，其他数字都出现了两次。请写程序找出这两个只出现一次的数字。要求时间复杂度是O(n)，空间复杂度是O(1)。

**示例 1：**

```
输入：nums = [4,1,4,6]
输出：[1,6] 或 [6,1]
```

**示例 2：**

```
输入：nums = [1,2,10,4,1,4,3,3]
输出：[2,10] 或 [10,2]
```

**限制：**

*   `2 <= nums.length <= 10000`


#### Solution - xor 以及xor分组

```c++
​class Solution {
public:
    vector<int> singleNumbers(vector<int>& nums)
    {
        int xor_result = 0;
        for (int num : nums)
        {
            xor_result ^= num;
        }

        int bit_1 = 1;
        while ((xor_result & 1) == 0)
        {
            xor_result = xor_result >> 1;
            bit_1++;
        }

        int xor_1 = 0;
        int xor_2 = 0;
        bit_1--;
        for (int num : nums)
        {
            int bit_1_temp = (num >> bit_1) & 1;

            if (bit_1_temp == 1)
            {
                xor_1 ^= num;
            }
            else
            {
                xor_2 ^= num;
            }
        }
        return {xor_1, xor_2};
    }
};
```

第一印象看到这个题目想到了用xor运算, 然而却没有想到如何将两个不同的数字分开.

看了题解发现 第一次xor出来的`xor_result` 如果是`11101`则说明两个不同的数字的右数第一位不同. 这样通过

其他数字的最后一位是1还是0就能分成两个数组 然后分别xor

```c++
class Solution {
public:
    vector<int> singleNumbers(vector<int>& nums)
    {
        int xor_result = 0;
        for (int num : nums)
        {
            xor_result ^= num;
        }

        int bit_1 = 1;
        while ((xor_result & bit_1) == 0)
        {
            bit_1 = bit_1 << 1; // 通过逻辑左移取位
        }

        int xor_1 = 0;
        int xor_2 = 0;
        for (int num : nums)
        {
            if (num & bit_1) // 这里也变了
            {
                xor_1 ^= num;
            }
            else
            {
                xor_2 ^= num;
            }
        }
        return {xor_1, xor_2};
    }
};
```

### [剑指 Offer 56 - II. 数组中数字出现的次数 II](https://leetcode-cn.com/problems/shu-zu-zhong-shu-zi-chu-xian-de-ci-shu-ii-lcof/)

Difficulty: **中等**


在一个数组 `nums` 中除一个数字只出现一次之外，其他数字都出现了三次。请找出那个只出现一次的数字。

**示例 1：**

```
输入：nums = [3,4,3,3]
输出：4
```

**示例 2：**

```
输入：nums = [9,1,7,9,7,9,7]
输出：1
```

**限制：**

*   `1 <= nums.length <= 10000`
*   `1 <= nums[i] < 2^31`


#### Solution - 二进制位出现次数 % 3

```c++
​class Solution {
public:
    int singleNumber(vector<int>& nums)
    {
        int bits[32]{};

        for (int num : nums)
        {
            int index = 0;
            while (num != 0)
            {
                bits[index] += num & 1;
                num = num >> 1;
                index++;
            }
        }

        int result = 0;
        for (int i = 0; i < 32; ++i)
        {
            result = result << 1;
            result |= bits[31 - i] % 3;
        }
        return result;
    }
};
```

#### Solution - 状态机


### [剑指 Offer 57\. 和为s的两个数字](https://leetcode-cn.com/problems/he-wei-sde-liang-ge-shu-zi-lcof/)

Difficulty: **简单**


输入一个递增排序的数组和一个数字s，在数组中查找两个数，使得它们的和正好是s。如果有多对数字的和等于s，则输出任意一对即可。

**示例 1：**

```
输入：nums = [2,7,11,15], target = 9
输出：[2,7] 或者 [7,2]
```

**示例 2：**

```
输入：nums = [10,26,30,31,47,60], target = 40
输出：[10,30] 或者 [30,10]
```

**限制：**

*   `1 <= nums.length <= 10^5`
*   `1 <= nums[i] <= 10^6`


#### Solution - 双指针 O(n) O(1)

```c++
​class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) 
    {
        int le = 0;
        int ri = nums.size() - 1;

        while (le < ri)
        {
            int sum = nums[le] + nums[ri];
            if (sum == target)
            {
                return {nums[le], nums[ri]};
            }
            else if (sum > target)
            {
                ri--;
            }
            else
            {
                le++;
            }
        }
        return {0, 0};
    }
};
```

#### Solution - 如果题目是无序数组 用Hash表 O(n) O(n)

```c++
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) 
    {
        unordered_set<int> set;

        for (int num : nums)
        {
            int need = target - num;
            
            auto iter = set.find(need);
            if (iter == set.end())
            {
                set.insert(num);
            }
            else
            {
                return {num, *iter};
            }
        }
        return {0, 0};
    }
};
```

### [剑指 Offer 57 - II. 和为s的连续正数序列](https://leetcode-cn.com/problems/he-wei-sde-lian-xu-zheng-shu-xu-lie-lcof/)

Difficulty: **简单**


输入一个正整数 `target` ，输出所有和为 `target` 的连续正整数序列（至少含有两个数）。

序列内的数字由小到大排列，不同序列按照首个数字从小到大排列。

**示例 1：**

```
输入：target = 9
输出：[[2,3,4],[4,5]]
```

**示例 2：**

```
输入：target = 15
输出：[[1,2,3,4,5],[4,5,6],[7,8]]
```

**限制：**

*   `1 <= target <= 10^5`


#### Solution - 滑动窗口

```c++
​class Solution {
public:
    vector<vector<int>> findContinuousSequence(int target) 
    {
        vector<vector<int>> result;
        int begin = 1;
        int end = 2;
        int sum = 3;

        while (end < target && begin != end)
        {
            if (sum == target)
            {
                result.emplace_back();
                vector<int>& vec = result.back();
                vec.reserve(end - begin + 1);
                for (int i = begin; i <= end; ++i)
                {
                    vec.push_back(i);
                }
                end++;
                sum += end;
            }
            else if (sum < target)
            {
                end++;
                sum += end;
            }
            else
            {
                sum -= begin;
                begin++;
            }
        }
        return result;
    }
};
```

### [剑指 Offer 58 - I. 翻转单词顺序](https://leetcode-cn.com/problems/fan-zhuan-dan-ci-shun-xu-lcof/)

Difficulty: **简单**


输入一个英文句子，翻转句子中单词的顺序，但单词内字符的顺序不变。为简单起见，标点符号和普通字母一样处理。例如输入字符串"I am a student. "，则输出"student. a am I"。

**示例 1：**

```
输入: "the sky is blue"
输出: "blue is sky the"
```

**示例 2：**

```
输入: "  hello world!  "
输出: "world! hello"
解释: 输入字符串可以在前面或者后面包含多余的空格，但是反转后的字符不能包括。
```

**示例 3：**

```
输入: "a good   example"
输出: "example good a"
解释: 如果两个单词间有多余的空格，将反转后单词间的空格减少到只含一个。
```

**说明：**

*   无空格字符构成一个单词。
*   输入字符串可以在前面或者后面包含多余的空格，但是反转后的字符不能包括。
*   如果两个单词间有多余的空格，将反转后单词间的空格减少到只含一个。

**注意：**本题与主站 151 题相同：

**注意：**此题对比原题有改动


#### Solution

```c++
​class Solution {
public:
    string reverseWords(string s)
    {
        if (s.empty())
        {
            return "";
        }
        
        int begin = 0;
        int end = s.size();
        while (begin < s.size() && s[begin] == ' ')
        {
            begin++;
        }
        while (end >= 1 && s[end - 1] == ' ')
        {
            end--;
        }
        if (begin >= end)
        {
            return "";
        }
        reverse(s.begin() + begin, s.begin() + end);

        int word_begin = begin;
        for (int i = begin; i < end; ++i)
        {
            if (s[i] == ' ')
            {
                int word_end = i;
                reverse(s.begin() + word_begin, s.begin() + word_end);
                word_begin = i + 1;
            }
        }
        reverse(s.begin() + word_begin, s.begin() + end);

        string result;
        result.reserve(end - begin);
        for (int i = begin; i < end; ++i)
        {
            if (s[i] == ' ')
            {
                if (result[result.size() - 1] != ' ')
                {
                    result.append(1, ' ');
                }
            }
            else
            {
                result.append(1, s[i]);
            }
        }
        return result;
    }
};
```

我用的解法是先翻转整个字符串 然后再把单词翻转过来 这样组成结果

实际上可以直接从尾部遍历, 然后一个单词一个单词的拼接出结果

```c++
class Solution {
public:
    string reverseWords(string s)
    {
        int begin = 0;
        int end = s.size();
        while (begin < s.size() && s[begin] == ' ')
        {
            begin++;
        }
        while (end >= 1 && s[end - 1] == ' ')
        {
            end--;
        }
        if (begin >= end)
        {
            return "";
        }
        string result;
        result.reserve(end - begin);

        int i = end - 1;
        int j = end;
        while (i >= begin)
        {
            while (i >= begin && s[i] != ' ')
            {
                i--;
            }
            result.append(s.begin() + i + 1, s.begin() + j);
            result.append(1, ' '); // 注意这里 第一个参数数量 第二个是字符 反过来也能运行 但是结果出错
            while (i >= begin && s[i] == ' ')
            {
                i--;
            }
            j = i + 1;
        }
        return result.substr(0, result.size() - 1);
    }
};
```

### [剑指 Offer 59 - I. 滑动窗口的最大值](https://leetcode-cn.com/problems/hua-dong-chuang-kou-de-zui-da-zhi-lcof/)

Difficulty: **简单**


给定一个数组 `nums` 和滑动窗口的大小 `k`，请找出所有滑动窗口里的最大值。

**示例:**

```
输入: nums = [1,3,-1,-3,5,3,6,7], 和 k = 3
输出: [3,3,5,5,6,7] 
解释: 

  滑动窗口的位置                最大值
---------------               -----
[1  3  -1] -3  5  3  6  7       3
 1 [3  -1  -3] 5  3  6  7       3
 1  3 [-1  -3  5] 3  6  7       5
 1  3  -1 [-3  5  3] 6  7       5
 1  3  -1  -3 [5  3  6] 7       6
 1  3  -1  -3  5 [3  6  7]      7
```

**提示：**

你可以假设 _k_ 总是有效的，在输入数组不为空的情况下，1 ≤ k ≤ 输入数组的大小。

注意：本题与主站 239 题相同：


#### Solution

```c++
​class Solution {
public:
    vector<int> maxSlidingWindow(vector<int>& nums, int k)
    {
        if (nums.empty())
        {
            return {};
        }
        vector<int> result;
        result.reserve(nums.size() - k + 1);

        int temp_i = 0;
        for (int i = 1; i < k; ++i)
        {
            if (nums[i] > nums[temp_i])
            {
                temp_i = i;
            }
        }
        result.push_back(nums[temp_i]);
        
        int left = 1;
        int right = k;
        while (right < nums.size())
        {
            if (left > temp_i)
            {
                temp_i = left;
                for (int j = left + 1; j <= right; ++j)
                {
                    if (nums[j] > nums[temp_i])
                    {
                        temp_i = j;
                    }
                }
            }
            else
            {
                if (nums[right] > nums[temp_i])
                {
                    temp_i = right;
                }
            }
            result.push_back(nums[temp_i]);
            left++;
            right++;
        }
        return result;
    }
};
```

实际不用在将最大元素移出时进行遍历,  可以采用如下方式, 添加一个deque来解决. 这样能保证获取最大元素为O(1)

```c++
class Solution {
public:
    vector<int> maxSlidingWindow(vector<int>& nums, int k)
    {
        if (nums.empty())
        {
            return {};
        }
        vector<int> result;
        result.reserve(nums.size() - k + 1);

        deque<int> window;
        for (int i = 0; i < k; ++i)
        {
            while (!window.empty() && nums[window.back()] < nums[i])
            {
                window.pop_back();
            }
            window.push_back(i);
        }
        result.push_back(nums[window.front()]);

        for (int i = k; i < nums.size(); ++i)
        {
            if (i - k + 1 > window.front()) // 移除已经滑动出去的值
            {
                window.pop_front();
            }
            
            while (!window.empty() && nums[window.back()] < nums[i]) // 每次添加元素前将小于其的元素移除 保证队列内有序
            {
                window.pop_back();
            }
            window.push_back(i);
            result.push_back(nums[window.front()]);
        }
        return result;
    }
};
```

### [剑指 Offer 59 - II. 队列的最大值](https://leetcode-cn.com/problems/dui-lie-de-zui-da-zhi-lcof/)

Difficulty: **中等**


请定义一个队列并实现函数 `max_value` 得到队列里的最大值，要求函数`max_value`、`push_back` 和 `pop_front` 的**均摊**时间复杂度都是O(1)。

若队列为空，`pop_front` 和 `max_value` 需要返回 -1

**示例 1：**

```
输入: 
["MaxQueue","push_back","push_back","max_value","pop_front","max_value"]
[[],[1],[2],[],[],[]]
输出: [null,null,null,2,1,2]
```

**示例 2：**

```
输入: 
["MaxQueue","pop_front","max_value"]
[[],[],[]]
输出: [null,-1,-1]
```

**限制：**

*   `1 <= push_back,pop_front,max_value的总操作数 <= 10000`
*   `1 <= value <= 10^5`


#### Solution

鉴于上一题所以这一题一开始就想的这种方法, 结果有个地方开始没转过来

如果压入123 那么maxs里面只会保留3 不管是否pop_front掉1和2 最大值只会是3, pop_front掉于最大值前面压入的数不会影响最大值

```c++
​class MaxQueue {
public:
    MaxQueue()
    {

    }
    
    int max_value()
    {
        return maxs.empty() ? -1 : maxs.front();
    }
    
    void push_back(int value)
    {
        nums.push(value);

        while (!maxs.empty() && maxs.back() < value)
        {
            maxs.pop_back();
        }
        maxs.push_back(value);
    }
    
    int pop_front()
    {
        if (nums.empty())
        {
            return -1;
        }
        int ret = nums.front();
        nums.pop();
        if (maxs.front() == ret)
        {
            maxs.pop_front();
        }

        return ret;
    }

    queue<int> nums;
    deque<int> maxs;
};
```

### [剑指 Offer 60\. n个骰子的点数](https://leetcode-cn.com/problems/nge-tou-zi-de-dian-shu-lcof/)

Difficulty: **中等**


把n个骰子扔在地上，所有骰子朝上一面的点数之和为s。输入n，打印出s的所有可能的值出现的概率。

你需要用一个浮点数数组返回答案，其中第 i 个元素代表这 n 个骰子所能掷出的点数集合中第 i 小的那个的概率。

**示例 1:**

```
输入: 1
输出: [0.16667,0.16667,0.16667,0.16667,0.16667,0.16667]
```

**示例 2:**

```
输入: 2
输出: [0.02778,0.05556,0.08333,0.11111,0.13889,0.16667,0.13889,0.11111,0.08333,0.05556,0.02778]
```

**限制：**

`1 <= n <= 11`


#### Solution - 暴力法

```c++
​class Solution {
public:

    vector<int> counter;

    int n_back;

    void dfs(int n, int sum)
    {
        if (n == 0)
        {
            counter[sum - n_back]++;
            return;
        }
        for (int i = 1; i <= 6; ++i)
        {
            dfs(n - 1, sum + i);
        }
    }

    vector<double> dicesProbability(int n) 
    {
        n_back = n;
        counter.resize(n * 6 - n + 1);
        dfs(n, 0);

        double sum_count = pow(6, n);
        vector<double> result;
        result.resize(n *  6 - n + 1);
        for (int i = 0; i < n * 6 - n + 1; ++i)
        {
            result[i] = counter[i] / sum_count;
        }
        return result;
    }
};
```

每个骰子都可能是1-6 将骰子掷完就能得到结果

#### Solution - 动态规划

```c++
​class Solution {
public:

    vector<double> dicesProbability(int n) 
    {
        vector<int> vec1(n * 6 + 1, 0);
        vector<int> vec2(n * 6 + 1, 0);

        for (int i = 1; i <= 6; ++i) // n == 1时 是 1 1 1 1 1 1
        {
            vec1[i] = 1;
        }

        for (int i = 2; i <= n; ++i)
        {
            for (int j = 0; j < i; ++j)
            {
                vec2[j] = 0;
            }

            for (int j = i; j <= i * 6; ++j)
            {
                vec2[j] = 0;
                for (int k = 1; j >= k && k <= 6; ++k)
                {
                    vec2[j] += vec1[j - k];
                }
            }
            vec1.swap(vec2);
        }

        double sum = pow(6, n);
        vector<double> ret;
        ret.reserve(n * 6 - n + 1);
        for (int i = n; i <= n * 6; ++i)
        {
            ret.push_back(vec1[i] / sum);
        }
        return ret;
    }
};
```

当投掷n个骰子的时候出现总和为m  那么 `x[n][m] = x[n-1][m-1] + x[n-1][m-2] + x[n-1][m-3] + x[n-1][m-4] + x[n-1][m-5] + x[n-1][m-6]`

这样每计算n个骰子的时候只需要第n-1轮的结果 可以使用两个数组, 计算完第n轮后将这个数组swap到"上一轮数组"用于计算n+1轮

### [剑指 Offer 61\. 扑克牌中的顺子](https://leetcode-cn.com/problems/bu-ke-pai-zhong-de-shun-zi-lcof/)

Difficulty: **简单**


从扑克牌中随机抽5张牌，判断是不是一个顺子，即这5张牌是不是连续的。2～10为数字本身，A为1，J为11，Q为12，K为13，而大、小王为 0 ，可以看成任意数字。A 不能视为 14。

**示例 1:**

```
输入: [1,2,3,4,5]
输出: True
```

**示例 2:**

```
输入: [0,0,1,2,5]
输出: True
```

**限制：**

数组长度为 5 

数组的数取值为 [0, 13] .


#### Solution

```c++
​class Solution {
public:
    bool isStraight(vector<int>& nums)
    {
        vector<int> vec(13, 0);
        int left_sub = vec.size();
        int zero_count = 0;
        for (int num : nums)
        {
            if (num == 0)
            {
                zero_count++; // 统计0的数量
            }
            else if (vec[num - 1] == 0)
            {
                vec[num - 1] = 1; // 标记出现的非0卡牌
                left_sub = min(left_sub, num - 1); // 更新第一张非0卡牌的开始位置
            }
            else
            {
                return false; // 存在重复的卡牌
            }
        }
        
        int right_sub = left_sub;
        while (right_sub < vec.size() && (vec[right_sub] == 1 || zero_count > 0))
        {
            if (vec[right_sub] == 0) // 为0表示没有这张卡 需要消耗一张0来补齐
            {
                zero_count--;
            }
            right_sub++;
        }

        return right_sub - left_sub + zero_count == nums.size(); // 输入数据的长度 = 顺子的长度(含补齐) + 剩余下的0
    }
};
```

### [剑指 Offer 63\. 股票的最大利润](https://leetcode-cn.com/problems/gu-piao-de-zui-da-li-run-lcof/)

Difficulty: **中等**


假设把某股票的价格按照时间先后顺序存储在数组中，请问买卖该股票一次可能获得的最大利润是多少？

**示例 1:**

```
输入: [7,1,5,3,6,4]
输出: 5
解释: 在第 2 天（股票价格 = 1）的时候买入，在第 5 天（股票价格 = 6）的时候卖出，最大利润 = 6-1 = 5 。
     注意利润不能是 7-1 = 6, 因为卖出价格需要大于买入价格。
```

**示例 2:**

```
输入: [7,6,4,3,1]
输出: 0
解释: 在这种情况下, 没有交易完成, 所以最大利润为 0。
```

**限制：**

`0 <= 数组长度 <= 10^5`

**注意：**本题与主站 121 题相同：


#### Solution - 动态规划

```c++
​class Solution {
public:
    int maxProfit(vector<int>& prices)
    {
        int buy_price = INT_MAX;
        int ans = 0;

        for (int curr_price : prices)
        {
            if (curr_price > buy_price)
            {
                // try sell
                ans = max(ans, curr_price - buy_price); // 当前价格大于卖出价格尝试卖出
            }
            else if (curr_price < buy_price)
            {
                // buy this
                buy_price = curr_price; // 当前价格小于买入价格 尝试改为当前价格买入
            }
        }
        return ans;
    }
};
```

### [剑指 Offer 64\. 求1+2+…+n](https://leetcode-cn.com/problems/qiu-12n-lcof/)

Difficulty: **中等**


求 `1+2+...+n` ，要求不能使用乘除法、for、while、if、else、switch、case等关键字及条件判断语句（A?B:C）。

**示例 1：**

```
输入: n = 3
输出: 6
```

**示例 2：**

```
输入: n = 9
输出: 45
```

**限制：**

*   `1 <= n <= 10000`


#### Solution - sizeof代替乘法 + 短路运算控制返回

```c++
​class Solution {
public:
    // int sumNums(int n)
    // {
    //     char a[n][n + 1];
    //     return (sizeof a) >> 1;
    // }

    int sumNums(int n)
    {
        n > 1 && (n += sumNums(n - 1)); // 短路运算右边对n进行赋值
        return n;
    }
};
```

### [剑指 Offer 65\. 不用加减乘除做加法](https://leetcode-cn.com/problems/bu-yong-jia-jian-cheng-chu-zuo-jia-fa-lcof/)

Difficulty: **简单**


写一个函数，求两个整数之和，要求在函数体内不得使用 “+”、“-”、“*”、“/” 四则运算符号。

**示例:**

```
输入: a = 1, b = 1
输出: 2
```

**提示：**

*   `a`, `b` 均可能是负数或 0
*   结果不会溢出 32 位整数


#### Solution a + b = x(进位信息 使用 & << 1) + y(本位信息 使用 xor)

```c++
​class Solution {
public:
    int add(int a, int b)
    {
        while (b != 0)
        {
            int c = (a & b) << 1; // 进位信息  1 + 1 = 10 由于1&1=1所以需要 <<
            a ^= b; // 本位信息 0 + 0 = 0  1 + 0 = 1 0 + 1 = 0 1 + 1 = 0
            b = c; // a + b = x1(进位) + y1(本位) = x2(进位) + y2(本位) .... + 到进位为0 这样就是结果了 0 + x = x;
        } // 已知a+b = 进位+本位 而进位+本位也是两个数字相加 所以可以连续调用公式 直到进位为0 本位就是结果
        return a;
    }
};
```


### [剑指 Offer 68 - I. 二叉搜索树的最近公共祖先](https://leetcode-cn.com/problems/er-cha-sou-suo-shu-de-zui-jin-gong-gong-zu-xian-lcof/)

Difficulty: **简单**


给定一个二叉搜索树, 找到该树中两个指定节点的最近公共祖先。

中最近公共祖先的定义为：“对于有根树 T 的两个结点 p、q，最近公共祖先表示为一个结点 x，满足 x 是 p、q 的祖先且 x 的深度尽可能大（**一个节点也可以是它自己的祖先**）。”

例如，给定如下二叉搜索树:  root = [6,2,8,0,4,7,9,null,null,3,5]

![](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/14/binarysearchtree_improved.png)

**示例 1:**

```
输入: root = [6,2,8,0,4,7,9,null,null,3,5], p = 2, q = 8
输出: 6 
解释: 节点 2 和节点 8 的最近公共祖先是 6。
```

**示例 2:**

```
输入: root = [6,2,8,0,4,7,9,null,null,3,5], p = 2, q = 4
输出: 2
解释: 节点 2 和节点 4 的最近公共祖先是 2, 因为根据定义最近公共祖先节点可以为节点本身。
```

**说明:**

*   所有节点的值都是唯一的。
*   p、q 为不同节点且均存在于给定的二叉搜索树中。

注意：本题与主站 235 题相同：


#### Solution - 读题 利用好每一个条件

```c++
​/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:

    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q)
    {
        if (!root)
        {
            return nullptr;
        }
        if (p->val > q->val)
        {
            swap(p, q);
        }
        while (true)
        {
            if (root->val < p->val)
            {
                root = root->right;
            }
            else if (root->val > q->val)
            {
                root = root->left;
            }
            else
            {
                break;
            }
        }
        return root;
    }
};
```

我第一次写这个题目没有用上二叉搜索树这个条件, 把下一个题目二叉树给写出来了....

没利用好题目, 没读好题 我有罪

这个题目利用的是二叉搜索树 如果两个数都小于根节点 则位于根节点的左子树 两个都大于则位于根节点的右子树

如果一个大于一个小于说明位于左右子树则当前节点即为答案. 通过保证p的值小于q可以简化以上的判断.  如果最小的p都大于根节点则较大的q也一定大于

