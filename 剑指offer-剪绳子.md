### 剪绳子
[https://leetcode-cn.com/problems/jian-sheng-zi-lcof/](https://leetcode-cn.com/problems/jian-sheng-zi-lcof/)
给你一根长度为 n 的绳子，请把绳子剪成整数长度的 m 段（m、n都是整数，n>1并且m>1），每段绳子的长度记为 k[0],k[1]...k[m-1] 。请问 k[0]*k[1]*...*k[m-1] 可能的最大乘积是多少？例如，当绳子的长度是8时，我们把它剪成长度分别为2、3、3的三段，此时得到的最大乘积是18。  

示例 1：
~~~
输入: 2
输出: 1
解释: 2 = 1 + 1, 1 × 1 = 1
~~~

示例二：
~~~
输入: 10
输出: 36
解释: 10 = 3 + 3 + 4, 3 × 3 × 4 = 36
~~~

#### 解题思路
两种解决办法，一种是贪心（数学思想），最好能划分成多个长度等于 3 的块，划分完成后就没问题；一种是动态规划，

#### 贪心思想
数据推导，最终能发现，如果能划分成多个 长度为 3 的绳子，那么乘积会是最大的，所以我们就需要尽可能划分出 3， 当然有例外，如果最终还剩余的长度为 1， 则最好是留下一个3， 转而乘4更大；如果最终剩余的是2，那么直接再相乘就好。  
~~~go
func cuttingRope(n int) int {
    if n <= 3 {
        return n - 1
    }
    res := 1
    for n >= 3 {
        res *= 3
        n -= 3
    }
    if n == 2{
        return n *res
    }
    if n ==1 {
        return res / 3 * 4
    }
    return res
   
}
~~~

#### 动态规划
使用动态规划，主要思路如下，使用 dp[i] 保存 长度为 i 的绳子切分后所能组成的最大乘积。
- 遍历 L = 1.... R-1, 则绳子可以分为 L 和 L-R 两部分，假设 L-R 也可以分
- 那么，dp[i]  = max(L * (R-L), L * dp[R-L]), 取其中最大的 dp[i]。

代码油然而生，动态规划出长度为 i 的最大的乘积，再网上加就好用了。
~~~go
func cuttingRope(n int) int {
    if n < 2{
        return 0
    }
    
    dp := make([]int, n+1,n+1) 
    dp[0] = 0
    dp[1] = 0
    dp[2] = 1
    for r := 3; r <= n; r++ {
        for l := 2; l < r; l ++{
            dp[r] = max(dp[r],max(l * (r-l), l * (dp[r-l])))
            // max(l * (r-l), l *dp[r-l]) 是由于剪去长度为 l 的绳子后，可以减，也可以不减，判断剪还是不减，l* (r-l) 表示不减更大， l*dp[r-l] 为再继续剪更大，因为之前已经求出了 dp[r-l] 的最大值，所以可以减少重复计算
            //  max(dp[r]), max(1*(r-l), l*dp[r-l]) 则是再 l 不同的时候，求出最大的dp[r]
        }
    }
    return dp[n]
}
~~~