## Week02

### 哈希表、映射、集合

*  哈希表（Hash Table），也叫散列表，是根据关键码值（key value）而直接进行访问的数据，它通过把关键码值映射到表中一个位置来访问记录，以加快查找的速度。这个映射函数叫作散列函数（Hash Function），存放记录的数组叫哈希表（或散列表）

*  哈希冲突： 当散列函数或者hash表空间过小时，容易出现hash冲突。如果出现hash冲突，则多个对象会在相同的地址上形成一个链表。

*  工程实践：
    * 电话号码薄，用户信息表，缓存，键值对存储； 
    * NSDictionary 和 NSSet 的底层实现都是由hash表来实现。
*  时间复杂度（无hash冲突的情况下）
    * Search O(1)
    * Insertion O(1)
    * Deletion O(1) 


### 树、二叉树、二叉搜索树

*    树，一个节点的有2个或者2个以上的子节点，且每个节点之间不会形成环。

*  二叉树， 有两个子节点的树
    * 二叉树的遍历（递归）
    
    ```
    public List<Integer> order(TreeNode root) {
        List<Integer> result = new ArrayList<>();
        preorder(root, result);
        inorder(root, result);
        pastorder(root, result);
        return result;
    }
    ```
    
    ```
    前序遍历 根->左->右
    public void preorder(TreeNode root, List<Integer> result) {
        if (root == null) {
            return;
        }
        result.add(root.val);
        preorder(root.left, result);
        preorder(root.right, result);
    }
       
    ```

    ```
    中序遍历 左->根->右
    public void inorder(TreeNode root, List<Integer> result) {
        if (root == null) {
            return;
        }
        inorder(root.left, result);
        result.add(root.val);
        inorder(root.right, result);
    }
    ```
    
    ```
    后序遍历 左->右->根
    public void pastorder(TreeNode root, List<Integer> result) {
        if (root == null) {
            return;
        }
        pastorder(root.left, result);
        pastorder(root.right, result);
        result.add(root.val);
    }
    ```
* 二叉搜索树, 也称二叉排序树、有序二叉树（Ordered Binary Tree）、排序二叉树（Sorted Binary Tree），是指一颗空树或者具有下列性质 的二叉树。（空树就是二叉搜索树）
    * 左子树上所有节点的值均小于它的根节点的值；
    * 右子树上所有节点的值均大于它的根节点的值；
    * 以此类推：左、右子树也分别为二叉查找树。（这就是重复性
    
    * 二叉搜索树插入和删除的过程（待总结）
    
    * 时间复杂度：
        * Search O(logn)
        * Insertion O(logn)
        * Deletion O(logn)

### 堆、二叉堆和图

* 堆（Heap），可以迅速找到一堆数中的最大或者最小值的数据结构
    * 将根节点最大的堆叫做顶推或大根堆，根节点最小的堆叫做小顶堆或者小根堆。常见的堆有二叉堆、斐波那契堆等
    * 时间复杂度：根据实现方式的不同，唯一能确定的时间复杂度就是查找最大，最小值
        * Search O(1)
* 二叉堆，完全二叉树来实现的堆，树中任意节点的值总是大于等于子节点的值（大顶堆）
    * 实现细节：
        * 二叉堆一般都是通过 数组 来实现的
        * 假设「第一个元素」 在数组中的索引未 0 的话，则父节点和子节点的位置关系如下：
            * 索引为 i 的左孩子的索引为 2 * i + 1
            * 索引为 i 的右孩子的索引为 2 * i + 2
            * 索引为 i 的父节点的索引为 floor((i - 1) / 2) 
    * 常用操作
        * Insert 插入操作，时间复杂度 O(logn)
            * 新元素一律插入到堆的尾部
            * 依次向上调整整个堆的结构（一直到根即可）HeapifyUp
        * Delete Max 删除堆顶操作 
            * 将堆尾元素替换到顶部（即堆顶被替换删除掉）
            * 依次从根部向下调整整个堆的结构（一直到堆尾即可）HeapifyDown
* 图

### 作业

[242 有效的字母异位词](https://leetcode-cn.com/problems/valid-anagram/description/)

```
    将两个字符串排序后，比较是否相同 time O(nlog(n)) space O(1)
    
    public static boolean isAnagram(String s, String t) {
        if (s.length() != t.length()) {
            return false;
        }

        char[] str1 = s.toCharArray();
        char[] str2 = t.toCharArray();

        Arrays.sort(str1);
        Arrays.sort(str2);

        return Arrays.equals(str1, str2);
    }    

```

```
    维护一个记录字母出现次数的hash表，比较最后表中的次数是否都为0. time O(n) space O(1)
    space 虽然新开辟了内存空间，但这个内存空间不会因n的变化而变化，所以是O(1)
    
    public static boolean isAnagram(String s, String t) {
        if (s.length() != t.length()) {
            return false;
        }
        if (s.length() != t.length()) {
            return false;
        }
        // 只会包含26个字母
        int[] counter = new int[26];
        for (int i = 0; i < s.length(); i++) {
            // - 'a'的目的是将char转为int
            counter[s.charAt(i) - 'a']++;
            counter[t.charAt(i) - 'a']--;
        }
        for (int count :
                counter) {
            if (count != 0) {
                return false;
            }
        }
        return true;
    }
```

[1 两数之和](https://leetcode-cn.com/problems/two-sum/description/)

```
    hash单循环 time: O(n) space O(n)

    public static int[] twoSum(int[] nums, int target) {
        Map<Integer, Integer> map = new HashMap<Integer, Integer>();
        for (int i = 0; i < nums.length; i++) {
            int diff = target - nums[i];
            if (map.containsKey(diff)) {
                return new int[]{map.get(diff), i};
            }
            map.put(nums[i], i);
        }
        throw new IllegalArgumentException("No two sum solution");
    }

```

[589 N叉树的前序遍历](https://leetcode-cn.com/problems/n-ary-tree-preorder-traversal/)

```
    public List<Integer> preorder(Node root) {
        List<Integer> result = new ArrayList<>();
        if (root == null) {
            return result;
        }
        Deque<Node> stack = new LinkedList<>();
        stack.add(root);
        while (!stack.isEmpty()) {
            Node node = stack.pollLast();
            result.add(node.val);
            Collections.reverse(node.children);
            for (Node item : node.children) {
                stack.add(item);
            }
        }
        return result;
    }

```
[49 字母异位词分组](https://leetcode-cn.com/problems/group-anagrams/)

[94 二叉树的中序遍历](https://leetcode-cn.com/problems/binary-tree-inorder-traversal/)

```
    // 迭代法
    
    public List<Integer> inorderTraversal(TreeNode root) {
        List<Integer> result = new ArrayList<>();
        if (root == null) {
            return result;
        }
        Deque<TreeNode> statck = new LinkedList<TreeNode>();
        TreeNode p = root;
        while (p != null || !statck.isEmpty()) {
            while (p != null) {
                statck.push(p);
                p = p.left;
            }
            TreeNode current = statck.pop();
            result.add(current.val);
            p = current.right;
        }
        return result;
    }

```
[144 二叉树的前序遍历](https://leetcode-cn.com/problems/binary-tree-preorder-traversal/)

```
    public List<Integer> inorderTraversal(TreeNode root) {
        List<Integer> result = new ArrayList<>();
        if (root == null) {
            return result;
        }
        Deque<TreeNode> stack = new LinkedList<TreeNode>();
        TreeNode p = root;
        while (p != null || !stack.isEmpty()) {
            while (p != null) {
                result.add(p.val);
                stack.push(p);
                p = p.left;
            }
            p = stack.pop().right;
        }
        return result;
    }

```
[429 N叉树的层序遍历](https://leetcode-cn.com/problems/n-ary-tree-level-order-traversal/)

[剑指 Offer 49 丑数](https://leetcode-cn.com/problems/chou-shu-lcof/)

[347 前 K 个高频元素](https://leetcode-cn.com/problems/top-k-frequent-elements/)

