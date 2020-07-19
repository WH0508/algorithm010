### Week06

### 分治、回溯、递归和动态规划
    
* 感触: <br> 1、人肉递归低效、很累 <br> 2、找到最近简方法, 将其拆解 <br> 3、数学归纳法思维（抵制人肉递归的诱惑）
* 本质: 寻找重复性 -> 计算机指令集

### 动态规划

* 定义

    1. wiki定义：https://en.wikipedia.org/wiki/Dynamic_programming

    2. “Simplifying a complicated problem by breaking it down into simpler sub-problems” (in a recursive manner)

    3. Divide & Conquer + Optimal substructure 分治 + 最优子结构


* 关键点
    
    * 动态规划 和 递归 或者 分治 没有根本上的区别（关键看有无最优的子结构）
        * 共性：找到重复子问题
        * 差异性：最优子结构、中途可以淘汰次优解

### 作业

[105 从前序与中序遍历序列构造二叉树](https://leetcode-cn.com/problems/minimum-path-sum/)

```
    public int minPathSum(int[][] grid) {
        int m = grid.length;
        if (m < 1) {
            return 0;
        }
        int n = grid[0].length;
        int[] dp = new int[n];
        dp[0] = grid[0][0];
        
        for (int i = 1; i < n; i++){
            dp[i] = dp[i - 1] + grid[0][i];
        }
        for (int i = 1; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (j == 0){
                    dp[j] += grid[i][j];
                } else {
                    dp[j] = Math.min(dp[j], dp[j - 1]) + grid[i][j];
                }
            }
        }
        return dp[n - 1];
    }

```

[91 解码方法](https://leetcode-cn.com/problems/decode-ways/)

```
public int numDecodings(String s) {
        if (s.length() < 1) {
            return 0;
        }
        int[] dp = new int[s.length()];
        dp[0] = s.charAt(0) == '0' ? 0 : 1;
        for (int i = 1; i < s.length(); i++) {
            int temp = i > 1 ? dp[i - 2] : 1;
            if (s.charAt(i) == '0') {
                if ((s.charAt(i - 1) == '1' || s.charAt(i - 1) == '2')) {
                    dp[i] = temp;
                } else {
                    dp[i] = 0;
                    continue;
                }
            } else {
                dp[i] = dp[i - 1];
            }
            if ((s.charAt(i - 1) == '2' && s.charAt(i) < '7' && s.charAt(i) > '0')
                    || (s.charAt(i - 1) == '1' && s.charAt(i) != '0')) {
                dp[i] += temp;
            }
        }
        return dp[s.length() - 1];
    }
    
```

[221 最大正方形](https://leetcode-cn.com/problems/maximal-square/)

```
 public int maximalSquare(char[][] matrix) {
        if (matrix.length == 0 || matrix[0].length == 0) return 0;
        int[][] f = new int[matrix.length][matrix[0].length];
        int max = 0;
        for (int i = 0; i < matrix.length; ++i) {
            if (matrix[i][0] == '1') {
                f[i][0] = 1;
                max = Math.max(max, f[i][0]);
            }
        }
        for (int j = 0; j < matrix[0].length; ++j) {
            if (matrix[0][j] == '1') {
                f[0][j] = 1;
                max = Math.max(max, f[0][j]);
            }
        }
        for (int i = 1; i < matrix.length; ++i) {
            for (int j = 1; j < matrix[0].length; ++j) {
                if (matrix[i][j] == '1') {
                    f[i][j] = 1;
                    if (matrix[i-1][j-1] == '1') {
                        int edge = f[i-1][j-1] + 1;
                        for (int k = 1; k < edge; k++) {
                            if (matrix[i-k][j] == '1' && matrix[i][j-k] == '1') f[i][j]++;
                            else break;
                        }
                    }
                    max = Math.max(max, f[i][j]);
                }
            }
        }
        return max*max;
    }
}
```

