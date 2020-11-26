## [买卖股票最佳时机](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock)
```
给定一个数组，它的第 i 个元素是一支给定股票第 i 天的价格。

如果你最多只允许完成一笔交易（即买入和卖出一支股票一次），设计一个算法来计算你所能获取的最大利润。

注意：你不能在买入股票前卖出股票。
 
示例 1:

输入: [7,1,5,3,6,4]
输出: 5
解释: 在第 2 天（股票价格 = 1）的时候买入，在第 5 天（股票价格 = 6）的时候卖出，最大利润 = 6-1 = 5 。
     注意利润不能是 7-1 = 6, 因为卖出价格需要大于买入价格；同时，你不能在买入前卖出股票。
示例 2:

输入: [7,6,4,3,1]
输出: 0
解释: 在这种情况下, 没有交易完成, 所以最大利润为 0。
```

1. 方法一：暴力解，遍历。注意两点：一是**数组索引不同于数组元素值**，二是添加利润，是大数减小数
```python
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        # 等价于取不降子串，取有最大差的子串
        '''思路出了问题
        if len(prices) < 2:
            return 0
        dp = [0] * (len(prices) - 1)
        dp[0] = prices[1] - prices[0]
        for i in range(2, len(prices)):
            dp[i] = max(prices[i] - )
        '''
        leng = len(prices)
        if leng < 2:
            return 0
        res = [0]
        for i in range(leng - 1):
            for j in range(i + 1, leng):
                # if i - j < 0:
                    # res.append(j - i)
                if prices[i] - prices[j] < 0:
                    res.append(prices[j] - prices[i])
        return max(res)
```
2. 方法二：动态规划，与最大子序和类似。实际写的时候，利用当前元素减去前面元素的最小值，应该有可以优化的点。
```python
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        # 等价于取不降子串，取有最大差的子串
        if len(prices) < 2:
            return 0
        res = prices[1] - prices[0] if (prices[1] - prices[0]) > 0 else 0
        for i in range(1, len(prices)):
            if prices[i] > min(prices[:i]):
                res = max(res, prices[i] - min(prices[:i]))
        return res
```
