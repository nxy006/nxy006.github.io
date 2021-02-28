---
title: January LeetCoding Challenge 2021 解题报告
date: 2021-01-01
tags:
- leetcode
---


## Overview

[January LeetCoding Challenge 2021](https://leetcode.com/explore/challenge/card/january-leetcoding-challenge-2021/)

**Week 1: January 1st - January 7th**

1. Palindrome Permutation `Premium`
2. [Check Array Formation Through Concatenation](#1-Check-Array-Formation-Through-Concatenation)
3. [Find a Corresponding Node of a Binary Tree in a Clone of That Tree](#2-Find-a-Corresponding-Node-of-a-Binary-Tree-in-a-Clone-of-That-Tree)
4. [Beautiful Arrangement](#3-Beautiful-Arrangement)
5. [Merge Two Sorted Lists](#4-Merge-Two-Sorted-Lists)
6. [Remove Duplicates from Sorted List II](#5-Remove-Duplicates-from-Sorted-List-II)
7. [Kth Missing Positive Number](#6-Kth-Missing-Positive-Number)
8. [Longest Substring Without Repeating Characters](#7-Longest-Substring-Without-Repeating-Characters)

**Week 2: January 8th - January 14th**

1. Find Root of N-Ary Tree `Premium`
2. [Check If Two String Arrays are Equivalent](#8-Check-If-Two-String-Arrays-are-Equivalent)
3. [Word Ladder](#9-Word-Ladder)
4. [Create Sorted Array through Instructions](#10-Create-Sorted-Array-through-Instructions)
5. [Merge Sorted Array](#11-Merge-Sorted-Array)
6. [Add Two Numbers](#12-Add-Two-Numbers)
7. [Boats to Save People](#13-Boats-to-Save-People)
8. [Minimum Operations to Reduce X to Zero](#14-Minimum-Operations-to-Reduce-X-to-Zero)

**Week 3: January 15th - January 21st**

1. Nested List Weight Sum `Premium`
2. [Get Maximum in Generated Array](#15-Get-Maximum-in-Generated-Array)
3. [Kth Largest Element in an Array](#16-Kth-Largest-Element-in-an-Array)
4. [Count Sorted Vowel Strings](#17-Count-Sorted-Vowel-Strings)
5. [Max Number of K-Sum Pairs](#18-Max-Number-of-K-Sum-Pairs)
6. [Longest Palindromic Substring](#19-Longest-Palindromic-Substring)
7. [Valid Parentheses](#20-Valid-Parentheses)
8. [Find the Most Competitive Subsequence](#21-Find-the-Most-Competitive-Subsequence)

**Week 4: January 22nd - January 28th**

1. One Edit Distance `Premium`
2. [Determine if Two Strings Are Close](#22-Determine-if-Two-Strings-Are-Close)
3. [Sort the Matrix Diagonally](#23-Sort-the-Matrix-Diagonally)
4. [Merge k Sorted Lists](#24-Merge-k-Sorted-Lists)
5. [Check If All 1's Are at Least Length K Places Away](#25-Check-If-All-1’s-Are-at-Least-Length-K-Places-Away)
6. [Path With Minimum Effort](#26-Path-With-Minimum-Effort)
7. [Concatenation of Consecutive Binary Numbers](#27-Concatenation-of-Consecutive-Binary-Numbers)
8. [Smallest String With A Given Numeric Value](#28-Smallest-String-With-A-Given-Numeric-Value)

**Week 5: January 29th - January 31st**

1. Number Of Corner Rectangles `Premium`
2. [Vertical Order Traversal of a Binary Tree](#29-Vertical-Order-Traversal-of-a-Binary-Tree)
3. [Minimize Deviation in Array](#30-Minimize-Deviation-in-Array)
4. [Next Permutation](#31-Next-Permutation)

<br/>

## 1. Check Array Formation Through Concatenation

[1640. Check Array Formation Through Concatenation](https://leetcode.com/problems/check-array-formation-through-concatenation/) `Easy`

**题目简述：**

给出数组 `arr` 及二维数组 `pieces`，判断数组 `arr` 是否可以拆分且重排序为二维数组 `pieces`，注意拆分后的子数组不可重排序。（值不重复）

*例如：`[91,4,64,78]` 可以划分为 `[91], [4, 64], [78]`，因而可以组合为 `[[78],[4,64],[91]]`，但不能组成 `[[78],[64, 4],[91]]`*

**题目解答：**

遍历 arr 数组并尝试用 pieces 数据拟合，一旦与 pieces 的首个值匹配，则接下来的每个值都要与 piece 子数组的值一致。

```java
class Solution {
    public boolean canFormArray(int[] arr, int[][] pieces) {
        Map<Integer, Integer> piecesIndexMap = new HashMap<>(pieces.length);
        for(int i = 0; i < pieces.length; i++) {
            piecesIndexMap.put(pieces[i][0], i);
        }

        int i = -1, j = -1;
        for(int num : arr) {
            if (j == -1 || j >= pieces[i].length) {
                if (!piecesIndexMap.containsKey(num)) {
                    return false;
                }
                i = piecesIndexMap.get(num);
                j = 0;
            }
            
            if (num != pieces[i][j]) {
                return false;
            }
            j++;
        }
        return true;
    }
}
```

<br/>


## 2. Find a Corresponding Node of a Binary Tree in a Clone of That Tree

[1379. Find a Corresponding Node of a Binary Tree in a Clone of That Tree](https://leetcode.com/problems/find-a-corresponding-node-of-a-binary-tree-in-a-clone-of-that-tree/) `Medium`

**题目简述：**

给出二叉树及其深拷贝树，查找二叉树节点 targer 在深拷贝节点树内的对应节点。（节点值不重复）

**Follow up:** 如果二叉树内有重复节点呢？

**题目解答：**

以相同方式遍历两颗树即可，相对位置相同则为对应节点。

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
    public final TreeNode getTargetCopy(final TreeNode  original, final TreeNode cloned, final TreeNode target) {
        if (target == original) return cloned;
        
        if (original.left != null) {
            TreeNode ret = getTargetCopy(original.left, cloned.left, target);
            if (ret != null) return ret;
        }
        if (original.right != null) {
            TreeNode ret = getTargetCopy(original.right, cloned.right, target);
            if (ret != null) return ret;
        }
        return null;
    }
}
```

<br/>

## 3. Beautiful Arrangement

[526. Beautiful Arrangement](https://leetcode.com/problems/beautiful-arrangement/) `Medium`

**题目简述：**

使用 1...n 整数，计算可以构成多少种 *漂亮安排*。漂亮安排指的是对任意 `i (1<=i<=n)`, `perm[i]` 可以整除 `i` ，或 `i` 可以被 `perm[i]` 整除。

**题目解答：**

n 最大只到 15，递归方式遍历检查

```java
class Solution {
    private int count = 0;
    private boolean[] visited;

    public int countArrangement(int n) {
        List<Integer> list = new ArrayList<>(n);
        visited = new boolean[n];
        for(int i = 0; i < n; i++) {
            visited[i] = false;
            list.add(i+1);
        }
        nextArray(list, n, 0);
        return count;
    }
    
    private void nextArray(List<Integer> list, int n, int index) {
        if (index >= n) {
            count++;
        }
        
        for(int i = 1; i <= n; i++) {
            if (!visited[i-1] && (i % (index+1) == 0 || (index+1) % i == 0)) {
                visited[i-1] = true;
                list.set(index, i);
                nextArray(list, n, index+1);
                visited[i-1] = false;
            }
        }
    }
}
```

<br/>

## 4. Merge Two Sorted Lists

[21. Merge Two Sorted Lists](https://leetcode.com/problems/merge-two-sorted-lists/) `Easy`

**题目简述：**

合并两个非降序链表为一个非降序链表。

**题目解答：**

递归方式查找下一个节点。

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
    public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
        if (l1 == null && l2 == null) return null;
        if (l1 == null) return l2;
        if (l2 == null) return l1;
        
        ListNode node;
        if (l1.val < l2.val) {
            node = l1;
            node.next = mergeTwoLists(l1.next, l2);
        }
        else {
            node = l2;
            node.next = mergeTwoLists(l1, l2.next);
        }
        return node;
    }
}
```

<br/>

## 5. Remove Duplicates from Sorted List II

[82. Remove Duplicates from Sorted List II](https://leetcode.com/problems/remove-duplicates-from-sorted-list-ii/) `Medium`

**题目简述：**

给出一个已排序链表，将其重复值的节点删除，剩余节点排序不变（仍然有序）。

**题目解答：**

常规解法，记录下当前值，如果后续出现重复则移动节点关系。代码写的有点复杂了，还能更清晰。

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
    public ListNode deleteDuplicates(ListNode head) {
        if (head == null) return head;
        
        ListNode node = null, ret = null, prevNode = null, currentNode = head;
        while(currentNode != null) {
            if ((prevNode == null || prevNode.val != currentNode.val) && (currentNode.next == null || currentNode.next.val != currentNode.val)) {
                if (node == null) {
                    node = currentNode;
                    ret = node;
                } else {
                    node.next = currentNode;
                    node = node.next;
                }
            }
            prevNode = currentNode;
            currentNode = currentNode.next;
            
            
        }
        if (node != null && node.next != null) node.next = null;
        
        return ret;
    }
}
```

<br/>

## 6. Kth Missing Positive Number

[1539. Kth Missing Positive Number](https://leetcode.com/problems/kth-missing-positive-number/) `Easy`

**题目简述：**

给出一个严格递增的正数数组，求不包含在数组内的第 k 个正数。

**题目解答：**

这道题范围仅 1000 级别，可以直接强行遍历每个数组，这里是用 arr 的数值差进行计算的。

**优化：** 或许可以用二分法来做，思路是对于第一个满足 `arr[i] > i + k` 的 `i` 值，可以直接用 `k - (i-1)` 获得结果。

```java
class Solution {
    public int findKthPositive(int[] arr, int k) {
        int count = 0;
        for(int i = 0; i < arr.length; i++) {
            int cnt = (i == 0) ? (arr[i] - 1) : (arr[i] - arr[i-1] - 1);
            if (count + cnt >= k) {
                return (i == 0 ? 0 : arr[i-1]) + (k - count);
            }
            count += cnt;
        }
        return arr[arr.length - 1] + (k - count);
    }
}
```

<br/>

## 7. Longest Substring Without Repeating Characters

[3. Longest Substring Without Repeating Characters](https://leetcode.com/problems/longest-substring-without-repeating-characters/) `Medium`

**题目简述：**

在给定字符串内，寻找最长连续子字符串，该字符串内不含重复字符。

**题目解答：**

双指针扫描，用 set 记录当前字符列表，如果遇到重复字符则移动左节点直到不再有重复字符。

**优化；** 记录每个字符最后出现位置，以判断是否重复，并在重复时快速找到新的开始位置，`O(n)` 复杂度。

```java
class Solution {
    public int lengthOfLongestSubstring(String s) {
        Set<Character> set = new HashSet<>();
        int ret = 0;
        int i = 0, j = 0;
        while(j < s.length()) {
            if (!set.contains(s.charAt(j))) {
                set.add(s.charAt(j));
                if (j-i+1 > ret) ret = j-i+1;
            } else {
                while(i < j) {
                    if (s.charAt(i) == s.charAt(j)) {
                        i++;
                        break;
                    } else {
                        set.remove(s.charAt(i));
                    }
                    i++;
                }
            }
            j++;
        }
        return ret;
    }
}
```

<br/>

## 8. Check If Two String Arrays are Equivalent

[1662. Check If Two String Arrays are Equivalent](https://leetcode.com/problems/check-if-two-string-arrays-are-equivalent/) `Easy`

**题目简述：**

给出两个字符串列表，判断两个列表将每个字符串按顺序拼接后，是否是相同的字符串。

**题目解答：**

直接字符串比较

```java
class Solution {
    public boolean arrayStringsAreEqual(String[] word1, String[] word2) {      
        String a = "";
        for(String word: word1) {
            a += word;
        }
        String b = "";
        for(String word: word2) {
            b += word;
        }
        
        return a.equals(b);
    }
}
```

<br/>

## 9. Word Ladder

[127. Word Ladder](https://leetcode.com/problems/word-ladder/) `Hard`

**题目简述：**

对多个有序词汇，每相邻两个词汇都恰好仅有一个字符不同，且均属于词汇表，则这个序列成称为词汇序列。求根据给出的词汇表，和开始/结束词汇，计算最少需要多少个词汇，以形成词汇序列，使开始词汇，最终转换为结束词汇。（如无有效序列，返回 0）

**题目解答：**

直接的想法是构造一个图，检查所有词汇间的转换关系，然后从开始词汇进行宽度优先搜索。但检查转换关系依然比较耗时，需要遍历所有两个词汇组合，此处优化为类似通配符的形式。

```java
class Solution {
    public int ladderLength(String beginWord, String endWord, List<String> wordList) {
        if (!wordList.contains(endWord)) {
            return 0;
        }
        
        Map<String, List<String>> map = new HashMap<>();
        for(String word : wordList) {
            for(int i = 0; i < word.length(); i++) {
                String wordPatten = word.substring(0, i) + "*" + word.substring(i+1, word.length());
                List<String> list = map.getOrDefault(wordPatten, new ArrayList<String>());
                list.add(word);
                map.put(wordPatten, list);
            }
        }
        
        Queue<Pair<String, Integer>> Q = new LinkedList<>();
        Q.add(new Pair(beginWord, 1));
        Set<String> set = new HashSet<String>();
        
        while (!Q.isEmpty()) {
            Pair<String, Integer> node = Q.remove();
            String word = node.getKey();
            int len = node.getValue();
            for (int i = 0; i < word.length(); i++) {
                String wordPatten = word.substring(0, i) + '*' + word.substring(i+1, word.length());

                for (String nextWord : map.getOrDefault(wordPatten, new ArrayList<>())) {
                    if (nextWord.equals(endWord)) {
                        return len + 1;
                    }
                    
                    if (!set.contains(nextWord)) {
                        set.add(nextWord);
                        Q.add(new Pair(nextWord, len + 1));
                    }
                }
            }
        }
        return 0;
    }
}
```

<br/>

## 10. Create Sorted Array through Instructions

[1649. Create Sorted Array through Instructions](https://leetcode.com/problems/create-sorted-array-through-instructions/) `Hard`

**题目简述：**

给出一个数组，要求按顺序将数组插入新的有序数组，每次插入新值需要付出一定代价，代价为：数组内严格大于该值的数量，与严格小于该值的数量的较小值。求给出的数组，最终需要付出多少代价。（最终返回值对 `10^9 + 7` 取余）

**题目解答：**

这道题其实是在问，每次的新值位于数组的哪个位置，左右各多少个数字。以下使用线段树来解答，因为题目中最大值为 100000， 这里直接按照 100000 建立。

```java
class Solution {
    public int createSortedArray(int[] instructions) {
        int ret = 0;
        int[] tree = new int[4 * 100001];
        for(int num : instructions) {
            ret = (ret + Integer.min(query(tree, 1, 0, 100000, 1, num-1), query(tree, 1, 0, 100000, num+1, 100000))) % 1000000007;
            update(tree, 1, 0, 100000, num);
        }
        return ret;
    }
    
    private int query(int[] tree, int node, int nodeLeft, int nodeRight, int l, int r) {
        if (r < nodeLeft || nodeRight < l) return 0;
        if (l <= nodeLeft && nodeRight <= r) return tree[node];
        
        int mid = (nodeLeft + nodeRight) / 2;
        return query(tree, node*2, nodeLeft, mid, l, r) + query(tree, node*2+1, mid+1, nodeRight, l, r);
    }
    
    private int update(int[] tree, int node, int nodeLeft, int nodeRight, int index) {
        if (index < nodeLeft || nodeRight < index) return tree[node];
        if (nodeLeft == nodeRight) return tree[node] = tree[node] + 1;
        
        int mid = (nodeLeft + nodeRight) / 2;
        update(tree, node*2, nodeLeft, mid, index);
        update(tree, node*2+1, mid+1, nodeRight, index);
        return tree[node] = tree[node] + 1;
    }
}
```

<br/>


## 11. Merge Sorted Array

[88. Merge Sorted Array](https://leetcode.com/problems/merge-sorted-array/) `Easy`

**题目简述：**

给出两个有序数组，数组1 前共 `m+n` 长度，前 `m` 位有序，数组2共 `n` 长度，要求将数组2合并入数组1。

**题目解答：**

直接模拟合并即可，注意此处倒序处理。

```java
class Solution {
    public void merge(int[] nums1, int m, int[] nums2, int n) {
        int i = m-1, j = n-1, k = n+m-1;
        while(i >= 0 && j >= 0) {
            if (nums1[i] >= nums2[j]) {
                nums1[k--] = nums1[i--];
            } else {
                nums1[k--] = nums2[j--];
            }
        }
        while(j >= 0) {
            nums1[k--] = nums2[j--];
        }
    }
}
```

<br/>

## 12. Add Two Numbers

[2. Add Two Numbers](https://leetcode.com/problems/add-two-numbers/) `Medium`

**题目简述：**

给出两个链表，每个链表是一个非负整数的**倒序**表示。要求用链表表示这两个链表所表示的整数之和。

**题目解答：**

模拟十进制进位即可。

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
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        ListNode node1 = l1, node2 = l2, ret = null, node = null, prevNode = null;
        int a, b, val, digt = 0;
        while(node1 != null || node2 != null) {
            a = 0;
            b = 0;
            if (node1 != null) {
                a = node1.val;
                node1 = node1.next;
            }
            if (node2 != null) {
                b = node2.val;
                node2 = node2.next;
            }
            val = a + b + digt;
            digt = val / 10;
            val %= 10;
            node = new ListNode(val);
            
            if (ret == null) {
                ret = node;
            }
            if (prevNode != null) {
                prevNode.next = node;
            }
            prevNode = node;
        }
        if (digt != 0) {
            node = new ListNode(digt);
            prevNode.next = node;
        }
        return ret;
    }
}
```

<br/>

## 13. Boats to Save People

[881. Boats to Save People](https://leetcode.com/problems/boats-to-save-people/) `Medium`

**题目简述：**

现有一些人的体重，每船最多可以承载两个人，且不能超过船的最大的重量。对于给出的体重列表和船的最大载重，求最少需要多少艘船才能运走所有人。

**题目解答：**

排序后双指针扫描即可。

```java
class Solution {
    public int numRescueBoats(int[] people, int limit) {
        int i = 0, j = people.length - 1, ret = 0;
        Arrays.sort(people);
        while(i <= j) {
            if (i != j && people[i] + people[j] <= limit) {
                i++;
                j--;
                ret++;
            } else {
                j--;
                ret++;
            }
        }
        return ret;
    }
}
```

<br/>

## 14. Minimum Operations to Reduce X to Zero

[1658. Minimum Operations to Reduce X to Zero](https://leetcode.com/problems/minimum-operations-to-reduce-x-to-zero/) `Medium`

**题目简述：**

给出一个数组，和目标值 x，每次可从数组左侧或右侧移除一个值。求最少经过多少次操作后，所移除的数字总合恰好为 x，如无解返回 -1。

**题目解答：**

这道题需要反向思考，题意可以理解为求和为 `sum-x` 的连续子数组最大长度。此时就可以使用双指针扫描，查找可能的连续子数组。

```java
class Solution {
    public int minOperations(int[] nums, int x) {
        int sum = 0;
        for(int num : nums) {
            sum += num;
        }
        int target = sum - x;
        
        int i = 0, j = 0, ret = -1;
        sum = 0;
        while(j < nums.length) {
            sum += nums[j];
            if (sum > target) {
                while(i <= j) {
                    sum -= nums[i++];
                    if (sum <= target) break;
                }
            }
            if (sum == target) {
                ret = (ret == -1 || nums.length-(j-i+1) < ret) ? nums.length-(j-i+1) : ret;
            }
            j++;
        }
        return ret;
    }
}
```

<br/>

## 15. Get Maximum in Generated Array

[1646. Get Maximum in Generated Array](https://leetcode.com/problems/get-maximum-in-generated-array/) `Easy`

**题目简述：**

`nums` 有如下规则，给出数字 `n`，求 `nums[0]...nums[n]` 的最大值：
- `nums[0] = 0`
- `nums[1] = 1`
- `nums[2 * i] = nums[i]` when `2 <= 2 * i <= n`
- `nums[2 * i + 1] = nums[i] + nums[i + 1]` when `2 <= 2 * i + 1 <= n`

**题目解答：**

直接模拟计算

```java
class Solution {
    public int getMaximumGenerated(int n) {
        int[] nums = new int[101];
        nums[0] = 0;
        nums[1] = 1;
        for(int i = 2; i <= n; i++) {
            if (i % 2 == 0) nums[i] = nums[i/2];
            else nums[i] = nums[i/2] + nums[i/2+1];
        }
        
        int max = 0;
        for(int i = 0; i <= n; i++) {
            if (nums[i] > max) max = nums[i];
        }
        ;
        return max;
    }
}
```

<br/>

## 16. Kth Largest Element in an Array

[215. Kth Largest Element in an Array](https://leetcode.com/problems/kth-largest-element-in-an-array/) `Medium`

**题目简述：**

求数组内第 `k` 大的值。

**题目解答：**

数组排序后直接获取即可。

```java
class Solution {
    public int findKthLargest(int[] nums, int k) {
        Arrays.sort(nums);
        return nums[nums.length - k];
    }
}
```

<br/>

## 17. Count Sorted Vowel Strings

[1641. Count Sorted Vowel Strings](https://leetcode.com/problems/count-sorted-vowel-strings/) `Medium`

**题目简述：**

求用 `a`, `e`, `i`, `o`, `u` 构成的长度为 `n` 的字母序字符串数量。

**题目解答：**

数学方式递推，即以 `a` 结尾的字符串可以新增 `a`, `e`, `i`, `o`, `u` 任一字符，以 `e` 结束可以新增除 `a` 外的其他字符，以此类推。

```java
class Solution {
    public int countVowelStrings(int n) {
        // a e i o u
        // 1 2 3 4 5
        int[] arr = new int[]{1,1,1,1,1};
        int[] copy = new int[5];
        for(int i = 1; i < n; i++) {
            System.arraycopy(arr, 0, copy, 0, 5);
            arr[0] = copy[0] + copy[1] + copy[2] + copy[3] + copy[4];
            arr[1] = copy[1] + copy[2] + copy[3] + copy[4];;
            arr[2] = copy[2] + copy[3] + copy[4];
            arr[3] = copy[3] + copy[4];
            arr[4] = copy[4];
        }
        return arr[0] + arr[1] + arr[2] + arr[3] + arr[4];
    }
}
```

<br/>

## 18. Max Number of K-Sum Pairs

[1679. Max Number of K-Sum Pairs](https://leetcode.com/problems/max-number-of-k-sum-pairs/) `Medium`

**题目简述：**

给出数组 `arr` 和数字 `k`，每次操作可以从数组中移除恰好和为 `k` 的两个值，求最多可以操作多少次。

**题目解答：**

排序后双指针扫描。

```java
class Solution {
    public int maxOperations(int[] nums, int k) {
        Arrays.sort(nums);
        
        int cnt = 0;
        int l = 0, r = nums.length - 1;
        while(l < r) {
            if (nums[l] + nums[r] == k) {
                cnt++;
                l++;
                r--;
            } else if (nums[l] + nums[r] > k){
                r--;
            } else {
                l++;
            }
        }
        return cnt;
    }
}
```

<br/>

## 19. Longest Palindromic Substring

[5. Longest Palindromic Substring](https://leetcode.com/problems/longest-palindromic-substring/) `Medium`

**题目简述：**

返回给出字符串中的最长回文子串。

**题目解答：**

这次使用了 Manacher 算法，具体原理相对复杂，可以利用已知回文长度帮助新的位置判断，此处不再详细说明。

```java
class Solution {
    public String longestPalindrome(String s) {
        // 插入 # 以合并奇偶长度回文串情况
        StringBuffer t = new StringBuffer("");
        for(char c : s.toCharArray()) {
            t.append("#");
            t.append(c);
        }
        t.append("#");
        s = t.toString();
        
        // 保存结果
        int[] arr = new int[s.length()];
        int maxRight = -1, j = -1;
        int retLen = 0, retPlace = 0;
        for(int i = 0; i < s.length(); i++) {
            int len = 0;
            // 如果 i 的范围在已知最远回文串范围内，则可以利用已有结果
            if (maxRight > i) {
                int minLen = Integer.min(arr[2*j-i], maxRight - i);
                len = expend(s, i, minLen);
            } else {
                len = expend(s, i, 0);
            }
            arr[i] = len;
            
            // 如果本次拓展最远回文串范围，更新 j 和 maxRight 值
            if (i + len > maxRight) {
                j = i;
                maxRight = i + len;
            }
            // 保存已知最优解（范围最大）
            if (len > retLen) {
                retLen = len;
                retPlace = i;
            }
        }
        
        // 输出最优解
        StringBuffer ans = new StringBuffer();
        for(int i = retPlace - retLen; i <= retPlace + retLen; i++) {
            if (s.charAt(i) != '#') {
                ans.append(s.charAt(i));
            }
        }
        return ans.toString();
    }
    
    private int expend(String s, int index, int len) {
        int ret = len;
        for(len = len+1; index-len >= 0 && index+len < s.length(); len++) {
            if (s.charAt(index-len) != s.charAt(index+len)) {
                return len-1;
            }
            if (len > ret) {
                ret = len;
            }
        }
        return (index-len >= 0 && index+len < s.length()) ? len : len-1;
    }
}
```

<br/>

## 20. Valid Parentheses

[20. Valid Parentheses](https://leetcode.com/problems/valid-parentheses/) `Easy`

**题目简述：**

判断由 `(`, `)`, `{`, `}`, `[` 和 `]` 构成的表达式是否是有效的括号表达式，即括号是否匹配。

**题目解答：**

栈模拟即可。

```java
class Solution {
    public boolean isValid(String s) {
        Stack<Character> stack = new Stack<>();
        for(char c : s.toCharArray()) {
            switch (c) {
                case '(':
                case '[':
                case '{':
                    stack.push(c);
                    break;
                case ')':
                case ']':
                case '}':
                    if (stack.empty() || !isMatch(stack.pop(), c)) {
                        return false;
                    }
            }
        }
        return stack.empty();
    }
    
    private boolean isMatch(char a, char b) {
        return a == '(' && b == ')' || a == '[' && b == ']' || a == '{' && b == '}';
    }
}
```

<br/>

## 21. Find the Most Competitive Subsequence

[1673. Find the Most Competitive Subsequence](https://leetcode.com/problems/find-the-most-competitive-subsequence/) `Medium`

**题目简述：**

给出数组 `nums` 和整数 `k`，返回 `nums` 内长度为 `k` 的子数组（只能从原数组移除某些某些元素，但不能改变顺序）中 *最具有竞争力* 的子数组。如果 a 与 b 相比，在其第一个值不相同的位置上，a 的值比 b 的值要小，则 a 比 b 更有竞争力。

**题目解答：**

也就是说找最小的几个数。使用栈实现，遇到比栈顶元素小的值时，除非剩余元素不足，否则需要一直出栈直到栈顶值大于或等于当前值。

```java
class Solution {
    public int[] mostCompetitive(int[] nums, int k) {
        int attempts = nums.length - k;
        Stack<Integer> stack = new Stack<>();
        for(int num : nums) {
            while(!stack.empty() && stack.peek() > num && attempts > 0) {
                stack.pop();
                attempts--;
            }
            stack.push(num);
        }
        
        int[] ret = new int[k];
        while(stack.size() > k) {
            stack.pop();
        }
        for(int i = k-1; i >= 0; i--) {
            ret[i] = stack.pop();
        }
        return ret;
    }
}
```

<br/>

## 22. Determine if Two Strings Are Close

[1657. Determine if Two Strings Are Close](https://leetcode.com/problems/determine-if-two-strings-are-close/) `Medium`

**题目简述：**

给出两个字符串，判断两个字符串是否 ***接近***。如果字符串 A 能够在执行任意次数以下操作后变为字符串 B，则它们接近。

1. 任意交互两个字符的位置
2. 将全部存在的字符 a 换成字符 b，同时字符 b 也全部换为字符 a（a、b 字符必须均至少有一个存在）

**题目解答：**

也即判断两个字符串是否具有相同的字符列表，及字符出现次数是否相等。（下面的写法比较粗糙，还可以更简洁）

```java
class Solution {
    public boolean closeStrings(String word1, String word2) {
        if (word1.length() != word2.length()) return false;
        
        Map<Character, Integer> map1 = processString(word1);
        Map<Character, Integer> map2 = processString(word2);
        
        if (map1.size() != map2.size()) return false;
        
        int[] arr1 = new int[map1.size()];
        int[] arr2 = new int[map2.size()];
        int i = 0;
        for (Character c : map1.keySet()) {
            if (!map2.containsKey(c)) {
                return false;
            }
            arr1[i] = map1.get(c);
            arr2[i] = map2.get(c);
            i++;
        }
        
        Arrays.sort(arr1);
        Arrays.sort(arr2);
        for(i = 0; i < arr1.length; i++) {
            if (arr1[i] != arr2[i]) {
                return false;
            }
        }
        return true;
    }
    
    private Map<Character, Integer> processString(String s) {
        Map<Character, Integer> map = new HashMap<>();
        for(char c : s.toCharArray()) {
            map.put(c, map.getOrDefault(c, 0) + 1);
        }
        return map;
    }
}
```

<br/>

## 23. Sort the Matrix Diagonally

[1329. Sort the Matrix Diagonally](https://leetcode.com/problems/sort-the-matrix-diagonally/) `Medium`

**题目简述：**

给出一个矩阵，将其矩阵对角线上的值按升序排列。矩阵对角线指的是是从任一最上面的行或最左边的列中的一些位置开始，向右下的方向前进，直到到达末尾位置电对角线。

**题目解答：**

逐个对象线排序后写入即可

```java
class Solution {
    public int[][] diagonalSort(int[][] mat) {
        int m = mat.length, n = mat[0].length;
        for(int r = 1-m; r < n; r++) {
            // init
            int[] arr = new int[Integer.min(m, n)];
            int i = 0;
            for(i = 0; i < arr.length; i++) {
                arr[i] = Integer.MAX_VALUE;
            }
            
            // get data
            i = 0;
            for(int x = 0; x < m; x++) {
                if (x+r >= 0 && x+r < n) {
                    arr[i++] = mat[x][x+r];
                }
            }
            if (i == 0) continue;
            
            // sorting and update
            i = 0;
            Arrays.sort(arr);  
            for(int x = 0; x < m; x++) {
                if (x+r >= 0 && x+r < n) {
                    mat[x][x+r] = arr[i++];
                }
            }
            //return mat;
        }
        return mat;
    }
}
```

<br/>

## 24. Merge k Sorted Lists

[23. Merge k Sorted Lists](https://leetcode.com/problems/merge-k-sorted-lists/) `Hard`

**题目简述：**

给出 k 个有序链表，将将其合并为一个有序链表。

**题目解答：**

逐个将各个链表待处理节点中的最小节点作为新节点即可。

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
    public ListNode mergeKLists(ListNode[] lists) {
        ListNode head = new ListNode(0), currentNode = head;
        while(true) {
            ListNode nextNode = null;
            int place = 0;
            for (int i = 0; i < lists.length; i++) {
                ListNode node = lists[i];
                if (node != null && (nextNode == null || node.val < nextNode.val)) {
                    nextNode = node;
                    place = i;
                }
            }
            
            if (nextNode == null) return head.next;
            else {
                lists[place] = lists[place].next;
                currentNode.next = nextNode;
                currentNode = currentNode.next;
            }
        }
    }
}
```

<br/>

## 25. Check If All 1’s Are at Least Length K Places Away

[1437. Check If All 1's Are at Least Length K Places Away](https://leetcode.com/problems/check-if-all-1s-are-at-least-length-k-places-away) `Easy`

**题目简述：**

给出由 0，1 组成的数组和距离 k，如果所有 1 值彼此间距离均至少为 k 距离，则返回 true，否则返回 false

**题目解答：**

直接遍历所有相邻距离即可。

```java
class Solution {
    public boolean kLengthApart(int[] nums, int k) {
        if (k == 0) return true;

        int prev = -1;
        for(int i = 0; i < nums.length; i++) {
            if (nums[i] == 1) {
                if (prev != -1 && i - prev - 1 < k) {
                    return false;
                }
                prev = i;
            }
        }
        return true;
    }
}
```

<br/>

## 26. Path With Minimum Effort

[1631. Path With Minimum Effort](https://leetcode.com/problems/path-with-minimum-effort/) `Medium`

**题目简述：**

给出一个二维矩阵，其每个位置的值代表该位置的高度，现需要从左上角移动到右下角，允许在上、下、左、右方向移动，每次移动需要花费两个位置绝对高度差的体力，求最少需要花费多少体力。

**题目解答：**

使用 BFS + 二分搜索，BFS 的目的是检查使用给定的 k 值是否可以移动到右下角，再结合 k 值的二分推算得出结果。

```java
class Solution {
    private int[][] moves = new int[][]{{0, -1}, {-1, 0}, {0, 1}, {1, 0}};
    
    public int minimumEffortPath(int[][] heights) {
        int l = 0, r = 1000000;
        while(l < r) {
            int mid = l + (r-l)/2;
            if (bfs(heights, mid)) {
                r = mid;
            } else {
                l = mid+1;
            }
        }
        return l;
    }
    
    private boolean bfs(int[][] heights, int k) {
        Queue<Pair<Integer, Integer>> q = new LinkedList<>();
        q.offer(new Pair(0,0));
        
        int[][] visited = new int[heights.length][heights[0].length];
        
        while(!q.isEmpty()) {
            Pair<Integer, Integer> pair = q.poll();
            int x = pair.getKey(), y = pair.getValue();
            if (x == heights.length-1 && y == heights[0].length-1) {
                return true;
            }
            
            for(int[] move : moves) {
                int nextX = x+move[0], nextY = y+move[1];
                if (nextX < 0 || nextX >= heights.length || nextY <0 || nextY >= heights[0].length || visited[nextX][nextY] == 1) {
                    continue;
                }
                if (Math.abs(heights[x][y] - heights[nextX][nextY]) <= k) {
                    q.offer(new Pair(nextX, nextY));
                    visited[nextX][nextY] = 1;
                }
            }
        }
        return false;
    }
}
```

<br/>


## 27. Concatenation of Consecutive Binary Numbers

[1680. Concatenation of Consecutive Binary Numbers](https://leetcode.com/problems/concatenation-of-consecutive-binary-numbers/) `Medium`

**题目简述：**

给出 n，将 1..n 的二进制表示前后拼接，返回拼接得到的新二进制串的十进制值，返回值对 `10^9+7` 取模

**题目解答：**

从后向前，将每个位所表示的十进制相加即可，注意取模。

```java
class Solution {
    public int concatenatedBinary(int n) {
        long num = 1, ret = 0;
        for(int i = n; i >= 1; i--) {
            ret = ret + (i * num) % 1000000007;
            ret %= 1000000007;
            
            int len = declLenOfnum(i);
            for(int j = 0; j < len; j++) {
                num *= 2;
                num %= 1000000007;
            }
        }
        return (int) ret;
    }
    
    private int declLenOfnum(int n) {
        int num = 1;
        for(int i = 1; i < 400; i++) {
            if (n < num) return i-1;
            num*=2;
        }
        return 0;
    }
}
```

<br/>


## 28. Smallest String With A Given Numeric Value

[1663. Smallest String With A Given Numeric Value](https://leetcode.com/problems/smallest-string-with-a-given-numeric-value/) `Medium`

**题目简述：**

定义小写字符的 ***值*** 为它的位置在字母表的位置（1-indexed），字符串的 ***值*** 为其所有字符的 ***值*** 之和。 给出字符串长度 n 与值 k，返回长度为 n、值为 k 且字典序最小的字符串。

**题目解答：**

贪心法，尽量不断在字符串末尾填充值较大的字符。

```java
class Solution {
    public String getSmallestString(int n, int k) {
        char[] arr = new char[n];
        int ln = n, lk = k, i = n-1;
        while(lk > ln) {
            if (lk-26 >= ln-1) {
                arr[i--] = 'z';
                lk -= 26;
            } else {
                arr[i--] = (char)('a' + lk - ln);
                lk -= (lk - ln + 1);
            }
            ln--;
        }
        
        while(i >= 0) {
            arr[i--] = 'a';
        }
        return new String(arr);
    }
}
```

<br/>


## 29. Vertical Order Traversal of a Binary Tree

[987. Vertical Order Traversal of a Binary Tree](https://leetcode.com/problems/vertical-order-traversal-of-a-binary-tree/) `Hard`

**题目简述：**

给定二叉树，其根节点位置是 `(0, 0)`，对任一节点 `(row, col)` ，其左右节点分别位于 `(row + 1, col - 1)`，`(row + 1, col + 1)`。要求返回一个二维数组，每个数组依次表示二叉树从左到右的每一列，数组内包含位于该列的二叉树节点值，并对该数组的值升序排列。

**题目解答：**

BFS + PriorityQueue 遍历，将所有节点加入队列，并按照所在列号从小到大排序后遍历，逐个列号排序即可。

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
    public List<List<Integer>> verticalTraversal(TreeNode root) {
        PriorityQueue<QueueNode> pq = new PriorityQueue<QueueNode>(1000, new Comparator<QueueNode>() { 
            public int compare(QueueNode a, QueueNode b) 
            { 
                if (a.x != b.x) return a.x.compareTo(b.x);
                if (a.y != b.y) return b.y.compareTo(a.y);
                return a.val.compareTo(b.val);
            }
        });
        fillTreeToQueue(root, 0, 0, pq);
        
        List<List<Integer>> ret = new ArrayList<>();
        List<Integer> list = new ArrayList<>();
        int key = Integer.MIN_VALUE;
        while(!pq.isEmpty()) {
            QueueNode node = pq.poll();
            if (key == Integer.MIN_VALUE) {
                key = node.x;
            }
            if (key != node.x) {
                key = node.x;
                ret.add(list);
                list = new ArrayList<>();
            }
            list.add(node.val);
        }
        ret.add(list);
        return ret;
    }
    
    private void fillTreeToQueue(TreeNode node, int x, int y, PriorityQueue<QueueNode> q) {
        if (node == null) return ;
        q.add(new QueueNode(node.val, x, y));
        fillTreeToQueue(node.left, x-1, y-1, q);
        fillTreeToQueue(node.right, x+1, y-1, q);
    }
    
    private static class QueueNode {
        Integer val, x, y;
        public QueueNode(int val, int x, int y) {
            this.val = val;
            this.x = x;
            this.y = y;
        }
    }
}
```

<br/>

## 30. Minimize Deviation in Array

[1675. Minimize Deviation in Array](https://leetcode.com/problems/minimize-deviation-in-array/) `Hard`

**题目简述：**

给定正整数数组 nums，允许对该数组的任意值执行任意次数以下操作：偶数可以除以二，奇数可以乘以二。求任意操作后，数组的最小 ***偏差***。**偏差** 指的是数组中任意两个数的最大差。

**题目解答：**

分析题目，每个值取值有一定范围：奇数的最小值是自身，最大值是其二倍值；偶数最小值是多次除二后的直到变成的奇数，最大值是自身。

为了缩小 `偏差={最大值}-{最小值}`，先把所有数值变为最小取值，然后尝试将数组中的最小值扩大，寻找新的偏差值，直到最小值已不能扩大。

```java
class Solution {
    public int minimumDeviation(int[] nums) {
        int[] maxs = new int[nums.length];
        int min = Integer.MAX_VALUE, max = Integer.MIN_VALUE;
        for(int i = 0; i < nums.length; i++) {
            maxs[i] = nums[i] % 2 == 1 ? nums[i] * 2 : nums[i];
            
            nums[i] = minNum(nums[i]);
            if (nums[i] < min) min = nums[i];
            if (nums[i] > max) max = nums[i];
        }
        int ret = max - min;
        
        boolean isFinish = false;
        while(!isFinish) {
            int prevMin = min;
            min *= 2;
            for(int i = 0; i < nums.length; i++) {
                if (nums[i] == prevMin){
                    if (nums[i] == maxs[i]) {
                        isFinish = true;
                    } else {
                        nums[i] = nums[i] * 2;
                        if (nums[i] > max) max = nums[i];
                    }
                }
                if (nums[i] < min) min = nums[i];
            }
            if (max - min < ret) ret = max - min;
        }
        return ret;
    }
    
    public int minNum(int n) {
        if (n % 2 == 1) return n;
        else return minNum(n/2);
    }
}
```

<br/>

## 31. Next Permutation

[31. Next Permutation](https://leetcode.com/problems/next-permutation/) `Medium`

**题目简述：**

将给定数组，修改为其下一个排列，如果已经是最大排列则修改为最小排列。

**题目解答：**

思路如下：

1. 在数组末尾找到最长连续降序的部分，在其前方 v 值破坏了升序
2. 如果找到的是整个数组，则说明数组已经为最大排列，直接升序重排序
3. 否则，找到降序范围内第一个大于 v 值的值，并与 v 值交换位置，将 v 值后方范围重排序为升序

```java
class Solution {
    public void nextPermutation(int[] nums) {
        int k = -1;
        for(int i = nums.length-1; i > 0; i--) {
            if (nums[i] > nums[i-1]) {
                k = i-1;
                break;
            }
        }
        if (k == -1) {
            Arrays.sort(nums);
            return ;
        }
        
        int v = nums[k];
        for(int i = nums.length-1; i > k; i--) {
            if (nums[i] > v) {
                nums[k] = nums[i];
                nums[i] = v;
                break;
            }
        }
        
        int i = k+1, j = nums.length-1;
        while(i < j) {
            int temp = nums[i];
            nums[i] = nums[j];
            nums[j] = temp;
            i++;
            j--;
        }
    }
}
```

<br/>
