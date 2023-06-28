---
title: '剑指 Offer 47. 礼物的最大价值'
date: 2023-03-09 18:10:38
tags: [leetcode]
published: true
hideInList: false
feature: 
isTop: false
---
在一个 m*n 的棋盘的每一格都放有一个礼物，每个礼物都有一定的价值（价值大于 0）。你可以从棋盘的左上角开始拿格子里的礼物，并每次向右或者向下移动一格、直到到达棋盘的右下角。给定一个棋盘及其上面的礼物的价值，请计算你最多能拿到多少价值的礼物？

<!-- more -->

来源：力扣（LeetCode）
链接：https://leetcode.cn/problems/li-wu-de-zui-da-jie-zhi-lcof
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

*  示例 1:

```text
输入:
[
[1,3,1],
[1,5,1],
[4,2,1]
]
输出: 12
解释: 路径 1→3→5→2→1 可以拿到最多价值的礼物

```

* 提示
  * `0 < grid.length <= 200`
  * `0 < grid[0].length <= 200`



这个是动态规划问题。 我们可以把问题转化为 获取下或者右的一个单位的的最大值。

```java


/**
 * 剑指 Offer 47. 礼物的最大价值 在一个 m*n 的棋盘的每一格都放有一个礼物，每个礼物都有一定的价值（价值大于
 * 0）。你可以从棋盘的左上角开始拿格子里的礼物，并每次向右或者向下移动一格、直到到达棋盘的右下角。给定一个棋盘及其上面的礼物的价值，请计算你最多能拿到多少价值的礼物？
 * <p>
 * 来源：力扣（LeetCode） 链接：https://leetcode.cn/problems/li-wu-de-zui-da-jie-zhi-lcof
 * 著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
 *
 * @author Li JiaWei
 * @version TODO
 * @date 2023/3/8
 * @since on
 */
public class GiftMaximum_2704 {

  /**
   * 在一个 m*n 的棋盘的每一格都放有一个礼物，每个礼物都有一定的价值（价值大于
   * 0）。你可以从棋盘的左上角开始拿格子里的礼物，并每次向右或者向下移动一格、直到到达棋盘的右下角。给定一个棋盘及其上面的礼物的价值，请计算你最多能拿到多少价值的礼物？
   * <p>
   * 来源：力扣（LeetCode） 链接：https://leetcode.cn/problems/li-wu-de-zui-da-jie-zhi-lcof
   * 著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。 输入: [ [1,3,1], [1,5,1], [4,2,1] ] 输出: 12 解释: 路径 1→3→5→2→1
   * 可以拿到最多价值的礼物 提示：
   * <p>
   * 0 < grid.length <= 200 0 < grid[0].length <= 200
   *
   * @param grid
   * @return
   */
  public static int maxValue(int[][] grid) {
    //  这是一个简单的 动态规划问题，
    // 实际上我要做的是 上或者下 去一个最大值。
    // f(x,y) = max( x,y+1) (x+1,y)+ (x-1,y-1) 前一个位置的值。
    int l = grid.length, r = grid[0].length;
    int[][] w = new int[l][r];
    for (int i = 0; i < l; i++) {
      for (int j = 0; j < r; j++) {
        if (i > 0) {
          w[i][j] = Math.max(w[i][j], w[i - 1][j]);
        }
        if (j > 0) {
          w[i][j] = Math.max(w[i][j], w[i][j - 1]);
        }
        w[i][j] += grid[i][j];
      }
    }
    System.out.println(w[l - 1][r - 1]);
    // 因为只需要前后两行居矩阵 ，可以这样
    int[][] w1 = new int[2][r];
    for (int i = 0; i < l; i++) {
      for (int j = 0; j < r; j++) {
        int pos = i % 2;
        if (i > 0) {
          w1[pos][j] = Math.max(w1[pos][j], w1[1 - pos][j]);
        }
        if (j > 0) {
          w1[pos][j] = Math.max(w1[pos][j], w1[pos][j - 1]);
        }
        w1[pos][j] += grid[i][j];
      }
    }
    System.out.println(w1[(l - 1) % 2][r - 1]);

    int[] dp = new int[r + 1];
    for (int i = 1; i <= l; i++) {
      for (int j = 1; j <= r; j++) {
        dp[i] = Math.max(dp[i], dp[j - 1]) + grid[i - 1][j - 1];
//        System.out.println( dp[i]);
      }
    }

    return dp[r];
  }

  public static void main(String[] args) {
    int[][] gif = {
        {1, 3, 1},
        {1, 5, 1},
        {4, 2, 1}
    };
    var max = maxValue(gif);
    System.out.println(max);
  }
}

```