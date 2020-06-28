## Week03

### 泛型递归、树的递归

* 为什么树的面试题的解法一般都是递归？<br>
  1.节点的定义<br>
  2.重复性
  
* 递归（Recursion）<br>
  递归的本质就是通过函数体来进行的循环
  
* 代码模板
  
  ```
       public void recursion(int level, int param) {
           // terminator
           if (level > MAX_LEVEL) {
               // process result
               return;
           }
           
           // process current logic
           process.(level, param);
           
           // drill down
           recursion(level: level + 1, newParam);
           
           // restore current status
       }
  
  ```
* 思维要点<br>
  1. 不要人肉进行递归（最大误区）；
  2. 找到最近简方法，将其拆解；
  3. 数学归纳法思维。
  
### 分治、回溯

* 分治（divide& conquer）

* 代码模版

```
public void divideConquer(problem, param1, param2) {
        // recursion terminater(递归终结条件)
        if (problem is null) {
                // process result
                return;
        }
        // prepare data (将大问题分成一个个子问题)
        data = prepareData(problem);
        subProblems = splitProblem(problem, data);

        // conquer subproblems（逐一解决子问题，获得子问题的结果）
        subResult1 = divideConquer(subProblems[1],p1,p2);
        subResult2 = divideConquer(subProblems[2],p1,p2);
        subResult3 = divideConquer(subProblems[3],p1,p2);

    // process and generate the final result（将subSolution组合成Solution）
    result = processResult(subResult1, subResult2, subResult3 ...)

    //restore current status（清理当前层）
}

```

* 回溯（Backtracking）
     * 采用试错的思想，尝试分步解决一个问题 
     * 在分布过程中，如果发现现有不能满足，则取消上一步或上几部的计算


### 作业

[236 二叉树的最近公共祖先](https://leetcode-cn.com/problems/lowest-common-ancestor-of-a-binary-tree/)

```
class Solution {
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        if(root == null || root == p || root == q) return root;
        TreeNode left = lowestCommonAncestor(root.left, p, q);
        TreeNode right = lowestCommonAncestor(root.right, p, q);
        if(left == null) return right;
        if(right == null) return left;
        return root;
    }
}

```

[105 从前序与中序遍历序列构造二叉树](https://leetcode-cn.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/)

```
public class Solution {

    public TreeNode buildTree(int[] preorder, int[] inorder) {
        int preLen = preorder.length;
        int inLen = inorder.length;
        if (preLen != inLen) {
            return null;
        }
        return helper(preorder, 0, preLen - 1, inorder, 0, inLen - 1);
    }

    private TreeNode helper(int[] preorder, int preLeft, int preRight,
                               int[] inorder, int inLeft, int inRight) {
        if (preLeft > preRight || inLeft > inRight) {
            return null;
        }
        
        int pivot = preorder[preLeft];
        TreeNode root = new TreeNode(pivot);
        int pivotIndex = inLeft;

        while (inorder[pivotIndex] != pivot) {
            pivotIndex++;
        }
        root.left = buildTree(preorder, preLeft + 1, pivotIndex - inLeft + preLeft,
                inorder, inLeft, pivotIndex - 1);
        root.right = buildTree(preorder, pivotIndex - inLeft + preLeft + 1, preRight,
                inorder, pivotIndex + 1, inRight);
        return root;
    }
}

```
