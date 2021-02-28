---
title: February LeetCoding Challenge 2021 解题报告
date: 2021-02-01
tags:
- leetcode
---


## Overview

[February LeetCoding Challenge 2021](https://leetcode.com/explore/challenge/card/february-leetcoding-challenge-2021/)

**Week 1: February 1st - February 7th**

1. Squirrel Simulation `Premium`
2. [Number of 1 Bits](#1-Number-of-1-Bits)
3. [Trim a Binary Search Tree](#2-Trim-a-Binary-Search-Tree)
4. [Linked List Cycle](#3-Linked-List-Cycle)
5. [Longest Harmonious Subsequence](#4-Longest-Harmonious-Subsequence)
6. [Simplify Path](#5-Simplify-Path)
7. [Binary Tree Right Side View](#6-Binary-Tree-Right-Side-View)
8. [Shortest Distance to a Character](#7-Shortest-Distance-to-a-Character)


**Week 2: February 8th - February 14th**

1. Number of Distinct Islands `Premium`
2. [Peeking Iterator](#8-Peeking-Iterator)
3. [Convert BST to Greater Tree](#9-Convert-BST-to-Greater-Tree)
4. [Copy List with Random Pointer](#10-Copy-List-with-Random-Pointer)
5. [Valid Anagram](#11-Valid-Anagram)
6. [Number of Steps to Reduce a Number to Zero](#12-Number-of-Steps-to-Reduce-a-Number-to-Zero)
7. [Shortest Path in Binary Matrix](#13-Shortest-Path-in-Binary-Matrix)
8. [Is Graph Bipartite?](#14-Is-Graph-Bipartite)


**Week 3: February 15th - February 21st**

1. Kill Process `Premium`
2. [The K Weakest Rows in a Matrix](#15-The-K-Weakest-Rows-in-a-Matrix)
3. [Letter Case Permutation](#16-Letter-Case-Permutation)
4. [Container With Most Water](#17-Container-With-Most-Water)
5. [Arithmetic Slices](#18-Arithmetic-Slices)
6. [Minimum Remove to Make Valid Parentheses](#19-Minimum-Remove-to-Make-Valid-Parentheses)
7. [Roman to Integer](#20-Roman-to-Integer)
8. [Broken Calculator](#21-Broken-Calculator)

**Week 4: February 22nd - February 28th**

1. Find the Celebrity `Premium`
2. [Longest Word in Dictionary through Deleting](#22-Longest-Word-in-Dictionary-through-Deleting)
3. [Search a 2D Matrix II](#23-Search-a-2D-Matrix-II)
4. [Score of Parentheses](#24-Score-of-Parentheses)
5. [Shortest Unsorted Continuous Subarray](#25-Shortest-Unsorted-Continuous-Subarray)
6. [Validate Stack Sequences](#26-Validate-Stack-Sequences)
7. [Divide Two Integers](#27-Divide-Two-Integers)
8. [Maximum Frequency Stack](#28-Maximum-Frequency-Stack)

<br/>

## 1. Number of 1 Bits

[191. Number of 1 Bits](https://leetcode.com/problems/number-of-1-bits/) `Easy`

**题目简述：**

计算一个无符号整数的二进制表示包含多少个 `1` ，注意 java 不支持无符号整数因此可能传入负数。

**题目解答：**

常规解法，逐位判断（传入负数时，要转换为补码）。

```java
public class Solution {
    // you need to treat n as an unsigned value
    public int hammingWeight(int n) {
        int cnt = n < 0 ? 1 : 0;
        if (n < 0) {
            n = Integer.MAX_VALUE + n + 1;
        }
        while(n != 0) {
            if ((n&1) == 1) cnt++;
            n >>= 1;
        }
        return cnt;
    }
}
```

<br/>

## 2. Trim a Binary Search Tree

[669. Trim a Binary Search Tree](https://leetcode.com/problems/trim-a-binary-search-tree/) `Medium`

**题目简述：**

处理一个二叉搜索树，将在给定取值范围外的节点移除，但注意剩余节点的相对位置不能改变。

**题目解答：**

为符合取值范围的节点查找其子节点即可，递归实现。

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    public TreeNode trimBST(TreeNode root, int low, int high) {
        if (root == null) return null;
        if (low <= root.val && root.val <= high) {
            root.left = trimBST(root.left, low, high);
            root.right = trimBST(root.right, low, high);
            return root;
        } else if (root.val < low) {
            return trimBST(root.right, low, high);
        } else {
            return trimBST(root.left, low, high);
        }
    }
}
```

<br/>

## 3. Linked List Cycle

[141. Linked List Cycle](https://leetcode.com/problems/linked-list-cycle/) `Easy`

**题目简述：**

经典题目，判断一个链表内是否存在循环。

**题目解答：**

常规解法一般是记录已访问的节点列表（或标记这些节点的 `val` 为特殊值），如果重复访问即说明存在循环。一种空间复杂度 O(1) 解法是，双指针以不同的速度扫描，如果遇到同一节点则说明存在循环。

```java
/**
 * Definition for singly-linked list.
 * class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) {
 *         val = x;
 *         next = null;
 *     }
 * }
 */
public class Solution {
    public boolean hasCycle(ListNode head) {
        if (head == null) return false;
        
        ListNode l = head, r = head.next;
        while(l != null && r != null) {
            if (l == r) return true;
            l = l.next;
            r = r.next;
            if (r != null) r = r.next;
        }
        return false;
    }
}
```

<br/>

## 4. Longest Harmonious Subsequence

[594. Longest Harmonious Subsequence594. Longest Harmonious Subsequence](https://leetcode.com/problems/longest-harmonious-subsequence/) `Easy`

**题目简述：**

定义最大最小值的差值恰好为 1 的序列为和谐（*harmonious*）序列，给出字符串，求最长和谐子序列长度，子序列不要求连续。

**题目解答：**

本题实际与顺序无关，排序后直接找值为 n 与 n+1 且均不为零，的最大计数之和。

```java
class Solution {
    public int findLHS(int[] nums) {
        Arrays.sort(nums);
        int n = nums[0], prev = 0, cnt = 0, max = 0;
        for(int num : nums) {
            if (num == n) {
                cnt++;
            } else if (num == n+1) {
                prev = cnt;
                n = num;
                cnt = 1;
            } else {
                prev = 0;
                n = num;
                cnt = 1;
            }
            
            if (prev != 0 && cnt + prev > max) {
                max = cnt + prev;
            }
        }
        return max;
    }
}
```

<br/>

## 5. Simplify Path

[71. Simplify Path](https://leetcode.com/problems/simplify-path/) `Medium`

**题目简述：**

将输入路径，改为绝对路径： `/` 开头，目录吉间用单个 `/` 分隔，除目录外不以 `/` 结尾，且不含 `.` 或`..` 目录

**题目解答：**

使用栈，以将相对路径转换为绝对路径。（ `.` 表示当前目录，`..` 表示上一层目录 ）

```java
class Solution {
    public String simplifyPath(String path) {
        Stack<String> stack = new Stack<>();
        path += "/";
        
        int prev = -1;
        for(int i = 0; i < path.length(); i++) {
            if (path.charAt(i) == '/') {
                if (prev != -1) {
                    String s = path.substring(prev+1, i);
                    if ("..".equals(s)) {
                        if (!stack.empty()) stack.pop();
                    } else if (!".".equals(s) && !"".equals(s)) {
                        stack.push(s);
                    }   
                }
                prev = i;
            }
        }
        
        StringBuilder sb = new StringBuilder();
        while(!stack.empty()) {
            String s = stack.pop();
            sb.insert(0, "/" + s);
            
        }
        return sb.length() == 0 ? "/" : sb.toString();
    }
}
```

<br/>

## 6. Binary Tree Right Side View

[199. Binary Tree Right Side View](https://leetcode.com/problems/binary-tree-right-side-view/) `Medium`

**题目简述：**

给定二叉树，求在二叉树右侧所看到的一列值，即求二叉树每层最右侧值的组成的列表

**题目解答：**

最直接的做法是层序遍历，但写起来比较麻烦，需要判断当前在哪一层，因此直接用递归写了个解法。

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    
    private List<Integer> ret = new ArrayList<>();

    public List<Integer> rightSideView(TreeNode root) {
        process(root, 0);
        return ret;
    }
    
    private void process(TreeNode node, int layer) {
        if (node == null) return;
        if (ret.size() <= layer) ret.add(node.val);
        else ret.set(layer, node.val);
        
        process(node.left, layer+1);
        process(node.right, layer+1);
    }
}
```

<br/>

## 7. Shortest Distance to a Character

[821. Shortest Distance to a Character](https://leetcode.com/problems/shortest-distance-to-a-character/) `Easy`

**题目简述：**

给定字符串和其中的一个字符，求字符串内每个位置与该字符的最短距离

**题目解答：**

找到该字符的每个位置，推算其周围的距离。

> 注：这里将 ret[i] 初始化为 -1 可以优化为 `Integer.MAX_VALUE` 以免去 `ret[i] == -1` 的判断

```java
class Solution {
    public int[] shortestToChar(String s, char c) {
        int[] ret = new int[s.length()];
        for(int i = 0; i < s.length(); i++) {
            ret[i] = -1;
        }
        
        for(int i = 0; i < s.length(); i++) {
            if (s.charAt(i) == c) {
                checkDistance(s, i, c, ret);
            }
        }
        return ret;
    }
    
    public void checkDistance(String s, int i, char c, int[] ret) {
        ret[i] = 0;
        boolean lFin = false, rFin = false;
        for(int l = i-1, r = i+1; !lFin || !rFin; l--,r++) {
            if (!lFin)  {
                if (l < 0 || s.charAt(l) == c) {
                    lFin = true;
                }
                if (!lFin && (ret[l] == -1 || ret[l] > i-l)) {
                    ret[l] = i-l;
                } else {
                    lFin = true;
                }
            }
            if (!rFin) {
                if (r >= s.length() || s.charAt(r) == c) {
                    rFin = true;
                }
                if (!rFin && (ret[r] == -1 || ret[r] > r-i)) {
                    ret[r] = r-i;
                } else {
                    rFin = true;
                }
            }
        }
    }
}
```

<br/>

## 8. Peeking Iterator

[284. Peeking Iterator](https://leetcode.com/problems/peeking-iterator/) `Medium`

**题目简述：**

给定一个 Iterator 对象，要求基于该对象包装，实现 peek() 方法，返回下一个元素，但不移动位置。

**题目解答：**

基于已有的 `next()` 和 `hasNext()` 显然不提供此功能，因此 peek 时实际上必然要发生移动，需要记录此时发生了 `peek` 操作，和当前 `peek` 的值用于后续操作。

```java
// Java Iterator interface reference:
// https://docs.oracle.com/javase/8/docs/api/java/util/Iterator.html

class PeekingIterator implements Iterator<Integer> {
    private Iterator<Integer> iterator;
    private Integer current = -1;
    private boolean isPeek = false;
    
	public PeekingIterator(Iterator<Integer> iterator) {
        this.iterator = iterator;
	    
	}
	
    // Returns the next element in the iteration without advancing the iterator.
	public Integer peek() {
        if (isPeek) return current;
        else {
            isPeek = true;
            return current = iterator.next();
        }
	}
	
	// hasNext() and next() should behave the same as in the Iterator interface.
	// Override them if needed.
	@Override
	public Integer next() {
        if (isPeek) {
            isPeek = false;
            return current;
        }
        else return iterator.next();
	}
	
	@Override
	public boolean hasNext() {
	    return iterator.hasNext() || isPeek;
	}
}
```

<br/>

## 9. Convert BST to Greater Tree

[538. Convert BST to Greater Tree](https://leetcode.com/problems/convert-bst-to-greater-tree/) `Medium`

**题目简述：**

给定二叉搜索树，为每个节点的值增加大于该节点值的节点值之和。

**题目解答：**

基于二叉搜索树的特点，实际上每个节点值就是按照后续遍历时的累加和。

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    private int sum = 0;
    public TreeNode convertBST(TreeNode root) {
        if (root == null) return null;
        
        convertBST(root.right);
        sum += root.val;
        root.val = sum;
        convertBST(root.left);
        return root;
    }
}
```

<br/>

## 10. Copy List with Random Pointer

[138. Copy List with Random Pointer](https://leetcode.com/problems/copy-list-with-random-pointer/) `Medium`

**题目简述：**

给出一个列表，next 值指向下一节点，random 值指向某一结点，要求深拷贝该列表。（注意原链表不能修改，且新链表必须均为全新节点）

**题目解答：**

这道题的主要问题是新旧节点的映射关系问题，一个旧节点必须对应唯一的新节点。上次做这道题直接用 Map 记录了映射关系，本次则采用了在链表内增加元素的方式（A --> A' --> B --> B' --> C --> C' --> D --> D'，带 `'` 的为新节点），以节约空间占用，旧节点的下一位置即新节点。

> 最终需要拆分为两个链表，注意原链表的位置关系也需要修复

```java
/*
// Definition for a Node.
class Node {
    int val;
    Node next;
    Node random;

    public Node(int val) {
        this.val = val;
        this.next = null;
        this.random = null;
    }
}
*/

class Solution {
    public Node copyRandomList(Node head) {
        if (head == null) return null;
        
        Node node = head;
        while(node != null) {
            Node nextNode = node.next;
            node.next = new Node(node.val);
            node.next.next = nextNode;
            node = nextNode;
        }
        
        node = head;
        while(node != null) {
            node.next.random = node.random == null ? null : node.random.next;
            node = node.next.next;
        }
        
        node = head;
        Node ret = head.next;
        while(node != null) {
            Node nextNode = node.next;
            node.next = node.next == null ? null : node.next.next;
            node = nextNode;
        }
        return ret;
    }
}
```

<br/>

## 11. Valid Anagram

[242. Valid Anagram](https://leetcode.com/problems/valid-anagram/) `Easy`

**题目简述：**

验证给出的两个字符串是否为有效的字谜（即字符仅允许顺序不同）

**题目解答：**

这里直接重排序判断，还可以用类似计数排序（`char[26]`）的方式比较。

```java
class Solution {
    public boolean isAnagram(String s, String t) {
        char[] a = s.toCharArray(), b = t.toCharArray();
        Arrays.sort(a);
        Arrays.sort(b);
        return Arrays.equals(a, b);
    }
}
```

<br/>

## 12. Number of Steps to Reduce a Number to Zero

[1342. Number of Steps to Reduce a Number to Zero](https://leetcode.com/problems/number-of-steps-to-reduce-a-number-to-zero/)  `Easy`

**题目简述：**

给出一个整数，按 “偶数时除二，奇数时减一” 的规则，求多少次运算后值变为 0

**题目解答：**

根据递推公式，写个递归

```java
class Solution {
    public int numberOfSteps (int num) {
        int cnt = 0;
        while(num != 0) {
            num = num % 2 == 0 ? num/2 : num-1;
            cnt++;
        }
        return cnt;
    }
}
```

<br/>

## 13. Shortest Path in Binary Matrix

[1091. Shortest Path in Binary Matrix](https://leetcode.com/problems/shortest-path-in-binary-matrix/) `Medium`

**题目简述：**

给出一个正方形数组，0 代表可以通过，1 代表道路不通，求从左上角到右下角的最少需要经过的格子数，每次行动可移动到周围八个格子内。

**题目解答：**

宽度优先搜索，结合优先队列。如果找到了已知节点的更优解则重新计算相关节点。

```java
class Solution {
    private int[][] gotos = new int[][]{{-1, -1},{0, -1},{1,-1},{-1, 0},{-1,1},{0,1},{1,0},{1,1}};
    public int shortestPathBinaryMatrix(int[][] grid) {
        int N = grid.length;
        int[][] shortestPaths = new int[N][N];
        for(int i = 0; i < N; i++) {
            for(int j = 0; j < N; j++) {
                shortestPaths[i][j] = Integer.MAX_VALUE;
            }
        }

        Queue<int[]> queue = new PriorityQueue<>((arr1, arr2) -> arr1[2] - arr2[2]);
        if (grid[0][0] == 0) {
            queue.offer(new int[]{0,0,1});
        }
        while(!queue.isEmpty()) {
            int[] arr = queue.poll();
            if (arr[2] >= shortestPaths[arr[0]][arr[1]]) {
                continue;
            }
            shortestPaths[arr[0]][arr[1]] = arr[2];
            if (arr[0] == N-1 && arr[1] == N-1) {
                return arr[2];
            }
            for(int i = 0; i < gotos.length; i++) {
                int nextX = arr[0] + gotos[i][0], nextY = arr[1] + gotos[i][1];
                if (nextX < 0 || nextX >= N || nextY < 0 || nextY >= N || grid[nextX][nextY] == 1) {
                    continue;
                }
                queue.offer(new int[]{nextX, nextY, arr[2]+1});
            }
        }
        return -1;
    }
}
```

<br/>


## 14. Is Graph Bipartite?

[785. Is Graph Bipartite?](https://leetcode.com/problems/is-graph-bipartite/) `Medium`

**题目简述：**

给出一个图形，判断该图形的节点是否可以分为两个集合，使得任意一条边的端点都分别位于两个集合内。（不保证所有点都连接，不保证图形连续）

**题目解答：**

假设节点 A 位于集合 1，则节点 A 连接的节点必位于集合 2，持续判断（宽度优先搜索，这里使用递归）直到发现冲突。注意，每个符合条件的节点都需检查。

```java
class Solution {
    private int[] arr;
    
    public boolean isBipartite(int[][] graph) {
        arr = new int[graph.length];
        for(int i = 0; i < graph.length; i++) {
            arr[i] = -1;
        }
        
        for(int i = 0; i < graph.length; i++) {
            if (graph[i].length != 0 && arr[i] == -1 && !check(graph, i, 1)) {
                return false;
            }
        }
        return true;
    }
    
    private boolean check(int[][] graph, int i, int group) {
        if (arr[i] == -1) {
            arr[i] = group;
        } else {
            return arr[i] == group;
        }
        
        int nextGroup = group == 1 ? 2 : 1;
        for(int j = 0; j < graph[i].length; j++) {
            if (!check(graph, graph[i][j], nextGroup)) {
                return false;
            }
        }
        return true;
    }
}
```

<br/>

## 15. The K Weakest Rows in a Matrix

[1337. The K Weakest Rows in a Matrix](https://leetcode.com/problems/the-k-weakest-rows-in-a-matrix/) `Easy`

**题目简述：**

给出一个由 0、1 构成的二维矩阵，要求按照 1 的数量从小到大排序每行，数量相等时按原始行号（0 起始）顺序排序，求排序后前 k 行的行号。

**题目解答：**

自定义排序即可

```java
class Solution {
    public int[] kWeakestRows(int[][] mat, int k) {
        List<Pair<Integer, Integer>> list = new ArrayList<>();
        for(int i = 0; i < mat.length; i++) {
            int cnt = 0;
            for(int j = 0; j < mat[i].length; j++) {
                cnt += mat[i][j];
            }
            list.add(new Pair(cnt, i));
        }

        Collections.sort(list, new Comparator<Pair<Integer, Integer>>() {
            @Override
            public int compare(Pair<Integer, Integer> a, Pair<Integer, Integer> b) {
                return a.getKey() != b.getKey() ? a.getKey()-b.getKey() : a.getValue() - b.getValue();
            }
        });
        
        int i = 0;
        int[] ret = new int[k];
        for(Pair<Integer, Integer> p : list) {
            ret[i++] = p.getValue();
            if (i >= k) return ret;
        }
        return ret;
    }
}
```

<br/>

## 16. Letter Case Permutation

[784. Letter Case Permutation](https://leetcode.com/problems/letter-case-permutation/) `Medium`

**题目简述：**

给出原始字符串，其中的英文字母可转换为大写或小写字母，求所有可能生成的字符串。（不要求返回值顺序）

**题目解答：**

递归遍历每个位置的所有可能值

```java
class Solution {
    public List<String> letterCasePermutation(String S) {
        List<String> list = new ArrayList<>();
        process(S.toLowerCase(), 0, list, new StringBuilder());
        return list;
    }
    
    private void process(String s, int i, List<String> list, StringBuilder sb) {
        if (sb.length() == s.length()) {
            list.add(sb.toString());
            return ;
        }
        char c = s.charAt(i);
        char[] arr = new char[2];
        int l = 1;
        
        arr[0] = c;
        if ('a' <= c && c <= 'z') {
            arr[l++] = (char) ((int)c-32);
        }
        for(int j = 0; j < l; j++) {
            sb.append(arr[j]);
            process(s, i+1, list, sb);
            sb.delete(sb.length()-1, sb.length());
        }
    }
}
```

<br/>

## 17. Container With Most Water

[11. Container With Most Water](https://leetcode.com/problems/container-with-most-water/) ``

**题目简述：**

给出 n 个非负整数，代表相互距离为 1 的垂直边高度。找到最合适的两个边，使之与 x 轴所形成的容器能够承载最多的水。

**题目解答：**

双指针扫描，任意两条边所能承载的水量为 最小垂直高度 * 距离。本题将指针分别位于最左和最右端，使之距离最长，然后逐步尝试提高最小高度，以谋求最优解。

```java
class Solution {
    public int maxArea(int[] height) {
        int l = 0, r = height.length - 1, ret = 0;
        while(l < r) {
            int w = r-l, h = Math.min(height[l], height[r]);
            if (w*h > ret) {
                ret = w*h;
            }
            
            if (height[l] == h) {
                while(l < r && height[l] <= h) {
                    l++;
                }
            }
            if (height[r] == h) {
                while(l < r && height[r] <= h) {
                    r--;
                }
            }
        }
        return ret;
    }
}
```

<br/>

## 18. Arithmetic Slices

[413. Arithmetic Slices](https://leetcode.com/problems/arithmetic-slices/) `Medium`

**题目简述：**

（待补充）

**题目解答：**

（待补充）

```java
class Solution {
    public int numberOfArithmeticSlices(int[] A) {
        int[] dp = new int[A.length];
        int sum = 0;
        for (int i = 2; i < dp.length; i++) {
            if (A[i] - A[i-1] == A[i-1] - A[i-2]) {
                dp[i] = dp[i-1] + 1;
                sum += dp[i];
            }
        }
        return sum;
    }
}
```

<br/>

## 19. Minimum Remove to Make Valid Parentheses

[1249. Minimum Remove to Make Valid Parentheses](https://leetcode.com/problems/minimum-remove-to-make-valid-parentheses/) `Medium`

**题目简述：**

给出小写字母与括号构成的字符串，要求从中移除最少数量的字符，以生成有效的括号表达式，即括号成对的字符串。（返回任一种有效表达式即可）

**题目解答：**

直观的做法是对左括号建立栈，此处用 `list` 模拟栈，寻找到多余的括号后则移除相关符号。

```java
class Solution {
    public String minRemoveToMakeValid(String s) {
        List<Integer> list = new ArrayList<>();
        int cnt = 0;
        StringBuilder sb = new StringBuilder();
        for(char c : s.toCharArray()) {
            if (c == '(') {
                cnt++;
                sb.append(c);
                list.add(sb.length()-1);
            } else if (c == ')') {
                if (cnt > 0) {
                    cnt--;
                    list.remove(list.size()-1);
                    sb.append(c);
                }
            } else {
                sb.append(c);
            }
        }
        
        if (cnt != 0) {
            for(int i = list.size()-1; i >= 0; i--) {
                sb.delete(list.get(i), list.get(i)+1);
            }
        }
        return sb.toString();
    }
}
```

<br/>


## 20. Roman to Integer

[13. Roman to Integer](https://leetcode.com/problems/roman-to-integer/) `Easy   `

**题目简述：**

将罗马数字字符串转换为数字。（字符串范围：`I`, `V`, `X`, `L`, `C`, `D`, `M`）

**题目解答：**

遍历各个字符即可，注意某些组合时，需要将两个字符放在一起运算。

```java
class Solution {
    public int romanToInt(String s) {
        int ret = 0;
        for(int i = 0; i < s.length(); i++) {
            if (i+1 < s.length() && match(s.charAt(i), s.charAt(i+1))) {
                ret += romanToInt(s.charAt(i+1)) - romanToInt(s.charAt(i));
                i++;
            } else {
                ret += romanToInt(s.charAt(i));
            }
        }
        return ret;
    }
    
    private int romanToInt(char c) {
        switch(c) {
            case 'I': return 1;
            case 'V': return 5;
            case 'X': return 10;
            case 'L': return 50;
            case 'C': return 100;
            case 'D': return 500;
            case 'M': return 1000;
            default: return 0;
        }
    }
    
    private boolean match(char a, char b) {
        return (a == 'I' && (b == 'V' || b == 'X')) || 
            (a == 'X' && (b == 'L' || b == 'C')) || 
            (a == 'C' && (b == 'D' || b == 'M'));
    }
}
```

<br/>

## 21. Broken Calculator

[991. Broken Calculator](https://leetcode.com/problems/broken-calculator/) `Medium`

**题目简述：**

给出数字 X，Y，每次可以对 X 执行 “乘二” 或 “减一” 操作，求最少多少次操作后 X 可以变为 Y。

**题目解答：**

当 X 比 Y 大时，必然需要执行 X-Y 次减一操作，反之，X 的操作可以转换为 Y 的反方向操作。

*说明；根据规则，X 通过一定操作能得到 Y 则 Y 一定能反向得到 X。此时，如果 Y 是偶数，则说明上一步 X 做了乘二操作，同理奇数为减一操作*

```java
class Solution {
    public int brokenCalc(int X, int Y) {
        int cnt = 0;
        while (X < Y) {
            cnt++;
            if (Y % 2 == 1) {
                Y++;
            } else {
                Y /= 2;
            }
        }
        return X-Y+cnt;
    }
}
```

<br/>

## 22. Longest Word in Dictionary through Deleting

[524. Longest Word in Dictionary through Deleting](https://leetcode.com/problems/longest-word-in-dictionary-through-deleting/) `Medium`

**题目简述：**

给出字符串 `s` 和字典 `dictionary`，要求通过在字符串中删除任意字符，得到字典中的单词，返回最长的单词，相同长度时返回字典序最小的一个。

**题目解答：**

直接排序遍历解答。

```java
class Solution {
    public String findLongestWord(String s, List<String> d) {
        Collections.sort(d, (s1, s2) -> s1.length()==s2.length() ? s1.compareTo(s2) : s2.length()-s1.length());
        for(String dic : d) {
            if (isMatch(s, dic)) {
                return dic;
            }
        }
        return "";
    }
    
    private boolean isMatch(String s, String d) {
        int i = 0, j = 0;
        while(i < s.length() && j < d.length()) {
            if (s.charAt(i) == d.charAt(j)) {
                j++;
            }
            i++;
        }
        return j == d.length();
    }
}
```

<br/>

## 23. Search a 2D Matrix II

[240. Search a 2D Matrix II](https://leetcode.com/problems/search-a-2d-matrix-ii/) `Medium`

**题目简述：**

给出二维矩阵，每行自左到右递增，每列自上到下递增。求矩阵内是否有某个值 `target`。

**题目解答：**

经典题目，从左下或右上开始查询即可。比如左下开始，小于目标值则右移，大于目标值则上移。

```java
class Solution {
    public boolean searchMatrix(int[][] matrix, int target) {
        int i = matrix.length-1, j = 0;
        while(i >= 0 && j < matrix[0].length) {
            if (matrix[i][j] == target) {
                return true;
            } else if (matrix[i][j] > target) {
                i--;
            } else {
                j++;
            }
        }
        return false;
    }
}
```

<br/>

## 24. Score of Parentheses

[856. Score of Parentheses](https://leetcode.com/problems/score-of-parentheses/) `Medium`

**题目简述：**

给出平衡的括号表达式（仅由 `(`，`)` 构成），计算其分值：

1. `()` 记一分
2. A、B 均为平衡括号表达式，则 `AB` 分值等于 A，B 表达式的分值之和
3. A 为平衡括号表达式，`(A)` 的分支等于 A 表达式的分值乘以二

**题目解答：**

按规则逐步递归计分即可

```java
class Solution {
    public int scoreOfParentheses(String S) {
        if ("".equals(S)) return 0;
        if ("()".equals(S)) return 1;
        
        int l = -1, cnt = 0, ret = 0;
        for(int i = 0; i < S.length(); i++) {
            if (S.charAt(i) == '(') {
                cnt++;
                if (l == -1) l = i;
            }
            else {
                cnt--;
            }
            
            if (cnt == 0) {
                int score = scoreOfParentheses(S.substring(l+1, i));
                ret += score == 0 ? 1 : score * 2;
                l = -1;
            }
        }
        return ret;
    }
}
```

<br/>

## 25. Shortest Unsorted Continuous Subarray

[581. Shortest Unsorted Continuous Subarray](https://leetcode.com/problems/shortest-unsorted-continuous-subarray/) `Medium`

**题目简述：**

给出数组 `arr`，求最少需要对多长的连续子数组重排序，可以使整个数组有序？

**题目解答：**

这道题用了个野蛮的解法，直接排序后比对前后数组。

```java
class Solution {
    public int findUnsortedSubarray(int[] nums) {
        int[] arr = new int[nums.length];
        for(int i = 0; i < nums.length; i++) {
            arr[i] = nums[i];
        }
        Arrays.sort(arr);
        
        int ret = nums.length;
        for(int i = 0; i < nums.length; i++) {
            if (arr[i] == nums[i]) {
                ret--;
            } else {
                break;
            }
        }
        
        for(int i = nums.length-1; i >= 0; i--) {
            if (arr[i] == nums[i]) {
                ret--;
            } else {
                break;
            }
        }
        return ret < 0 ? 0 : ret;
    }
}
```

<br/>

## 26. Validate Stack Sequences

[946. Validate Stack Sequences](https://leetcode.com/problems/validate-stack-sequences/) `Medium`

**题目简述：**

给出入栈序列 `pushed` 与出栈序列 `popped`，判断对于一个空栈，是否可以通过出入栈实现此序列。（序列内所有值均唯一）

**题目解答：**

用一个栈尝试模拟解答即可

```java
class Solution {
    public boolean validateStackSequences(int[] pushed, int[] popped) {
        int n = pushed.length, i = 0, j = 0;
        if (n == 0) return true;
        
        Stack<Integer> stack = new Stack<>();
        stack.push(pushed[i++]);
        while(!stack.empty() || i < n) {
            // 如果栈顶符合，出栈
            if (!stack.empty() && stack.peek() == popped[j]) {
                stack.pop();
                j++;
            } else {
                // 所有数据均已入栈，仍不符合
                if (i >= n) {
                    return false;
                // 数据继续入栈
                } else {
                    stack.push(pushed[i++]);
                }
            }
        }
        return true;
    }
}
```

<br/>

## 27. Divide Two Integers

[29. Divide Two Integers](https://leetcode.com/problems/divide-two-integers/) `Medium`

**题目简述：**

不适用乘、除、取余操作，给出被除数 `dividend` 和除数 `divisor`（两个数均可能为负数），求两个数相除的整数商。（返回 `[−2^31,  2^31 − 1]` 范围内的值，超过则返回范围内最接近的值）

**题目解答：**

不适用乘、除、取余，则只能用最朴素的加法运算。但如果每次只加一格速度过慢，这里每次翻倍以尽快接近目标值。（2 进制思想）

```java
class Solution {
    public int divide(int dividend, int divisor) {
        int flag = (dividend > 0 && divisor < 0 || dividend < 0 && divisor > 0) ? -1 : 1;
        long ret = div(Math.abs((long) dividend), Math.abs((long) divisor));
        
        if (ret > Integer.MAX_VALUE) return flag == 1 ? Integer.MAX_VALUE : Integer.MIN_VALUE;
        else return (int) ret * flag;
    }
    
    public long div(long dividend, long divisor) {
        if (divisor > dividend) return 0;
        
        long mul = 1, sum = divisor;
        while(sum + sum < dividend) {
            sum += sum;
            mul += mul;
        }
        return mul + div(dividend - sum, divisor);
    }
}
```

<br/>

## 28. Maximum Frequency Stack

[895. Maximum Frequency Stack](https://leetcode.com/problems/maximum-frequency-stack/) `Hard`

**题目简述：**

实现 `FreqStack`，以使得 `push(int x)` 方法向栈内增加一个值，`pop()` 移除并返回栈内出现次数最多的值，如果有个多个值满足，则返回最接近栈顶的值。

**题目解答：**

按照出现频次和位置排序各个值即可，这里用优先队列，自定义 `Comparator` 实现。

```java
class FreqStack {
    List<Integer> data = new LinkedList<>();
    Map<Integer, Integer> cntMap = new HashMap<>();
    Queue<Pair<Integer, Integer>> queue = new PriorityQueue(new Comparator<Pair<Integer, Integer>>() { 
        public int compare(Pair<Integer, Integer> a, Pair<Integer, Integer> b) 
        {
            if (a.getKey().equals(b.getKey())) return b.getValue().compareTo(a.getValue());
            else return b.getKey().compareTo(a.getKey());
        }
    });

    public FreqStack() {
        
    }
    
    public void push(int x) {
        int place = data.size();
        data.add(x);
        
        Integer cnt = cntMap.getOrDefault(x, 0)+1;
        cntMap.put(x, cnt);
        queue.offer(new Pair<Integer, Integer>(cnt, place));
    }
    
    public int pop() {
        if (!queue.isEmpty()) {
            Pair<Integer, Integer> pair = queue.poll();
            int x = data.get(pair.getValue());
            cntMap.put(x, cntMap.get(x)-1);
            return x;
        }
        return -1;
    }
}

/**
 * Your FreqStack object will be instantiated and called as such:
 * FreqStack obj = new FreqStack();
 * obj.push(x);
 * int param_2 = obj.pop();
 */
```

<br/>
