---
title: April LeetCoding Challenge 2021 解题报告
date: 2021-04-01
draft: true
tags:
- leetcode
---


## Overview

[April LeetCoding Challenge 2021](https://leetcode.com/explore/featured/card/april-leetcoding-challenge-2021/)

**Week 1: April 1st - April 7th**

1. Largest Unique Number `Premium`
2. [Palindrome Linked List](#1-Palindrome-Linked-List)
3. [Ones and Zeroes](#2-Ones-and-Zeroes)
4. [Longest Valid Parentheses](#3-Longest-Valid-Parentheses)
5. [Design Circular Queue](#4-Design-Circular-Queue)
6. [Global and Local Inversions](#5-Global-and-Local-Inversions)
7. [Minimum Operations to Make Array Equal](#6-Minimum-Operations-to-Make-Array-Equal)
8. [Determine if String Halves Are Alike](#7-Determine-if-String-Halves-Are-Alike)

**Week 2: April 8th - April 14th**

1. Inorder Successor in BST `Premium`
2. [Letter Combinations of a Phone Number](#8-Letter-Combinations-of-a-Phone-Number)
3. [Verifying an Alien Dictionary](#9-Verifying-an-Alien-Dictionary)
4. [Longest Increasing Path in a Matrix](#10-Longest-Increasing-Path-in-a-Matrix)
5. [Deepest Leaves Sum](#11-Deepest-Leaves-Sum)
6. [Beautiful Arrangement II](#12-Beautiful-Arrangement-II)
7. [Flatten Nested List Iterator](#13-Flatten-Nested-List-Iterator)
8. [Partition List](#14-Partition-List)

**Week 3: April 15th - April 21st**

1. Minimum Swaps to Group All 1's Together `Premium`
2. [Fibonacci Number](#15-Fibonacci-Number)
3. [Remove All Adjacent Duplicates in String II](#16-Remove-All-Adjacent-Duplicates-in-String-II)
4. [Number of Submatrices That Sum to Target](#17-Number-of-Submatrices-That-Sum-to-Target)
5. [Remove Nth Node From End of List](#18-Remove-Nth-Node-From-End-of-List)
6. [Combination Sum IV](#19-Combination-Sum-IV)
7. [N-ary Tree Preorder Traversal](#20-N-ary-Tree-Preorder-Traversal)
8. [Triangle](#21-Triangle)

**Week 4: April 22nd - April 28th**

1. Missing Number In Arithmetic Progression `Premium`
2. [Brick Wall](#22-Brick-Wall)
3. [Count Binary Substrings](#23-Count-Binary-Substrings)
4. Critical Connections in a Network
5. [Rotate Image](#25-Rotate-Image)
6. Furthest Building You Can Reach
7. [Power of Three](#27-Power-of-Three)
8. [Unique Paths II](#28-Unique-Paths-II)

**Week 5: April 29th - April 30th**

1. Meeting Scheduler `Premium`
2. [Find First and Last Position of Element in Sorted Array](#29-Find-First-and-Last-Position-of-Element-in-Sorted-Array)
3. [Powerful Integers](#30-Powerful-Integers)

<br/>

## 1. Palindrome Linked List

[234. Palindrome Linked List](https://leetcode.com/problems/palindrome-linked-list/) `Easy`

**题目简述：**

判断给出的链表是否是回文链表。

**提高：** 能否用 O(n) 时间复杂度与 O(1) 空间复杂度完成？

**题目解答：**

**双指针（快慢指针）**：提高要求其实是个很明显的提示，O(1) 空间复杂度下又要能保留反向结果，只能利用原链表，将右半边做反转即得到反向顺序，因而常量空间为分别代表正向和反向的指针位置。

时间复杂度： $O(n)$ ，空间复杂度： $O(1)$

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public boolean isPalindrome(ListNode head) {
        ListNode ret = new ListNode(0);
        ret.next = head;

        // 左指针每次移动一位，右指针移动两位（如果右指针已经在结尾，左指针不再移动）
        ListNode l = ret, r = ret;
        while(r != null) {
            r = r.next;
            if (r != null) {
                r = r.next;
                l = l.next;
            }

        }

        // 左指针指向中间位置，下一个位置就是右半边的末尾，反转链表
        ListNode last = revese(l.next);
        l.next = null;

        // 比较原链表与反转后链表
        while(head != null && last != null) {
            if (head.val != last.val) {
                return false;
            }
            head = head.next;
            last = last.next;
        }
        return true;
    }

    // 反转链表
    private ListNode revese(ListNode head) {
        ListNode prev = null, node = head;
        while(node != null) {
            ListNode temp = node.next;
            node.next = prev;
            prev = node;
            node = temp;
        }
        return prev;
    }
}
```

<br/>


## 2. Ones and Zeroes

[474. Ones and Zeroes](https://leetcode.com/problems/ones-and-zeroes/) `Medium`

**题目简述：**

给出二进制字符串 `strs`（字符串只包含 0，1）集合和整数 `m`、`n`，求 `strs` 的最大子集长度，要求子集所有字符最多包含 `m` 个 `0` 和 `n` 个 `1` 字符。

**题目解答：**

动态规划，dp[i][j] 表示用 i 个零和 j 个一限制时的最大子集长度。

```java
class Solution {
    public int findMaxForm(String[] strs, int m, int n) {
        int[][] dp = new int[m+1][n+1];
        for(String str : strs) {
            int zeros = countZeroOfStr(str), ones = str.length() - zeros;
            for(int i = m; i >= zeros; i--) {
                for(int j = n; j >= ones; j--) {
                    dp[i][j] = Math.max(dp[i][j], dp[i - zeros][j - ones] + 1);
                }
            }
        }
        return dp[m][n];
    }

    private int countZeroOfStr(String s) {
        int cnt = 0;
        for(char c : s.toCharArray()) {
            cnt += c == '0' ? 1 : 0;
        }
        return cnt;
    }
}
```

<br/>


## 3. Longest Valid Parentheses

[32. Longest Valid Parentheses](https://leetcode.com/problems/longest-valid-parentheses/) `Hard`

**题目简述：**

给定由 `(`, `)` 构成的字符串，求其最长有效连续子字符串。（左右括号相互匹配的字符串称为有效字符串）

**题目解答：**

使用栈结构记录匹配位置

```java
class Solution {
    public int longestValidParentheses(String s) {
        int maxans = 0;
        Stack<Integer> stack = new Stack<>();
        stack.push(-1);
        for (int i = 0; i < s.length(); i++) {
            if (s.charAt(i) == '(') {
                stack.push(i);
            } else {
                stack.pop();
                if (stack.empty()) {
                    stack.push(i);
                } else {
                    maxans = Math.max(maxans, i - stack.peek());
                }
            }
        }
        return maxans;
    }
}
```

<br/>



## 4. Design Circular Queue

[622. Design Circular Queue](https://leetcode.com/problems/design-circular-queue/) `Medium`

**题目简述：**

设计一个循环队列实现。 循环队列是一种线性数据结构，其操作遵循先进先出且队尾队首相连。该队列需要支持如下操作：

- `MyCircularQueue(k)`: 构造器，设置队列长度为 k 。
- `Front`: 从队首获取元素。如果队列为空，返回 -1 。
- `Rear`: 获取队尾元素。如果队列为空，返回 -1 。
- `enQueue(value)`: 向循环队列插入一个元素。如果成功插入则返回真。
- `deQueue()`: 从循环队列中删除一个元素。如果成功删除则返回真。
- `isEmpty()`: 检查循环队列是否为空。
- `isFull()`: 检查循环队列是否已满。


**题目解答：**

使用数组记录元素值，并注意两个指针相对关系即可。

```java
class MyCircularQueue {
    int[] arrayQueue = null;
    int front = 0;
    int rear = 0;
    boolean noEl;
    
    public MyCircularQueue(int k) {
            arrayQueue  = new int[k];
        this.noEl = true;
        
    }
    
    public boolean enQueue(int value) {
        if(!this.isFull()){
            this.noEl = false;
            arrayQueue[rear] = value;
            rear++;
            rear = rear%arrayQueue.length;
            return true;
        } else{
            return false;
        }
    }
    
    public boolean deQueue() {
        if(!this.isEmpty()){
            front++;
            front = front% arrayQueue.length;
            if(front == rear){
                this.noEl = true;
            }
            return true;
                
        } else{
            return false;
        }
    }
    
    public int Front() {
        if(!this.isEmpty()){
            return arrayQueue[front];
        } else{
            return -1;
        }
            
    }
    
    public int Rear() {
        if(!this.isEmpty()){
            if(rear==0){
                return arrayQueue[arrayQueue.length-1];
            } else{
             return arrayQueue[rear-1];
            }
        } else{
            return -1;
        }
    }
    
    public boolean isEmpty() {
        return this.noEl;
    }
    
    public boolean isFull() {
        return (rear == front)  && !this.noEl;
    }
}

/**
 * Your MyCircularQueue object will be instantiated and called as such:
 * MyCircularQueue obj = new MyCircularQueue(k);
 * boolean param_1 = obj.enQueue(value);
 * boolean param_2 = obj.deQueue();
 * int param_3 = obj.Front();
 * int param_4 = obj.Rear();
 * boolean param_5 = obj.isEmpty();
 * boolean param_6 = obj.isFull();
 */
```

<br/>



## 5. Global and Local Inversions

[775. Global and Local Inversions](https://leetcode.com/problems/global-and-local-inversions/) `Medium`

**题目简述：**

给出一个由 [0, n - 1] 内所有整数构成的的整数数组 nums，求该数组中 *“全局倒置”* 的数量是否等于 *“局部倒置”* 的数量。

*“全局倒置”* 的数目等于满足下述条件不同下标对 (i, j) 的数目：

- `0 <= i < j < n`
- `nums[i] > nums[j]`

*“局部倒置”* 的数目等于满足下述条件的下标 i 的数目：

- `0 <= i < n - 1`
- `nums[i] > nums[i + 1]`


**题目解答：**

根据题目定义，局部倒置是全局导致的一种特殊情况，因而判断全局倒置的数量是否等于局部倒置的数量，相当于判断数组中是否存在非局部倒置。

思考以下情况：  

> 对于不存在存在非局部倒置的排列而言，0 应该在哪里呢？ 如果 0 的下标大于等于 2，一定会有 A[0] > A[2] = 0，这是一个非局部倒置。所以 0 只能出现在下标 0 或者下标 1。当 A[1] = 0，显然 A[0] = 1，否则就会有 A[0] > A[j] = 1，这也是一个非局部倒置。当 A[0] = 0，这时候问题就转化成了一个子问题。
> 
> （LeetCode-CN 官方解释）

可以归纳出不存在存在非局部倒置的数组的充分必要条件为 Math.abs(A[i] - i) <= 1。


```java
class Solution {
    public boolean isIdealPermutation(int[] A) {
        for (int i = 0; i < A.length; i++)
            if (i - A[i] > 1 || i - A[i] < -1) return false;
        return true;
    }
}
```

<br/>



## 6. Minimum Operations to Make Array Equal

[1551. Minimum Operations to Make Array Equal](https://leetcode.com/problems/minimum-operations-to-make-array-equal/) `Medium`

**题目简述：**

现有长度为 n 的数组，其取值为 `arr[i] = (2 * i) + 1`，现可以操作数组内元素，一次操作可以使得任意元素加一，且同时另一元素减一，求最少需要多少次操作可以使得数组内所有值相等。（题目保证一定能在有限步骤内完成）

**题目解答：**

题目中要求的数组，显然是 (1,3,5,..) 格式，此时：

- 对奇数长度而言，需要使得左半数量的元素（`k = n/2`）每次加 $(...,8,4,2)$，即等差数列求和 $Sn=n(a_1+a_n)/2$，此时 $n = k，, a_1 = 2,a_n = 2*k = n$，即可得到，$ans = (k * (2 + 2*k))/2 = k * (k + 1)$
- 对偶数长度而言，需要加的值实际是 $(...,5,3,1)$，即每个值都比奇数时少一，因而在奇数情况下减 `k` 即可。

时间复杂度：$O(1)$，空间复杂度：$O(1)$

```java
class Solution {
    public int minOperations(int n) {
        int k = n/2;
        return k * (k+1) - (n % 2 == 0 ? k : 0);
    }
}
```

<br/>



## 7. Determine if String Halves Are Alike

[1704. Determine if String Halves Are Alike](https://leetcode.com/problems/determine-if-string-halves-are-alike/) `Easy`

**题目简述：**

给出偶数长度的字符串，将其划分为等长的两个新字符串，判断这两个字符串中元音字母（`'a'`, `'e'`, `'i'`, `'o'`, `'u'`, `'A'`, `'E'`, `'I'`, `'O'`, `'U'`）的数量是否相等。

**题目解答：**

枚举检查即可

```java
class Solution {
    public boolean halvesAreAlike(String s) {
        int cnt = 0;
        for(int i = 0; i < s.length()/2; i++) {
            cnt += isVowel(s.charAt(i)) ? 1 : 0;
        }
        for(int i = s.length()/2; i < s.length(); i++) {
            cnt -= isVowel(s.charAt(i)) ? 1 : 0;
        }
        return cnt == 0;
    }
    
    private boolean isVowel(char c) {
        return 
            c == 'a' || c == 'e' || c == 'i' || c == 'o' || c == 'u' || 
            c == 'A' || c == 'E' || c == 'I' || c == 'O' || c == 'U';
    }
}
```

<br/>



## 8. Letter Combinations of a Phone Number

[17. Letter Combinations of a Phone Number](https://leetcode.com/problems/letter-combinations-of-a-phone-number/) `Medium`

**题目简述：**

给出仅包含数字 2-9 的字符串，返回所有它能表示的字母组合。数字到字母的映射如下（与电话按键相同）。

注意：1 不对应任何字母，答案可以按任意顺序返回。

![](/images/leetcode/april-leetcoding-challenge-2021/200px-Telephone-keypad2.svg.png)

**题目解答：**

根据映射关系，穷举所有的解即可。

时间复杂度：$O(3^m \times 4^n)$，空间复杂度：$O(m+n)$

```java
class Solution {
    private String[] btns = new String[]{"", "", "abc", "def", "ghi", "jkl", "mno", "pqrs", "tuv", "wxyz"};
    private List<String> ans = new ArrayList<>();
    
    public List<String> letterCombinations(String digits) {
        if (digits == null || digits.length() == 0) return ans;
        
        process(digits, 0, "");
        return ans;
    }
    
    private void process(String digits, int i, String current) {
        if (digits.length() == i) {
            ans.add(current);
            return ;
        }
        
        int num = digits.charAt(i) - '0';
        for(char c : btns[num].toCharArray()) {
            process(digits, i+1, current + c);
        }
    }
}
```

<br/>


## 9. Verifying an Alien Dictionary

[953. Verifying an Alien Dictionary](https://leetcode.com/problems/verifying-an-alien-dictionary/) `Easy`

**题目简述：**

现有一种外星语言同样使用小写英文字母，但顺序不同。先给出该语言的字母排序，和一个词典列表，判断词典中词语的排序是否与字母排序相符合。

**题目解答：**

用 Map 记录每个字母的排序值，再遍历检查词典中单词顺序即可。

时间复杂度：$O(m)$，空间复杂度：$O(1)$，$m$ 表示词典内单词数量

```java
class Solution {
    public boolean isAlienSorted(String[] words, String order) {
        int sorted[] = new int[27];
        for(int i = 0; i < order.length(); i++) {
            sorted[order.charAt(i) - 'a'] = i;
        }
        
        for(int i = 0; i < words.length-1; i++) {
            for(int j = 0; j < words[i].length(); j++) {
                if (j >= words[i+1].length()) return false;
                
                if (words[i].charAt(j) != words[i+1].charAt(j)) {
                    if (sorted[words[i].charAt(j) - 'a'] > sorted[words[i+1].charAt(j) - 'a']) return false;
                    else break;
                }
            }
        }
        return true;
    }
}
```

<br/>



## 10. Longest Increasing Path in a Matrix

[329. Longest Increasing Path in a Matrix](https://leetcode.com/problems/longest-increasing-path-in-a-matrix/) `Easy`

**题目简述：**

给出矩阵，求矩阵内严格升序序列最长长度，每次可以上、下、左、右移动一格。

**题目解答：**

深度优先遍历，递归查询并记忆每个位置开始的最大升序长度。

```java
class Solution {
    private int n, m, ret = 0;
    private int[][] walks = new int[][]{{-1, 0}, {1, 0}, {0, -1}, {0, 1}}, visited;

    public int longestIncreasingPath(int[][] matrix) {
        n = matrix.length;
        m = matrix[0].length;
        visited = new int[n][m];

        int ret = 0;
        for(int i = 0; i < n; i++) {
            for(int j = 0; j < m; j++) {
                ret = Integer.max(ret, path(matrix, i, j));
            }
        }
        return ret;
    }

    private int path(int[][] matrix, int i, int j) {
        if (visited[i][j] != 0) return visited[i][j];

        int ret = 1;
        for(int[] walk : walks) {
            int nextI = i + walk[0], nextJ = j + walk[1];
            if (nextI >= 0 && nextI < n && nextJ >= 0 && nextJ < m && matrix[nextI][nextJ] > matrix[i][j]) {
                ret = Integer.max(ret, path(matrix, nextI, nextJ) + 1);
            }
        }
        return visited[i][j] = ret;
    }
}
```

<br/>



## 11. Deepest Leaves Sum

[1302. Deepest Leaves Sum](https://leetcode.com/problems/palindrome-linked-list/) `Medium`

**题目简述：**

给出一棵二叉树，求二叉树层数最深的节点之和

**题目解答：**

深度优先遍历两轮，第一轮确定最大深度，第二轮对该深度求和即可。

时间复杂度：$O(N)$，空间复杂度：$O(H)$，其中 $N$ 是树中的节点个数，$H$ 是树的高度（最大深度）。

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    public int deepestLeavesSum(TreeNode root) {
        List<TreeNode> list = new ArrayList<TreeNode>();
        list.add(root);
        
        int i = 0, currentTier = 1, nextTier = 0, count = 0;
        while(i < list.size()) {
            if (count >= currentTier) {
                currentTier = nextTier;
                count = 0;
                nextTier = 0;
            }
            
            TreeNode node = list.get(i++);
            if (node.left != null) {
                list.add(node.left);
                nextTier++;
            }
            if (node.right != null) {
                list.add(node.right);
                nextTier++;
            }
            count++;
        }
        
        int ret = 0;
        while(currentTier != 0) {
            ret += list.remove(list.size() - 1).val;
            currentTier--;
        }
        return ret;
    }
}

```

<br/>



## 12. Beautiful Arrangement II

[667. Beautiful Arrangement II](https://leetcode.com/problems/beautiful-arrangement-ii/) `Medium`

**题目简述：**

使用 `1` 到 `n` 共 `n` 个数字组成一个序列，使得两个相邻数字的绝对值之差共有 k 种值。（如果存在多个解答，只需返回任一有效解答即可）

**题目解答：**

（官解）分析可得：
- 如果 k 为 1：则有效解答为 `[1, 2, 3, .., n]`
- 如果 k 为 n-1，则有效解答为 `[1, n, 2, n-1, 3, n-2, ....]`

因此，我们可以将答案分为两个部分，一个部分下，数字以最大的波动展示，以谋求最多的不同差值，直到达到 k 为止，将剩下的差值均置为 1。

比如 n = 6, k = 3 时，我们先尽量具有更大的差值，即 `[1, 4, 2, 3]`，此时差值分别为 `3`，`2`，`1`，已达到 k 种情况，剩下的差值只需都用 `1` 补齐即可，方法是将 `[1, 2]` 补在列表前面，并将已有列表的值均加 2，得到 `[1, 2, 3, 6, 4, 5]`

算法为，第 r 个（0-indexed）元素取值：
- r 小于 n-k 时：`nums[r] = i+1`
- r 大于 n-k 时，`nums[r] = (i%2 == 0) ? (n-k + i/2) : (n - i/2)` （i/1 取值为 `1, 1, 2, 2, 3, 3, ...`）

```java
class Solution {
    public int[] constructArray(int n, int k) {
        int[] ans = new int[n];
        int c = 0;
        for (int v = 1; v < n-k; v++) {
            ans[c++] = v;
        }
        for (int i = 0; i <= k; i++) {
            ans[c++] = (i%2 == 0) ? (n-k + i/2) : (n - i/2);
        }
        return ans;
    }
}
```

<br/>



## 13. Flatten Nested List Iterator

[341. Flatten Nested List Iterator](https://leetcode.com/problems/flatten-nested-list-iterator/) `Medium`

**题目简述：**

题目给出一个嵌套的整型列表，每个列表中的值或者为一个整数，或者是另一个列表。要求设计一个迭代器，使其能够遍历这个整型列表中的所有整数。

**题目解答：**

标准做法是栈，但是实现时候发现有写复杂，强制转换为非嵌套列表完成。但实际上这种做法并不符合迭代器的定义。

```java
/**
 * // This is the interface that allows for creating nested lists.
 * // You should not implement it, or speculate about its implementation
 * public interface NestedInteger {
 *
 *     // @return true if this NestedInteger holds a single integer, rather than a nested list.
 *     public boolean isInteger();
 *
 *     // @return the single integer that this NestedInteger holds, if it holds a single integer
 *     // Return null if this NestedInteger holds a nested list
 *     public Integer getInteger();
 *
 *     // @return the nested list that this NestedInteger holds, if it holds a nested list
 *     // Return empty list if this NestedInteger holds a single integer
 *     public List<NestedInteger> getList();
 * }
 */
public class NestedIterator implements Iterator<Integer> {
    private List<NestedInteger> list;
    private Iterator<NestedInteger> it;

    public NestedIterator(List<NestedInteger> nestedList) {
        this.list = new ArrayList<>();
        init(nestedList);
        this.it = list.iterator();
    }

    private void init(List<NestedInteger> nestedList) {
        for(NestedInteger nested : nestedList) {
            if (nested.isInteger()) {
                list.add(nested);
            } else {
                init(nested.getList());
            }
        }
    }

    @Override
    public Integer next() {
        return this.it.next().getInteger();
    }

    @Override
    public boolean hasNext() {
        return this.it.hasNext();
    }
}

/**
 * Your NestedIterator object will be instantiated and called as such:
 * NestedIterator i = new NestedIterator(nestedList);
 * while (i.hasNext()) v[f()] = i.next();
 */
```

<br/>


## 14. Partition List

[86. Partition List](https://leetcode.com/problems/partition-list/) `Medium`

**题目简述：**

给出链表和标准值 `x` ，要求对链表进行处理，使小于 x 的节点都出现在大于或等于 x 的节点之前。（保留每个节点的初始相对位置）

**题目解答：**

双指针扫描：用一个指针用于遍历链表，另一个指针记录当前小于 x 的链表当前位置。如果遇到小于 x 的节点，将其接到当前链表最后即可。

时间复杂度：$O(n)$，空间复杂度：$O(1)$

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public ListNode partition(ListNode head, int x) {
        ListNode ret = new ListNode(Integer.MIN_VALUE);
        ret.next = head;
        
        ListNode node = ret, current = ret;
        while(node.next != null) {
            if (node.next.val < x) {
                if (current != node) {
                    ListNode currentNext = current.next, nodeNext = node.next.next;
                    current.next = node.next;
                    node.next.next = currentNext;
                    node.next = nodeNext;
                } else {
                    node = node.next;
                }
                current = current.next;
            } else {
                node = node.next;
            }
        }
        return ret.next;
    }
}
```

<br/>



## 15. Fibonacci Number

[509. Fibonacci Number](https://leetcode.com/problems/fibonacci-number/) `Easy`

**题目简述：**

求第 n 个（0-index）斐波那契数，斐波那契数由 0、1 开始，后面每个数字都是前两个数字之和。

**题目解答：**

递归的最常见的入门题之一，注意记忆即可。

时间复杂度：$O(n)$，空间复杂度：$O(n)$

```java
class Solution {
    private int[] ans = new int[31];
    public int fib(int n) {
        if (ans[n] != 0) return ans[n];
        if (n == 0 || n == 1) return ans[n] = n;
        return fib(n-1) + fib(n-2);
    }
}
```

<br/>



## 16. Remove All Adjacent Duplicates in String II

[1209. Remove All Adjacent Duplicates in String II](https://leetcode.com/problems/remove-all-adjacent-duplicates-in-string-ii/) `Medium`

**题目简述：**

给定字符串 s 和正整数 k，每次可以从字符串中删除 k 个相邻且相等的字母，删去后的字符串的左侧和右侧相连。要求重复进行无限次这样的删除操作，直到无法继续为止，并返回最终得到的字符串。（保证保证唯一）

**题目解答：**

使用栈结构即可，一旦遇到 k 个相同元素即出栈清除。

```java
class Solution {
    public String removeDuplicates(String s, int k) {
        Stack<Node> stack = new Stack<>();
        for(int i = 0; i < s.length(); i++) {
            if (!stack.empty() && stack.peek().c == s.charAt(i)) {
                stack.peek().cnt += 1;
            } else {
                stack.add(new Node(s.charAt(i), 1));
            }
            
            if (stack.peek().cnt >= k) {
                stack.pop();
            }
        }
        
        StringBuilder sb = new StringBuilder();
        while(!stack.empty()) {
            Node node = stack.pop();
            for(int i = 0; i < node.cnt; i++) {
                sb.insert(0, node.c);
            }
        }
        return sb.toString();
    }
    
    private static class Node {
        public char c;
        public int cnt;
        
        Node(char c, int cnt) {
            this.c = c;
            this.cnt = cnt;
        }
    }
}
```

<br/>



## 17. Number of Submatrices That Sum to Target

[1074. Number of Submatrices That Sum to Target](https://leetcode.com/problems/number-of-submatrices-that-sum-to-target/) `Hard`

**题目简述：**

给出二维数组和目标值，求二维数组含有多少元素和为 target 的子二维数组。

**题目解答：**

二维前缀和：前缀和 + 四重迭代

```java
class Solution {
    public int numSubmatrixSumTarget(int[][] matrix, int target) {
        int n = matrix.length, m = matrix[0].length;
        int[][] dp = new int[n + 1][m + 1];
        for(int i = 0; i < n; i++) {
            for(int j = 0; j < m; j++) {
                dp[i+1][j+1] = dp[i][j+1] + dp[i+1][j] - dp[i][j] + matrix[i][j];
            }
        }

        int ans = 0;
        for(int x1 = 0; x1 < n; x1++) {
            for(int x2 = x1+1; x2 < n+1; x2++) {
                for(int y1 = 0; y1 < m; y1++) {
                    for(int y2 = y1+1; y2 < m+1; y2++) {
                        if (dp[x2][y2] - dp[x1][y2] - dp[x2][y1] + dp[x1][y1] == target) {
                            ans ++;
                        }
                    }
                }
            }
        }
        return ans;
    }
}
```

<br/>



## 18. Remove Nth Node From End of List

[19. Remove Nth Node From End of List](https://leetcode.com/problems/remove-nth-node-from-end-of-list/) `Medium`

**题目简述：**

给出单向链表，要求移除倒数第 n 个元素。

**提高**：能否在一轮遍历中完成

**题目解答：**

双指针，使用距离为 n 的双指针遍历即可找到倒数第 n 个元素

时间复杂度：$O(L)$ ，空间复杂度：$O(1)$

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public ListNode removeNthFromEnd(ListNode head, int n) {
        ListNode ret = new ListNode(0), l = ret, r = ret;
        ret.next = head;
        while(r.next != null && n-- > 0) {
            r = r.next;
        }

        while(r.next != null) {
            r = r.next;
            l = l.next;
        }
        l.next = l.next.next;
        return ret.next;
    }
}
```

<br/>

## 19. Combination Sum IV

[377. Combination Sum IV](https://leetcode.com/problems/combination-sum-iv/) `Medium`

**题目简述：**

给出互不相同的一组数字及目标值，可任意使用使用数组范围内的数字（每个数字不限次数），使之求和为目标值，求共有多少种取值方式（不同排序视为不同解答）。

**题目解答：**

动态规划公式：$dp[target] = \sum_{i=1}^{n} dp[target-nums[i]], dp[0] = 1$

注意遍历方式，需要按顺序遍历 n 的所有取值。尝试了好几次遇到 `target-n` 值再遍历的方式，会遇到重复解或不正确解（局部解）。

```java
class Solution {
    private int ans[] = new int[1001];

    public int combinationSum4(int[] nums, int target) {
        int[] dp = new int[target+1];
        dp[0] = 1;
        for(int i = 1; i <= target; i++) {
            for(int num : nums) {
                if (num <= i) dp[i] += dp[i - num];
            }
        }
        return dp[target];
    }
}
```

<br/>

## 20. N-ary Tree Preorder Traversal

[589. N-ary Tree Preorder Traversal](https://leetcode.com/problems/n-ary-tree-preorder-traversal/) `Easy`

**题目简述：**

给出 N 叉树的根节点，求其节点值的先序遍历结果。

**提高：** 递归写法非常简单，你是否能用迭代方式给出解答？

**题目解答：**

深度优先搜索、递归即可

```java
/*
// Definition for a Node.
class Node {
    public int val;
    public List<Node> children;

    public Node() {}

    public Node(int _val) {
        val = _val;
    }

    public Node(int _val, List<Node> _children) {
        val = _val;
        children = _children;
    }
};
*/

class Solution {
    private List<Integer> list = new ArrayList<>();
    public List<Integer> preorder(Node root) {
        if (root == null) return list;
        
        list.add(root.val);
        for(Node node : root.children) {
            preorder(node);
        }
        return list;
    }
}
```

<br/>

## 21. Triangle

[120. Triangle](https://leetcode.com/problems/triangle/) `Medium`

**题目简述：**

给出一个三角形二维数组，从顶到底，对当前位置 `i` ，每次可以移动到下一层的 `i` 或 `i+1` 位置，求从顶到底的路径中，最小和为多少？

**提高**：能否在 O(n) 空间复杂度内完成？

**题目解答：**

动态规划：每个位置只能从上一层同位置或前一位置移动得到，显然有：$dp[i][j] = min(dp[i-1][j], dp[i-1][j-1]) + triangle[i][j]$，只需判断边界情况即可。

时间复杂度：$O(n^2)$，空间复杂度：$O(1)$

```java
class Solution {
    public int minimumTotal(List<List<Integer>> triangle) {
        for(int i = 1; i < triangle.size(); i++) {
            List<Integer> prevList = triangle.get(i-1), list = triangle.get(i);
            for(int j = 0; j < list.size(); j++) {
                if (j == 0) {
                    list.set(j, list.get(j) + prevList.get(j));
                } else if (j == list.size()-1) {
                    list.set(j, list.get(j) + prevList.get(j-1));
                } else {
                    list.set(j, list.get(j) + Integer.min(prevList.get(j-1), prevList.get(j)));
                }
            }
        }
        
        int ans = Integer.MAX_VALUE;
        for(int num : triangle.get(triangle.size()-1)) {
            ans = Integer.min(ans, num);
        }
        return ans;
    }
}
```

<br/>

## 22. Brick Wall

[554. Brick Wall](https://leetcode.com/problems/brick-wall/) `Medium`

**题目简述：**

给出一堵墙中每行各个砖头的宽度，要求垂直切开整个墙（不能用两侧切），求最少需要切开多少块砖头。

**题目解答：**

Map 保存每个位置的空隙数量

时间复杂度：$O(nm)$，空间复杂度：$O(nm)$，其中 $n$ 是砖墙的高度，$m$ 是每行砖墙的砖的平均数量。

```java
class Solution {
    public int leastBricks(List<List<Integer>> wall) {
        Map<Integer, Integer> map = new HashMap<>();
        for(List<Integer> bricks : wall) {
            int width = 0;
            for(int i = 0; i < bricks.size()-1; i++) {
                width += bricks.get(i);
                map.put(width, map.getOrDefault(width, 0) + 1);
            }
        }

        int maxCnt = 0;
        for(Integer num : map.values()) {
            maxCnt = Integer.max(maxCnt, num);
        }
        return wall.size() - maxCnt;
    }
}
```

<br/>

## 23. Count Binary Substrings

[696. Count Binary Substrings](https://leetcode.com/problems/count-binary-substrings/) `Easy`

**题目简述：**

给定只由 `0`、`1` 组成的字符串 s，计算具有相同数量 0 和 1 的且所有 0 和所有 1 都是连续的非空（连续）子字符串的数量。（不同位置但内容相同的子串视为不同的子串。

**题目解答：**

检查整个字符串，计算相邻且各自连续的 `0`、`1` 最大长度，其中 `0`、`1` 出现次数的较小值就是该范围内的子串数量，将所有结果相加即可。

时间复杂度：$O(n)$，空间复杂度：$O(1)$

```java
class Solution {
    public int countBinarySubstrings(String s) {
        int zeros = 0, ones = 0, ans = 0;
        for(int i = 0; i < s.length(); i++) {
            if (i > 0 && s.charAt(i) != s.charAt(i-1)) {
                ans += Integer.min(zeros, ones);
                if (s.charAt(i) == '1') {
                    ones = 0;
                } else {
                    zeros = 0;
                }
            }

            if (s.charAt(i) == '1') {
                ones++;
            } else {
                zeros++;
            }
        }
        ans += Integer.min(zeros, ones);
        return ans;
    }
}
```

<br/>

## 24. Critical Connections in a Network

[1192. Critical Connections in a Network](https://leetcode.com/problems/critical-connections-in-a-network/) `Hard`

**题目简述：**

给定无向图（点的数量 + 无向边端点列表），该无向图目前所有节点相连，求找出图中所有 *“关键连接”*。（返回顺序不限）

关键连接 是指某些无向边，删除该无向边后某些点将无法访问其他某些点。

**题目解答：**

（待补充）

```java
class Solution {
    int[] disc, low;
    int time = 1;
    List<List<Integer>> ans = new ArrayList<>();
    Map<Integer,List<Integer>> edgeMap = new HashMap<>();
    public List<List<Integer>> criticalConnections(int n, List<List<Integer>> connections) {
        disc = new int[n];
        low = new int[n];
        for (int i = 0; i < n; i++)
            edgeMap.put(i, new ArrayList<Integer>());
        for (List<Integer> conn : connections) {
            edgeMap.get(conn.get(0)).add(conn.get(1));
            edgeMap.get(conn.get(1)).add(conn.get(0));
        }
        dfs(0, -1);
        return ans;
    }
    public void dfs(int curr, int prev) {
        disc[curr] = low[curr] = time++;
        for (int next : edgeMap.get(curr)) {
            if (disc[next] == 0) {
                dfs(next, curr);
                low[curr] = Math.min(low[curr], low[next]);
            } else if (next != prev)
                low[curr] = Math.min(low[curr], disc[next]);
            if (low[next] > disc[curr]) 
                ans.add(Arrays.asList(curr, next));
        }
    }
}

```

<br/>

## 25. Rotate Image

[48. Rotate Image](https://leetcode.com/problems/rotate-image/) `Medium`

**题目简述：**

给出一个二维正方形数组（宽高相等），将其原地 90 度旋转。

**题目解答：**

原地旋转：直接原地交换四个位置的值即可，注意交换范围是左上角高 `n/2`，宽 `(n+1)/2` 范围内。

时间复杂度：$O(N^2)$，空间复杂度：$O(1)$

```java
class Solution {
    public void rotate(int[][] matrix) {
        int n = matrix.length, k = (n/2)+(n%2), r = k-(n%2==0?0:1);
        for(int i = 0; i < k; i++) {
            for(int j = 0; j < r; j++) {
                int temp = matrix[i][j];
                matrix[i][j] = matrix[n-j-1][i];
                matrix[n-j-1][i] = matrix[n-i-1][n-j-1];
                matrix[n-i-1][n-j-1] = matrix[j][n-i-1];
                matrix[j][n-i-1] = temp;
            }
        }
    }
}
```

<br/>

## 26. Furthest Building You Can Reach

[1642. Furthest Building You Can Reach](https://leetcode.com/problems/furthest-building-you-can-reach/) `Medium`

**题目简述：**

给你一个整数数组 heights ，表示一排建筑物的高度。另有 bricks 数量的砖块  和 ladders 数量的梯子。

你从建筑物 0 开始旅程，不断向后面的建筑物移动，求最远可以到达的建筑物的下标。

当从建筑物 i 移动到建筑物 i+1 时：

- 如果当前建筑物的高度 大于或等于 下一建筑物的高度，则不需要梯子或砖块
- 如果当前建筑的高度 小于 下一个建筑的高度，您可以使用 一架梯子 或 (h[i+1] - h[i]) 个砖块

**题目解答：**

（待补充）

```java
class Solution {
    public int furthestBuilding(int[] heights, int bricks, int ladders) {
        int len = heights.length;
        Queue<Integer> queue = new PriorityQueue<>();
        for(int i = 0; i < len-1; i++) {
            int diff = heights[i+1] - heights[i];
            if (diff > 0) {
                if (ladders > 0) {
                    queue.add(diff);
                    ladders--;
                } else if (!queue.isEmpty() && diff > queue.peek()) {
                    queue.add(diff);
                    bricks -= queue.poll();
                } else {
                    bricks -= diff;
                }
                if (bricks < 0) {
                    return i;
                }
            }
        }
        return len-1;
    }
}
```

<br/>

## 27. Power of Three

[326. Power of Three](https://leetcode.com/problems/power-of-three/) `Easy`

**题目简述：**

判断给出的数是否是 3 的幂

**提高：** 是否能不适用循环/递归实现？

**题目解答：**

循环：循环做整除即可

时间复杂度：$O(log_b(n))$，空间复杂度：$O(1)$

```java
class Solution {
    public boolean isPowerOfThree(int n) {
        while(n != 0 && n % 3 == 0) {
            n /= 3;
        }
        return n == 1;
    }
}
```

<br/>

## 28. Unique Paths II

[63. Unique Paths II](https://leetcode.com/problems/unique-paths-ii/) `Medium`

**题目简述：**

给出二维数组，其中包含 0、1 两种取值，1 代表障碍物，不能行走。路径上没有障碍物的情况下，每次可以向右或向下行进一格，求从左上角到右下角共有多少种不同走法。

**题目解答：**

递归判断，并注意记忆结果

> 这道题答错好几次，主要问题在于：
> 1. 没有考虑开始结束位置为障碍物的情况；
> 2. 判断的顺序不对，没有把越界判断放在最前面，一开始把 `x==0 && y==0` 写在最前面，完全没考虑 `obstacleGrid[x][y] == 1` 的可能性，当然加上这个判断也是一个办法，但从通用写法的角度考虑，把特殊值的情况放在越界判断后面显然是更合理一点的。

时间复杂度：$O(M \times N)$，空间复杂度：$O(M \times N)$


```java
class Solution {
    private int m, n;
    private int[][] res;
    public int uniquePathsWithObstacles(int[][] obstacleGrid) {
        this.m = obstacleGrid.length;
        this.n = obstacleGrid[0].length;
        this.res = new int[m][n];

        for(int i = 0; i < m; i++) {
            Arrays.fill(res[i], -1);
        }
        res[0][0] = 1;
        return uniquePathsWithObstacles(obstacleGrid, m-1, n-1);
    }

    public int uniquePathsWithObstacles(int[][] obstacleGrid, int x, int y) {
        if (x < 0 || x >= m || y < 0 || y >= n || obstacleGrid[x][y] == 1) return 0;
        if (res[x][y] != -1) return res[x][y];

        return res[x][y] = uniquePathsWithObstacles(obstacleGrid, x-1, y) + uniquePathsWithObstacles(obstacleGrid, x, y-1);
    }
}
```

<br/>

## 29. Find First and Last Position of Element in Sorted Array

[34. Find First and Last Position of Element in Sorted Array](https://leetcode.com/problems/find-first-and-last-position-of-element-in-sorted-array/) `Medium`

**题目简述：**

给出升序排序的数组，求目标数字 `target` 在数组中的开始和结束位置，如果数组中不存在该数字，则返回 `[-1, -1]`

**提高**：是否能在 `O(log n)` 时间复杂度内完成？

**题目解答：**

本题可以看作寻找第一个等于 target 的位置，与第一个大于 target 的位置，因此可以通过两次二分实现。

时间复杂度：`O(log n)`，空间复杂度：`O(1)`

```java
class Solution {
    public int[] searchRange(int[] nums, int target) {
        int l = binarySearch(nums, target, true);
        int r = binarySearch(nums, target, false) - 1;  // 二分找到的是第一个大于 targer 的位置，因此还需要减一
        
        if (l <= r && r < nums.length && nums[l] == target && nums[r] == target) {
            return new int[]{l, r};
        }
        return new int[]{-1, -1};
    }
    
    // lower 为 true 表示寻找的是等于 targer 的第一个值
    public int binarySearch(int[] nums, int target, boolean lower) {
        int l = 0, r = nums.length - 1, ans = nums.length;  // ans 初始值为 nums.length，即没有值大于 target 时的默认结果。
        while(l <= r) {
            int mid = (l + r) / 2;

            // 由完整表达式简化得到：(!lower && nums[mid] > target) || (lower && nums[mid] >= target)
            if (nums[mid] > target || (lower && nums[mid] >= target)) {
                r = mid-1;
                ans = mid;
            } else {
                l = mid+1;
            }
        }
        return ans;
    }
}

```

<br/>

## 30. Powerful Integers

[970. Powerful Integers](https://leetcode.com/problems/powerful-integers/) `Medium`

**题目简述：**

给出 `x`、`y` 和 `bound`，求在不超过 `bound` 的范围内有哪些值满足 $x^i + y^j, (i >= 0, j >= 0)$ 。（返回值不要求顺序，但同一值不能重复出现）

**题目解答：**

暴力（广度优先搜索）：暴力计算所有 $x^i + y^j$ 的值即可，此处用队列方式搜索所有组合。

时间复杂度：$O(\log^2{\text{bound}})$，空间复杂度：$O(\log^2{\text{bound}})$

```java
class Solution {
    public List<Integer> powerfulIntegers(int x, int y, int bound) {
        if (bound <= 1) return Collections.emptyList();
        
        List<Integer> ans = new ArrayList<>();
        Set<Pair<Integer, Integer>> set = new HashSet<>();
        Set<Integer> ansSet = new HashSet<>();
        Queue<int[]> q = new LinkedList<>();
        q.offer(new int[]{2, 1, 1});
        set.add(new Pair<>(1,1));
        while(!q.isEmpty()) {
            int[] arr = q.poll();
            if (!ansSet.contains(arr[0])) {
                ans.add(arr[0]);
                ansSet.add(arr[0]);
            }
            
            long num;
            if ((num = arr[1] * x + arr[2]) <= bound && !set.contains(new Pair<>(arr[1] * x, arr[2]))) {
                set.add(new Pair<>(arr[1] * x, arr[2]));
                q.offer(new int[]{(int)num, arr[1] * x, arr[2]});
            }
            if ((num = arr[1] + arr[2] * y) <= bound && !set.contains(new Pair<>(arr[1], arr[2] * y))) {
                set.add(new Pair<>(arr[1], arr[2] * y));
                q.offer(new int[]{(int)num, arr[1], arr[2] * y});
            }
        }
        return ans;
    }
}

```

<br/>
