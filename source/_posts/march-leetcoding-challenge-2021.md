---
title: March LeetCoding Challenge 2021 解题报告
date: 2021-03-01
tags:
- leetcode
---


## Overview

[March LeetCoding Challenge 2021](https://leetcode.com/explore/featured/card/march-leetcoding-challenge-2021/)

**Week 1: March 1st - March 7th**

1. Single-Row Keyboard `Premium`
2. [Distribute Candies](#1-Distribute-Candies)
3. [Set Mismatch](#2-Set-Mismatch)
4. [Missing Number](#3-Missing-Number)
5. [Intersection of Two Linked Lists](#4-Intersection-of-Two-Linked-Lists)
6. [Average of Levels in Binary Tree](#5-Average-of-Levels-in-Binary-Tree)
7. [Short Encoding of Words](#6-Short-Encoding-of-Words)
8. [Design HashMap](#7-Design-HashMap)

**Week 2: March 8th - March 14th**

1. Strobogrammatic Number `Premium`
2. [Remove Palindromic Subsequences](#9-Add-One-Row-to-Tree)
3. [Add One Row to Tree](#9-Add-One-Row-to-Tree)
4. [Integer to Roman](#10-Integer-to-Roman)
5. [Coin Change](#11-Coin-Change)
6. [Check If a String Contains All Binary Codes of Size K](#12-Check-If-a-String-Contains-All-Binary-Codes-of-Size-K)
7. [Binary Trees With Factors](#13-Binary-Trees-With-Factors)
8. [Swapping Nodes in a Linked List](#14-Swapping-Nodes-in-a-Linked-List)

**Week 3: March 15th - March 21st**

1. Construct Binary Tree from String `Premium`
2. [Encode and Decode TinyURL](#15-Encode-and-Decode-TinyURL)
3. [Best Time to Buy and Sell Stock with Transaction Fee](#16-Best-Time-to-Buy-and-Sell-Stock-with-Transaction-Fee)
4. [Generate Random Point in a Circle](#17-Generate-Random-Point-in-a-Circle)
5. [Wiggle Subsequence](#18-Wiggle-Subsequence)
6. [Keys and Rooms](#19-Keys-and-Rooms)
7. [Design Underground System](#20-Design-Underground-System)
8. [Reordered Power of 2](#21-Reordered-Power-of-2)

**Week 4: March 22nd - March 28th**

1. Find Smallest Common Element in All Rows `Premium`
2. [Vowel Spellchecker](#22-Vowel-Spellchecker)
3. [3Sum With Multiplicity](#23-3Sum-With-Multiplicity)
4. [Advantage Shuffle](#24-Advantage-Shuffle)
5. [Pacific Atlantic Water Flow](#25-Pacific-Atlantic-Water-Flow)
6. [Word Subsets](#26-Word-Subsets)
7. [Palindromic Substrings](#27-Palindromic-Substrings)
8. [Reconstruct Original Digits from English](#28-Reconstruct-Original-Digits-from-English)

**Week 5: March 29th - March 31st**

1. Parallel Courses `Premium`
2. [Flip Binary Tree To Match Preorder Traversal](#29-Flip-Binary-Tree-To-Match-Preorder-Traversal)
3. [Russian Doll Envelopes](#30-Russian-Doll-Envelopes)
4. [Stamping The Sequence](#31-Stamping-The-Sequence)

<br/>

## 1. Distribute Candies

[575. Distribute Candies](https://leetcode.com/problems/distribute-candies) `Easy`

**题目简述：**

Alice 有 n 个（n 为偶数）糖果，提供长度为 n 的数组表示每个糖果的种类，Alice 只能吃 n/2 个糖果，她最多能吃多少种不同的糖果？

**题目解答：**

计算糖果类型数与 n/2 最小值即可

```java
class Solution {
    public int distributeCandies(int[] candyType) {
        Set<Integer> set = new HashSet<>(candyType.length);
        for(int type : candyType) {
            set.add(type);
        }
        
        return Math.min(set.size(), candyType.length/2);
    }
}
```

<br/>


## 2. Set Mismatch

[645. Set Mismatch](https://leetcode.com/problems/set-mismatch/) `Easy`

**题目简述：**

给出长度为 n 的数组，取值为 1..n，但其中一个数字发生错误而被写成了另一个值，导致数组中某个取值出现了两次，而另一个取值未出现，请返回这两个值。（先返回出现两次的值，后返回未出现的值）

**题目解答：**

简陋解法，排序后按顺序寻找对应的值。

```java
class Solution {
    public int[] findErrorNums(int[] nums) {
        Arrays.sort(nums);
        
        int missing = 0, duplicated = 0;
        for(int i = 1; i < nums.length; i++) {
            if (nums[i] == nums[i-1]) {
                duplicated = nums[i];
            } else if (nums[i] != nums[i-1]+1) {
                missing = nums[i-1]+1;
            }
        }
        
        if (missing == 0) missing = nums[0] != 1 ? 1 : nums.length;
        return new int[]{duplicated, missing};
    }
}
```

<br/>

## 3. Missing Number

[268. Missing Number](https://leetcode.com/problems/missing-number/) `Easy`

**题目简述：**

给出长度为 n 的数组，其中的值均唯一且在 [0, n] 范围内，求数组内缺失的值。

**提高：** 能否在 `O(1)` 空间复杂度和 `O(n)` 时间复杂度内实现？

**题目解答：**

和一道经典的题类似，即列表内仅有一个数出现奇数次，而其他数均出现偶数次，求出现奇数次的这个数。方法是使用 XOR 异或，相同的数即可互相抵消，剩下的值就是答案。本题如果将数组内所有值，与 0..n 所有值异或，即与该题一致，消失的数出现一次，其他数均两次。

```java
class Solution {
    public int missingNumber(int[] nums) {
        int missing = nums.length;
        for(int i = 0; i < nums.length; i++) {
            missing ^= i;
            missing ^= nums[i];
        }
        return missing;
    }
}
```

<br/>

## 4. Intersection of Two Linked Lists

[160. Intersection of Two Linked Lists](https://leetcode.com/problems/intersection-of-two-linked-lists/) `Easy`

**题目简述：**

给出两个链表，两个链表在某个节点可能相汇（即指向一个同一个节点），找到并返回该节点，如果没有这样的节点，返回 `null`。

**提高：** 能否在 `O(1)` 空间复杂度和 `O(n)` 时间复杂度内实现？

**题目解答：**

想在 `O(1)` 空间复杂度内实现，显然需要两个节点以不同的方式遍历，并不断比较这两个节点。我最开始的想法是用不同的速度循环遍历两个链表，以比较所有节点，但如何判断结束是个问题，且时间复杂度不止 `O(n)` 。

参考了解答，只需要将 a+b 与 b+a 同时遍历，则如果链表相交，必然在某个位置指向相同节点。

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) {
 *         val = x;
 *         next = null;
 *     }
 * }
 */
public class Solution {
    public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
        ListNode p1 = headA, p2 = headB;
        while(p1 != p2) {
            p1 = p1 == null ? headB : p1.next;
            p2 = p2 == null ? headA : p2.next;
        }
        return p1;
    }
}
```

<br/>

## 5. Average of Levels in Binary Tree

[637. Average of Levels in Binary Tree](https://leetcode.com/problems/average-of-levels-in-binary-tree/) `Easy`

**题目简述：**

给出非空二叉树，求二叉树各行的平均值。（节点值在 32-bit 有符号整数范围内）

**题目解答：**

BFS 层序遍历即可，注意每行总数需要用 `long` 保存。

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
    public List<Double> averageOfLevels(TreeNode root) {
        List<Double> ret = new ArrayList<>();
        if (root == null) return ret;
        
        Queue<Pair<TreeNode, Integer>> queue = new LinkedList<>();
        queue.offer(new Pair<>(root, 1));
        
        long sum = 0;
        int cnt = 0, tier = 1;
        while(!queue.isEmpty()) {
            Pair<TreeNode, Integer> pair = queue.poll();
            TreeNode node = pair.getKey();
            if (node == null) continue;
            
            if (pair.getValue() > tier) {
                ret.add(sum*1.0/cnt);
                sum = 0;
                cnt = 0;
                tier++;
            }
            sum += node.val;
            cnt++;
            
            if (node.left != null) {
                queue.offer(new Pair<>(node.left, pair.getValue()+1));
            }
            if (node.right != null) {
                queue.offer(new Pair<>(node.right, pair.getValue()+1));
            }
        }
        ret.add(sum*1.0/cnt);
        
        return ret;
    }
}
```

<br/>

## 6. Short Encoding of Words

[820. Short Encoding of Words](https://leetcode.com/problems/short-encoding-of-words/) `Medium`

**题目简述：**

对一个字符串数组 `words` 的有效编码 `s`，应能找到索引值数组 `indices` 满足以下条件：

-  `words.length == indices.length`
-  `s` 以 `#` 结尾
-  字符串 `s` 中，以索引值 `indices[i]` 为开始位置，到下一个 `#` 符号之间的子字符串恰好为 `words[i]`.

根据给出的 `words` 找到最短的有效编码字符串 `s` 长度。

**题目解答：**

构造一棵后缀树，并计数各分支最大长度之和

```java
class Solution {
    public int minimumLengthEncoding(String[] words) {
        Node root = new Node('#');
        for(String word : words) {
            buildTree(word, word.length()-1, root);
        }
        
        return conutForTree(root, 0);
    }
    
    private void buildTree(String word, int index, Node node) {
        if (index < 0) return ;
        
        char c = word.charAt(index);
        Node nextNode = node.nexts.get(c);
        if (nextNode == null) {
            nextNode = new Node(c);
            node.nexts.put(c, nextNode);
        }
        buildTree(word, index-1, nextNode);
    }
    
    private int conutForTree(Node node, int start) {
        if (node.nexts.size() == 0) return start+1;
        
        int cnt = 0;
        for(Node nextNode : node.nexts.values()) {
            cnt += conutForTree(nextNode, start+1);
        }
        return cnt;
    }
    
    private static class Node {
        char c;
        Map<Character, Node> nexts;
        
        public Node(char c) {
            this.c = c;
            this.nexts = new HashMap<>();
        }
    }
}
```

<br/>

## 7. Design HashMap

[706. Design HashMap](https://leetcode.com/problems/design-hashmap/) `Easy`

**题目简述：**

实现 MyHashMap 类，使之能够实现以下操作：`MyHashMap()`，`put`，`get`，`remve` 

**题目解答：**

题目中给出了最大范围 0 ~ $10^6$，可以直接用数组实现。当然正规做法是自己实现，链式方法也很容易实现。

```java
class MyHashMap {
    int[] data;

    /** Initialize your data structure here. */
    public MyHashMap() {
        data = new int[1000001];
        Arrays.fill(data, -1);
    }
    
    /** value will always be non-negative. */
    public void put(int key, int value) {
        data[key] = value;
    }
    
    /** Returns the value to which the specified key is mapped, or -1 if this map contains no mapping for the key */
    public int get(int key) {
        return data[key];
    }
    
    /** Removes the mapping of the specified value key if this map contains a mapping for the key */
    public void remove(int key) {
        data[key] = -1;
    }
}

/**
 * Your MyHashMap object will be instantiated and called as such:
 * MyHashMap obj = new MyHashMap();
 * obj.put(key,value);
 * int param_2 = obj.get(key);
 * obj.remove(key);
 */
```

<br/>

## 8. Remove Palindromic Subsequences

[1332. Remove Palindromic Subsequences](https://leetcode.com/problems/remove-palindromic-subsequences/) `Easy`

**题目简述：**

字符串 `s` 仅包含 `a`、`b` 字符，每次可以从字符串中移除一个回文子序列（子序列可以由原字符串任意删除某些字符得到，不要求连续），求最少多次操作后，字符串为空。

**题目解答：**

思考题，因为只有两种字符，如已经是回文串只需一次，否则可以先移除所有的 `a`，再移除所有的 `b` ，两次即可。

```java
class Solution {
    public int removePalindromeSub(String s) {
        if (s.length() == 0) return 0;
        
        for(int i = 0, j = s.length()-1; i < j; i++, j--) {
            if (s.charAt(i) != s.charAt(j)) {
                return 2;
            }
        }
        return 1;
    }
}
```

<br/>

## 9. Add One Row to Tree

[623. Add One Row to Tree](https://leetcode.com/problems/add-one-row-to-tree/) `Medium`

**题目简述：**

给出二叉树根节点 root，要求在 depth 行插入一行新节点（根节点深度为 1），节点值为 val：
- 如果在第 1 行插入，原根节点移动为新节点左树
- 第 depth 层中，原属于上层左树的节点，移动为新节点左树，右树同理

**题目解答：**

深度优先，找到对应行后按规则更新节点即可。

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
    public TreeNode addOneRow(TreeNode root, int v, int d) {
        if (d == 1) {
            return new TreeNode(v, root, null);
        }
        addOneRow(root, 1, v, d);
        return root;
    }

    private void addOneRow(TreeNode node, int depth, int v, int d) {
        if (node == null) return ;

        if (depth == d-1) {
            node.left = new TreeNode(v, node.left, null);
            node.right = new TreeNode(v, null, node.right);
        } else {
            addOneRow(node.left, depth+1, v, d);
            addOneRow(node.right, depth+1, v, d);
        }
    }
}
```

<br/>

## 10. Integer to Roman

[12. Integer to Roman](https://leetcode.com/problems/integer-to-roman/) `Medium`

**题目简述：**

将数字转换为罗马数字字符串

**题目解答：**

罗马数字从字符串到数字还算能明白规则，但到底如何从数字表示，就很难描述情况，最后还是抄了答案。（捂脸）

其实就是优先用更大的单位表示，表示不了时再尝试小单位。

```java
class Solution {
    final static int[]      val = {1000, 900, 500, 400 , 100,   90,  50,  40,   10,    9,   5,    4,   1};
    final static String[] roman = { "M","CM", "D", "CD", "C", "XC", "L", "XL", "X", "IX", "V", "IV", "I"};

    public String intToRoman(int num) {
        StringBuilder sb = new StringBuilder();
        for (int i = 0; num > 0; i++)
            while (num >= val[i]) {
                sb.append(roman[i]);
                num -= val[i];
            }
        return sb.toString();
    }
}
```

<br/>

## 11. Coin Change

[322. Coin Change](https://leetcode.com/problems/coin-change/) `Medium`

**题目简述：**

给出硬币面额列表 coins 和目标金额，求最少可以用多少个硬币（每种面额可使用的次数不限）组成目标金额，如果无法组成目标金额，返回 -1

**题目解答：**

对目标金额 i 来说，dp[i] 的解为 dp[i-coins[j]]+1 的最小值。即减去一个硬币值后的最小解+1

```java
class Solution {
    public int coinChange(int[] coins, int amount) {
        int[] dp = new int[amount + 1];
        Arrays.fill(dp, 10001);
        
        dp[0] = 0;
        for (int i = 1; i <= amount; i++) {
            for (int j = 0; j < coins.length; j++) {
                if (coins[j] <= i) {
                    dp[i] = Math.min(dp[i], dp[i - coins[j]] + 1);
                }
            }
        }
        return dp[amount] >= 10001 ? -1 : dp[amount];
    }
}
```

<br/>

## 12. Check If a String Contains All Binary Codes of Size K    

[1461. Check If a String Contains All Binary Codes of Size K](https://leetcode.com/problems/check-if-a-string-contains-all-binary-codes-of-size-k/) `Medium`

**题目简述：**

给出由 0、1 构成字符串 s，判断字符串内是否包含全部长度为 `k` 的二进制序列

**题目解答：**

最直接的做法是将所有长度为 `k` 的子串扔入 Set，判断总数是否相符，此处做以下优化：

- 将子串转换为数字，节约存储空间
- 基于从 i 开始的子串运算从 i+1 子串的值，优化时间

比如对于字符串 `1101`，`110` 字符串左移并去除最高位即可得到 `(110 << 1) & 111 = 100`， 加上新值即可得到 `100 & 1 = 101`，即 `101` 字符串的数字值

```java
class Solution {
    public boolean hasAllCodes(String s, int k) {
        int n = 1 << k;
        boolean[] visited = new boolean[n];
        int allOne = n-1;
        int hash = 0;
        int cnt = 0;
        
        for(int i = 0; i < s.length(); i++) {
            hash = (hash << 1 & allOne) | s.charAt(i) - '0';
            if (i >= k-1 && !visited[hash]) {
                visited[hash] = true;
                if (++cnt == n) {
                    return true;
                }
            }
        }
        return false;
    }
}
```

<br/>

## 13. Binary Trees With Factors

[823. Binary Trees With Factors](https://leetcode.com/problems/binary-trees-with-factors/) `Medium`

**题目简述：**

给出值不重复且大于 1 的数组 `arr`，要求用数组内的值组成二叉树，满足：任一父节点的值等于其子节点（如有）之乘积，数组内的值可以不限次、不限数量使用，求最多可以组成多少不同的二叉树。(对 10^9 + 7 取余)

**题目解答：**

以 a 为父节点的树总量为，所有乘积为 a 的一组节点，各自作为父节点的解答乘积之和。

最开始用的是求出所有乘积的方式，超时。查询讨论后，发现应该反过来思考，根据某个父节点 a 与某个子节点 b，查找 `a/b` 是否存在。

```java
class Solution {
    private static final int MOD = 1000000007;
    
    public int numFactoredBinaryTrees(int[] arr) {
        Arrays.sort(arr);
        Map<Integer, Integer> map = new HashMap();
        for (int i = 0; i < arr.length; i++) {
            map.put(arr[i], i);
        }
        
        long ret = 0;
        long[] dp = new long[arr.length];
        Arrays.fill(dp, 1l);
        for(int i = 0; i < arr.length; i++) {
            for(int j = 0; j < i; j++) {
                int right;
                if (arr[i] % arr[j] == 0 && map.containsKey(right = arr[i] / arr[j])) {
                    dp[i] = (dp[i] + dp[j] * dp[map.get(right)]) % MOD;
                }
            }
            ret = (ret + dp[i]) % MOD;
        }
        
        return (int) (ret % MOD);
    }
}
```

<br/>

## 14. Swapping Nodes in a Linked List

[1721. Swapping Nodes in a Linked List](https://leetcode.com/problems/swapping-nodes-in-a-linked-list/) `Medium`

**题目简述：**

给定链表，将链表从前到后第 k 个节点与从后到前第 k 个节点交换位置。

**题目解答：**

与求倒数第 k 个节点类似，使用双指针扫描，两个指针相距 k 距离，第一个右指针位置与最后一个左指针位置即为需要交换的节点。

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
    public ListNode swapNodes(ListNode head, int k) {
        ListNode node1 = head, node2 = head;
        ListNode left = head, right = head;
        for(int i = 1; i < k; i++) {
            right = right.next;
            node1 = right;
        }
        
        while(right.next != null) {
            left = left.next;
            right = right.next;
            node2 = left;
        }
        
        int tmp = node1.val;
        node1.val = node2.val;
        node2.val = tmp;
        return head;
    }
}
```

<br/>

## 15. Encode and Decode TinyURL

[535. Encode and Decode TinyURL](https://leetcode.com/problems/encode-and-decode-tinyurl/) `Medium`

**题目简述：**

设计一个短链接生成方法，对相同链接要生成相同的短链接，不同链接则生成不同的链接。

**题目解答：**

这道题一开始想复杂了，恰好几个月内看过知乎如何设计短链接服务的问题，总在考虑是否会存在 Hash 冲突，后来看了下解法才发现根本无关。直观解法，用两个 Map 分别记录每个链接保存映射关系，和短链接到原链接的映射关系。难点说不定是如何生成随机字符串。

```java
public class Codec {
    private static final String host = "http://tinyurl.com/";
    private static final String str = "abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789";
    
    private Map<String, String> shortToOrignMap = new HashMap<>();
    private Map<String, String> orignToShortMap = new HashMap<>();

    // Encodes a URL to a shortened URL.
    public String encode(String longUrl) {
        if (orignToShortMap.containsKey(longUrl)) return orignToShortMap.get(longUrl);
        
        String shortUrl = "";
        while(shortUrl.length() == 0 || shortToOrignMap.containsKey(shortUrl)) {
            shortUrl += str.charAt(new Random().nextInt(62));
        }
        shortToOrignMap.put(shortUrl, longUrl);
        orignToShortMap.put(longUrl, shortUrl);
        return shortUrl;
    }

    // Decodes a shortened URL to its original URL.
    public String decode(String shortUrl) {
        return shortToOrignMap.get(shortUrl.replace(host, ""));
    }
}

// Your Codec object will be instantiated and called as such:
// Codec codec = new Codec();
// codec.decode(codec.encode(url));
```

<br/>

## 16. Best Time to Buy and Sell Stock with Transaction Fee

[417. Best Time to Buy and Sell Stock with Transaction Fee](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-with-transaction-fee/) `Medium`

**题目简述：**

给定股票在一定天数内的价格，不限交易次数，但最多持有一股，同时每轮交易（买入+卖出）需要付出 `fee` 价格的服务费，求能获得的最大收益。

**题目解答：**

用 `cash` 表示第 `i` 天不持有股票时的最大利润，`hold` 表示第 `i` 天持有股票时的最大利润。

在第 `i+1` 天，我们可以选择：

- 出售股票：`cash = max(cash, hold + prices[i] - fee)`
- 购买股票：`hold = max(hold, cash - prices[i])`

很精妙的解法，根据上一天的最优利润，与今天是否买入/卖出的选择结合。但也因为太精妙了因而想不到如何以通用的思路找到这个解答，还需要后续考虑下思考思路。

```java
class Solution {
    public int maxProfit(int[] prices, int fee) {
        int cash = 0, hold = -prices[0];
        for(int i = 1; i < prices.length; i++) {
            cash = Math.max(cash, hold + prices[i] - fee);
            hold = Math.max(hold, cash - prices[i]);
        }
        return cash;
    }
}
```

<br/>

## 17. Generate Random Point in a Circle

[478. Generate Random Point in a Circle](https://leetcode.com/problems/generate-random-point-in-a-circle/) `Medium`

**题目简述：**

给出一个圆心和半径，实现 `randPoint` 方法返回圆内随机一点的坐标

**题目解答：**

这道题考察的点是返回的位置要尽量随机，通用的解法就是，在所有可能范围内尝试，然后剔除那些不符合规则的数据。

```java
class Solution {
    double x, y, r;

    public Solution(double radius, double x_center, double y_center) {
        this.r = radius;
        this.x = x_center;
        this.y = y_center;
    }
    
    public double[] randPoint() {
        double pointX = x - r + Math.random() * 2 * r;
        double pointY = y - r + Math.random() * 2 * r;
        while((pointX - x)*(pointX - x) + (pointY - y)*(pointY - y) >= r*r) {
            pointX = x - r + Math.random() * 2 * r;
            pointY = y - r + Math.random() * 2 * r;
        }
        return new double[]{pointX, pointY};
    }
}

/**
 * Your Solution object will be instantiated and called as such:
 * Solution obj = new Solution(radius, x_center, y_center);
 * double[] param_1 = obj.randPoint();
 */
```

<br/>

## 18. Wiggle Subsequence

[376. Wiggle Subsequence - LeetCode](https://leetcode.com/problems/wiggle-subsequence/) `Medium`

**题目简述：**

如果一个数组前后两个数之差，呈严格正负交错，则称之为摇摆序列。求给定数组内，其非连续子数组为摇摆序列的最大长度。

**题目解答：**

贪心法。画个图会很清晰，本题可以理解为求数组的极大值和极小值的总数，只需要遍历数组，计数所有拐点即可。

```java
class Solution {
    public int wiggleMaxLength(int[] nums) {
        int flag = 0, base = nums[0], ret = 1;
        
        for(int i = 1; i < nums.length; i++) {
            if (flag == 0) {
                if (nums[i] > base) {
                    flag = -1;
                    base = nums[i];
                    ret++;
                } else if (nums[i] < base) {
                    flag = 1;
                    base = nums[i];
                    ret++;
                }
            } else if (flag == 1) {
                if (nums[i] > base) {
                    flag *= -1;
                    base = nums[i];
                    ret++;
                } else {
                    base = nums[i];
                }
            } else {
                if (nums[i] < base) {
                    flag *= -1;
                    base = nums[i];
                    ret++;
                } else {
                    base = nums[i];
                }
            }
        }
        return ret;
    }
}
```

<br/>

## 19. Keys and Rooms

[841. Keys and Rooms](https://leetcode.com/problems/keys-and-rooms/) `Medium`

**题目简述：**

现有 `0..N-1` 号房间和对应的钥匙，给出每间房间内具有的钥匙能打开哪些房间，当前在 0 号房间，判断是否能打开所有房间的门。

**题目解答：**

深度优先搜索：直接递归写法，深度优先搜索，只要注意不要重复搜索即可。

```java
class Solution {
    int[] visited;
    int cnt;
    public boolean canVisitAllRooms(List<List<Integer>> rooms) {
        cnt = rooms.size();
        visited = new int[rooms.size()];
        visitRoom(rooms, 0);
        return cnt == 0;
    }
    
    private void visitRoom(List<List<Integer>> rooms, Integer room) {
        if (visited[room] == 1) return ;
        else {
            visited[room] = 1;
            cnt--;
            for(int nextRoom : rooms.get(room)) {
                visitRoom(rooms, nextRoom);
            }
        }
    }
}
```

<br/>

## 20. Design Underground System

[1396. Design Underground System](https://leetcode.com/problems/design-underground-system/) `Medium`

**题目简述：**

设计一个地下铁路系统，该系统能够按顺序录入某个用户在某个时间进入（checkIn）或离开（checkOut）某个站点，同时提供 getAverageTime 方法以计算某两个地点的平均花费时间。

**题目解答：**

朴素解法：用两个 Map 分别记录某个用户的当前进入站点的信息，和任意两个站点之间的总时间和人数。出站时更新相关时间/人数，并用此 Map 提供平均值计算。

```java
class UndergroundSystem {
    HashMap<Integer, Pair<String, Integer>> idMap;
    HashMap<Pair<String, String>, Pair<Integer, Integer>> travelMap;

    public UndergroundSystem() {
        idMap = new HashMap<>();
        travelMap = new HashMap<>();
    }
    
    public void checkIn(int id, String stationName, int t) {
        idMap.put(id, new Pair<>(stationName, t));
    }
    
    public void checkOut(int id, String stationName, int t) {
        Pair<String, Integer> pair = idMap.get(id);
        Pair<String, String> travel = new Pair<>(pair.getKey(), stationName);
        
        Pair<Integer, Integer> data = travelMap.getOrDefault(travel, new Pair<>(0, 0));
        travelMap.put(
            travel,
            new Pair<>(data.getKey() + (t-pair.getValue()), data.getValue()+1)
        );
    }
    
    public double getAverageTime(String startStation, String endStation) {
        Pair<Integer, Integer> data = travelMap.get(new Pair<>(startStation, endStation));
        return data.getKey() * 1.0 / data.getValue();
    }
}

/**
 * Your UndergroundSystem object will be instantiated and called as such:
 * UndergroundSystem obj = new UndergroundSystem();
 * obj.checkIn(id,stationName,t);
 * obj.checkOut(id,stationName,t);
 * double param_3 = obj.getAverageTime(startStation,endStation);
 */
```

<br/>

## 21. Reordered Power of 2

[869. Reordered Power of 2](https://leetcode.com/problems/reordered-power-of-2/) `Medium`

**题目简述：**

给出一个整数，如果这个整数以任何顺序重排序（十进制分隔方式）后为 2 的倍数，则返回 true，否则返回 false。

**题目解答：**

将数字转换为每个数字的出现次数，并检查是否有 2 的倍数的格数字出现次数与之符合。

```java
class Solution {
    public boolean reorderedPowerOf2(int N) {
        int[] arr = process(N);
        for(int i = 0; i < 31; i++) {
            if (Arrays.equals(arr, process(1 << i))) {
                return true;
            }
        }
        return false;
    }
    
    private int[] process(int n) {
        int[] cnt = new int[10];
        while(n != 0) {
            cnt[n%10] ++;
            n = n / 10;
        }
        return cnt;
    }
}
```

<br/>

## 22. Vowel Spellchecker

[966. Vowel Spellchecker](https://leetcode.com/problems/vowel-spellchecker/) `Medium`

**题目简述：**

题目给出单词列表和查询单词列表，按如下规则在单词列表中寻找与查询单词匹配的词（优先级从高到底）：

- 如果单词表中存在与查询词完全相同的词（大小写也完全相同），则匹配
- 如果单词表中存在与查询词仅大小写不同的词，则匹配首个符合的单词
- 如果单词表中存在，将查询词中的元音（aeiou）任意替换为另一个元音后的词（大小写不敏感），则匹配首个符合的单词
- 以上均不匹配，返回空字符串

**题目解答：**

使用 HashMap 存储以上三种优先级的单词关系即可。

```java
class Solution {
    public String[] spellchecker(String[] wordlist, String[] queries) {
        Set<String> sameSet = new HashSet<>();
        Map<String, String> capMap = new HashMap<>(), vowelMap = new HashMap<>();
        for(String word : wordlist) {
            sameSet.add(word);

            String lowerString = word.toLowerCase();
            if (!capMap.containsKey(lowerString)) {
                capMap.put(lowerString, word);
            }
            String vowelString = lowerString.replaceAll("[aeiou]", "?");
            if (!vowelMap.containsKey(vowelString)) {
                vowelMap.put(vowelString, word);
            }
        }

        String[] ret = new String[queries.length];
        for(int i = 0; i < queries.length; i++) {
            String query = queries[i];
            if (sameSet.contains(query)) {
                ret[i] = query;
                continue;
            }
            String lowerString = query.toLowerCase();
            if (capMap.containsKey(lowerString)) {
                ret[i] = capMap.get(lowerString);
                continue;
            }
            String vowelString = lowerString.replaceAll("[aeiou]", "?");
            if (vowelMap.containsKey(vowelString)) {
                ret[i] = vowelMap.get(vowelString);
                continue;
            }
            ret[i] = "";
        }
        return ret;
    }
}
```

<br/>

## 23. 3Sum With Multiplicity

[923. 3Sum With Multiplicity](https://leetcode.com/problems/3sum-with-multiplicity/) `Medium`

**题目简述：**

在给定数组内，找到 i,j,k 三个值满足 `i < j < k` 且 `arr[i] + arr[j] + arr[k] == target`，求解的总数，并对 $10^9 + 7$ 取余。

**题目解答：**

三指针扫描：对每个 i 尝试找 j、k 的取值，扫描所有组合。

```java
class Solution {
    public int threeSumMulti(int[] arr, int target) {
        long ret = 0;
        Arrays.sort(arr);
        for(int i = 0; i < arr.length; i++) {
            int t = target - arr[i], j = i+1, k = arr.length-1;
            while(j < k) {
                if (arr[j] + arr[k] > t) {
                    k--;
                } else if (arr[j] + arr[k] < t) {
                    j++;
                } else if (arr[j] != arr[k]) {
                    int l = 1, r = 1;
                    while(j+1 < k && arr[j] == arr[j+1]) {
                        l++;
                        j++;
                    }
                    while(j < k-1 && arr[k-1] == arr[k]) {
                        r++;
                        k--;
                    }
                    ret = (ret + l * r) % 1000000007;
                    j++;
                    k--;
                } else {
                    ret = (ret + ((k-j+1) * (k-j) / 2)) % 1000000007;
                    break;
                }  
            }
        }
        return (int) ret;
    }
}
```

<br/>

## 24. Advantage Shuffle

[870. Advantage Shuffle](https://leetcode.com/problems/advantage-shuffle/) `Medium`

**题目简述：**

给出 A、B 两个长度相等的数组，返回 A 的任意重排序结果（如有多个解），使之 A、B 在所有索引位置上 `A[i] > B[i]` 的数量最多。

**题目解答：**

对 B 数组中每个值，按从小到大的顺序，取 A 数组中尽量小的但大于 B 数组对应值的值即可。

```java
class Solution {
    public int[] advantageCount(int[] A, int[] B) {
        int[] Acopy = new int[A.length], Bcopy = new int[B.length];
        System.arraycopy(A, 0, Acopy, 0, A.length);
        System.arraycopy(B, 0, Bcopy, 0, B.length);
        Arrays.sort(Acopy);
        Arrays.sort(Bcopy);

        Map<Integer, List<Integer>> map = new HashMap<>();
        for(int i = 0, l = 0, r = B.length - 1; i < Acopy.length; i++) {

            if (Acopy[i] > Bcopy[l]) {
                List<Integer> list = map.getOrDefault(Bcopy[l], new ArrayList<>());
                list.add(Acopy[i]);
                map.put(Bcopy[l++], list);
            } else {
                List<Integer> list = map.getOrDefault(Bcopy[r], new ArrayList<>());
                list.add(Acopy[i]);
                map.put(Bcopy[r--], list);
            }
        }

        for(int i = 0; i < Acopy.length; i++) {
            List<Integer> list = map.getOrDefault(B[i], new ArrayList<>());
            A[i] = list.get(list.size()-1);
            list.remove(list.size()-1);
        }
        return A;
    }
}
```

<br/>

## 25. Pacific Atlantic Water Flow

[417. Pacific Atlantic Water Flow](https://leetcode.com/problems/pacific-atlantic-water-flow/) `Medium`

**题目简述：**

给出一个正数二维数组，数组中的值表示该位置的高度，水流可以从某个位置以上下左右方向流向高度相等或更低的相邻位置。已知在数组左侧和上测是河水 A，右侧和下侧是河水 B，求在哪些坐标放水，可以使水同时流向河水 A 和河水 B。（返回坐标不要求顺序）

**题目解答：**

宽度优先搜索：沿着左右两个方向分别进行一轮深度优先搜索，各自找到可以流向河水 A、B 的坐标，再找并集即可。

```java
class Solution {
    private int n, m;
    public List<List<Integer>> pacificAtlantic(int[][] matrix) {
        if (matrix.length == 0 || matrix[0].length == 0) {
            return Collections.emptyList();
        }
        n = matrix.length;
        m = matrix[0].length;

        boolean[][] lArea = new boolean[n][m], rArea = new boolean[n][m];
        Queue<Pair<Integer, Integer>> queue = new LinkedList<>();
        for(int i = 0; i < n; i++) {
            for(int j = 0; j < m; j++) {
                if (i == 0 || j == 0) {
                    lArea[i][j] = true;
                    queue.offer(new Pair(i, j));
                }                
            }
        }
        process(matrix, queue, lArea);


        for(int i = 0; i < n; i++) {
            for(int j = 0; j < m; j++) {
                if (i == n-1 || j == m-1) {
                    rArea[i][j] = true;
                    queue.offer(new Pair(i, j));
                }                
            }
        }
        process(matrix, queue, rArea);

        List<List<Integer>> ret = new ArrayList<>();
        for(int i = 0; i < n; i++) {
            for(int j = 0; j < m; j++) {
                if (lArea[i][j] && rArea[i][j]) {
                    ret.add(Arrays.asList(i, j));
                }
            }
        }
        return ret;
    }

    private int[][] flows = new int[][]{{-1, 0}, {0, -1}, {1, 0}, {0, 1}};
    private void process(int[][] matrix, Queue<Pair<Integer, Integer>> queue, boolean[][] visited) {
        while(!queue.isEmpty()) {
            Pair<Integer, Integer> pair = queue.poll();
            int i = pair.getKey(), j = pair.getValue();
            visited[i][j] = true;

            for(int[] flow : flows) {
                int nextI = i + flow[0], nextJ = j + flow[1];
                if (0 <= nextI && nextI < n && 0 <= nextJ && nextJ < m && 
                    !visited[nextI][nextJ] && matrix[nextI][nextJ] >= matrix[i][j]) {
                    visited[nextI][nextJ] = true;
                    queue.offer(new Pair(nextI, nextJ));
                }
            }
        }
    }
}
```

<br/>

## 26. Word Subsets

[916. Word Subsets](https://leetcode.com/problems/word-subsets/) `Medium`

**题目简述：**

定义如果 a 字符串包含 b 中的所有字符（包括多次，即 a 中每个字符的次数必须大于等于 b 中的对应字符次数），则 b 是 a 的子集。返回数组 A 中，数组 B 中所有字符串均是它的子集的字符串（不要求返回顺序）。

**题目解答：**

统计 B 数组中每个字符的最大字符出现次数，如果某个字符串所有字符串的出现次数均大于等于该次数即认为是符合条件的。

```java
class Solution {
    public List<String> wordSubsets(String[] A, String[] B) {
        int[] bArr = new int[27], arr = new int[27];;
        for(String b : B) {
            Arrays.fill(arr, 0);
            for(char c : b.toCharArray()) {
                arr[c - 'a'] += 1;
                if (arr[c - 'a'] > bArr[c - 'a']) {
                    bArr[c - 'a'] = arr[c - 'a'];
                }
            }
        }

        List<String> ret = new ArrayList<>();
        for(String a : A) {
            Arrays.fill(arr, 0);
            for(char c : a.toCharArray()) {
                arr[c - 'a'] += 1;
            }

            boolean isUniversal = true;
            for(int i = 0; i < 27; i++) {
                if (arr[i] < bArr[i]) {
                    isUniversal = false;
                    break;
                }
            }
            if (isUniversal) {
                ret.add(a);
            }
        }
        return ret;
    }
}
```

<br/>

## 27. Palindromic Substrings

[647. Palindromic Substrings](https://leetcode.com/problems/palindromic-substrings/) `Medium`

**题目简述：**

求给定字符串内有多少个回文串，其中位置不同但内容相同的回文串视为不同的解。

**题目解答：**

使用 Manacher 解法，并处理 # 情况即可。

```java
class Solution {
    public int countSubstrings(String s) {
        StringBuilder sb = new StringBuilder("#");
        for(char c : s.toCharArray()) {
            sb.append(c);
            sb.append("#");
        }
        s = sb.toString();

        int right = -1, j = -1, ret = 0;
        int[] arr = new int[s.length()];
        for(int i = 0; i < s.length(); i++) {
            int len;
            if (i < right) {
                int minLen = Math.min(arr[2*j-i], right-i);
                len = expand(s, i, minLen);
            } else {
                len = expand(s, i, 0);
            }
            arr[i] = len;

            if (i + len > right) {
                right = i + len;
                j = i;
            }
            ret += s.charAt(i) == '#' ? arr[i]/2 : arr[i]/2+1;
        }
        return ret;
    }

    private int expand(String s, int i, int len) {
        for(len=len+1; i-len >= 0 && i+len < s.length(); len++) {
            if (s.charAt(i-len) != s.charAt(i+len)) {
                return len-1;
            }
        }
        return len-1;
    }
}
```

<br/>

## 28. Reconstruct Original Digits from English

[423. Reconstruct Original Digits from English](https://leetcode.com/problems/reconstruct-original-digits-from-english/) `Medium`

**题目简述：**

给出由 0-9 的英语单词组成，但顺着被打乱的字符串。（保证输入有效）

**题目解答：**

直接找数组规律，比如 zero 的出现次数就是 z 字符的出现次数。

```java
class Solution {

    public String originalDigits(String s) {
        int[] letterCnt = new int[27];
        for(char c : s.toCharArray()) {
            letterCnt[c - 'a'] += 1;
        }

        int[] numsCnt = new int[10];
        numsCnt[0] = letterCnt['z'-'a'];    // zero = 'z'
        numsCnt[1] = letterCnt['o'-'a'] - letterCnt['z'-'a'] - letterCnt['w'-'a'] - letterCnt['u'-'a'];
                                            // one  = 'o' - 'z' - 'w' - 'u'
        numsCnt[2] = letterCnt['w'-'a'];    // two  = 'w'
        numsCnt[3] = letterCnt['r'-'a'] - letterCnt['u'-'a'] - letterCnt['z'-'a'];
                                            // three= 'r' - 'u' - 'z'
        numsCnt[4] = letterCnt['u'-'a'];    // four = 'u'
        numsCnt[5] = letterCnt['f'-'a'] - letterCnt['u'-'a'];
                                            // five = 'f' - 'u'
        numsCnt[6] = letterCnt['x'-'a'];    // six  = 'x'
        numsCnt[7] = letterCnt['s'-'a'] - letterCnt['x'-'a'];
                                            // seven= 's' - 'x'
        numsCnt[8] = letterCnt['g'-'a'];    // eight= 'g'
        numsCnt[9] = letterCnt['i'-'a'] - letterCnt['f'-'a'] + letterCnt['u'-'a'] - letterCnt['x'-'a'] - letterCnt['g'-'a'];
                                            // nine = 'i' - ('f' - 'u') - 'x' - 'g'

        StringBuilder sb = new StringBuilder();
        for(int i = 0; i < 10; i++) {
            for(int j = 0; j < numsCnt[i]; j++) {
                sb.append(i);
            }
        }
        return sb.toString();
    }
}
```

<br/>

## 29. Flip Binary Tree To Match Preorder Traversal

[971. Flip Binary Tree To Match Preorder Traversal](https://leetcode.com/problems/flip-binary-tree-to-match-preorder-traversal/) `Medium`

**题目简述：**

给出二叉树，允许将二叉树某个节点的左右节点交换，求最少需要对哪些节点的子节点交换，能使得二叉树的先序遍历与目标数组一致，如果不可能一致，则返回 [-1]。（二叉树每个节点的值唯一，均为 0-n 范围，且保证节点数与数组长度一致）

**题目解答：**

因为是先序优先遍历，只需要按照要求深度优先尝试匹配即可。

以下解法参考了 LeetCode 官方题解，最早的解法判断条件有些绕，且实际上并不需要考虑真的交换节点。

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
    private List<Integer> ans = new ArrayList<>();
    private boolean isFailed = false;
    private int i = 0;

    public List<Integer> flipMatchVoyage(TreeNode root, int[] voyage) {
        matchs(root, voyage);
        return isFailed ? Arrays.asList(-1) : ans;
    }

    private void matchs(TreeNode node, int[] voyage) {
        if (node == null) return ;

        if (node.val != voyage[i++]) {
            isFailed = true;
            return ;
        }

        if (node.left == null && node.right == null) {
            return ;
        }

        if (node.left != null && node.left.val != voyage[i]) {
            ans.add(node.val);
            matchs(node.right, voyage);
            matchs(node.left, voyage);
        } else {
            matchs(node.left, voyage);
            matchs(node.right, voyage);
        }
    }
}
```

<br/>

## 30. Russian Doll Envelopes

[354. Russian Doll Envelopes](https://leetcode.com/problems/russian-doll-envelopes/) `Hard`

**题目简述：**

给出一系列信封的宽高，如果一个信封的宽高均大于另一个信封，则这两个信封可以嵌套，求最多可以有多少个信封互相嵌套。（信封不可旋转）

**题目解答：**

动态规划：`dp[i]` 表示位置 i 为最后一个信封时的最大嵌套数量。

```java
class Solution {
    public int maxEnvelopes(int[][] envelopes) {
        Arrays.sort(envelopes, new Comparator<int[]>() {
            @Override
            public int compare(int[] a, int[] b) {
                return a[0] == b[0] ? a[1]-b[1] : a[0]-b[0];
            }
        });

        int ans = 1;
        int[] dp = new int[envelopes.length];
        Arrays.fill(dp, 1);
        for(int i = 0; i < envelopes.length; i++) {
            for(int j = 0; j < i; j++) {
                if (envelopes[i][0] > envelopes[j][0] && envelopes[i][1] > envelopes[j][1]) {
                    dp[i] = Math.max(dp[i], dp[j] + 1);
                }
            }
            ans  = Math.max(ans, dp[i]);
        }
        return ans;
    }
}
```

<br/>

## 31. Stamping The Sequence

[936. Stamping The Sequence - LeetCode](https://leetcode.com/problems/stamping-the-sequence/) `Hard`

**题目简述：**

给出一个印章字符串 `stamp` 和目标字符串 `target`，每次可以在 `target.length` 长度的字符串上进行盖章（不能超出边界），以使得目标范围的字符更新为印章值。求返回在 `10 * target.length` 次数内，按顺序从哪个位置盖章可以形成目标字符串。（可返回任一有效解）

**题目解答：**

（官解）按题意，后面的印章会覆盖已有的印记，因而正序考虑很困难，逆推则简单得多。

`List<Node> A` 表示每个起始位置的窗口范围内，哪些位置匹配目标字符串（`made`），哪些不匹配（`todo`）

- 一开始，我们找完整匹配印章的字符串，找到后匹配的这些位置可以认为匹配任意字符。
- 遍历影响到的窗口范围（`List<Node> A `），如果这些窗口移除可以匹配任意字符的范围后，可以匹配目标字符串，则匹配成功，再次移除该印章范围，持续判断下去。

注意本解法仅保证能得出有效解答，而不保证最优解。

```java
class Solution {
    public int[] movesToStamp(String stamp, String target) {
        int M = stamp.length(), N = target.length();
        Queue<Integer> queue = new ArrayDeque();
        boolean[] done = new boolean[N];
        Stack<Integer> ans = new Stack();
        List<Node> A = new ArrayList();

        for (int i = 0; i <= N-M; ++i) {
            // For each window [i, i+M), A[i] will contain
            // info on what needs to change before we can
            // reverse stamp at this window.

            Set<Integer> made = new HashSet();
            Set<Integer> todo = new HashSet();
            for (int j = 0; j < M; ++j) {
                if (target.charAt(i+j) == stamp.charAt(j))
                    made.add(i+j);
                else
                    todo.add(i+j);
            }

            A.add(new Node(made, todo));

            // If we can reverse stamp at i immediately,
            // enqueue letters from this window.
            if (todo.isEmpty()) {
                ans.push(i);
                for (int j = i; j < i + M; ++j) if (!done[j]) {
                    queue.add(j);
                    done[j] = true;
                }
            }
        }

        // For each enqueued letter (position),
        while (!queue.isEmpty()) {
            int i = queue.poll();

            // For each window that is potentially affected,
            // j: start of window
            for (int j = Math.max(0, i-M+1); j <= Math.min(N-M, i); ++j) {
                if (A.get(j).todo.contains(i)) {  // This window is affected
                    A.get(j).todo.remove(i);
                    if (A.get(j).todo.isEmpty()) {
                        ans.push(j);
                        for (int m: A.get(j).made) if (!done[m]) {
                            queue.add(m);
                            done[m] = true;
                        }
                    }
                }
            }
        }

        for (boolean b: done)
            if (!b) return new int[0];

        int[] ret = new int[ans.size()];
        int t = 0;
        while (!ans.isEmpty())
            ret[t++] = ans.pop();

        return ret;
    }
}

class Node {
    Set<Integer> made, todo;
    Node(Set<Integer> m, Set<Integer> t) {
        made = m;
        todo = t;
    }
}
```

<br/>
