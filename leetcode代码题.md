## [290 单词规律](https://leetcode-cn.com/problems/word-pattern/)
```
给定一种规律 pattern 和一个字符串 str ，判断 str 是否遵循相同的规律。

这里的 遵循 指完全匹配，例如， pattern 里的每个字母和字符串 str 中的每个非空单词之间存在着双向连接的对应规律。
```
### 方法一：将两个对象都映射到列表中，若列表相等则True
```python
class Solution:
    def wordPattern(self, pattern: str, s: str) -> bool:
        
        words = s.split()
        dic = {}
        res = []
        signal = 1
        for word in words:
            if word not in dic:
                dic[word] = str(signal)
                signal += 1
            res.append(dic[word])

        res2 = []
        signal2 = 1
        dic2 = {}
        for i in pattern:
            if i not in dic2:
                dic2[i] = str(signal2)
                signal2 += 1
            res2.append(dic2[i])
        # return "".join(res) == "".join(res2)
        return res == res2
        
```

### 方法二：将字符串添加列dic_map中，但无法解决 "cat dog dog fish"与"abbc"的问题，因此查看
```
class Solution:
    def wordPattern(self, pattern: str, s: str) -> bool:
        words = s.split()
        map_dic = {}
        if len(pattern) != len(words):
            return False
        for i, word in enumerate(words):
            if word not in map_dic:
                map_dic[word] = pattern[i]
            else:
                if map_dic[word] != pattern[i]:
                    return False
        if len(map_dic.values()) != len(set(map_dic.values())):
            return False
        return True
 ```
 
 ### 方法三：方法二的变式
 ```python
 class Solution:
    def wordPattern(self, pattern: str, s: str) -> bool:
        # 方法三
        words = s.split()
        map_dic = {}
        if len(pattern) != len(words):
            return False
        for i, word in enumerate(words):
            if word not in map_dic:
                if pattern[i] in map_dic.values():
                    return False
                map_dic[word] = pattern[i]
            else:
                if map_dic[word] != pattern[i]:
                    return False
        return True

```

### 方法四：双字典存储。todo


## [用最少数量的箭引爆气球](https://leetcode-cn.com/problems/minimum-number-of-arrows-to-burst-balloons)
```
在二维空间中有许多球形的气球。对于每个气球，提供的输入是水平方向上，气球直径的开始和结束坐标。由于它是水平的，所以纵坐标并不重要，因此只要知道开始和结束的横坐标就足够了。开始坐标总是小于结束坐标。

一支弓箭可以沿着 x 轴从不同点完全垂直地射出。在坐标 x 处射出一支箭，若有一个气球的直径的开始和结束坐标为 xstart，xend， 且满足  xstart ≤ x ≤ xend，则该气球会被引爆。可以射出的弓箭的数量没有限制。 弓箭一旦被射出之后，可以无限地前进。我们想找到使得所有气球全部被引爆，所需的弓箭的最小数量。

给你一个数组 points ，其中 points [i] = [xstart,xend] ，返回引爆所有气球所必须射出的最小弓箭数。
```
### 方法一：求points交集
points数组的集合合并一次，points数组长度-1。利用count+=1,由length-count记录数组最后的长度，该长度为所需最少数量的箭。
```
class Solution:
    def findMinArrowShots(self, points: List[List[int]]) -> int:
        # 排序，均为升序
        points.sort()
        length = len(points)
        # 求交集
        count = 0
        for i in range(length-1):
            al, ar = points[i][0], points[i][1]
            bl, br = points[i+1][0], points[i+1][1]
            # 由于已排序，al<=bl，只需判断ar与bl
            if bl <= ar:
                points[i+1][0] = min(ar, bl) 
                points[i+1][1] = min(ar, br)
                count += 1
        return length - count
```
为什么用count来计数，不直接使用list.pop(index)方法来修改数组呢？这是因为**涉及到下标遍历时，原地增删数组都容易造成下边错误，或超出边界或索引不合预期**。
举例，在上述代码中，若增加pop(index)，会出现什么情况呢？
```
if bl <= ar:
    points[i + 1][0] = min(ar, bl)
    points[i + 1][1] = min(ar, br)
    points.pop(i)
```
由于i是固定迭代，i值不会受points删除影响。

第一轮i=0，进入if语句，原地删除points的i=0的数组（嵌套数组），points长度减一，预期中的points最后一个下标会因该pop导致最后一个索引超出边界。

第二轮i=1，points[1]现在变成原points数组的第三个元素，因为原地删除第一个元素数组。

因此，**涉及到下标遍历时，原地增删数组都容易造成下边错误，或超出边界或索引不合预期**

基于上述发现，使用pop(index)方法原地修改列表，不遍历下标的情况可能行，因此换while循环替代for循环。




## [合并两个有序数组](https://leetcode-cn.com/problems/merge-sorted-array)
```
给你两个有序整数数组 nums1 和 nums2，请你将 nums2 合并到 nums1 中，使 nums1 成为一个有序数组。

 

说明：

初始化 nums1 和 nums2 的元素数量分别为 m 和 n 。
你可以假设 nums1 有足够的空间（空间大小大于或等于 m + n）来保存 nums2 中的元素。
 

示例：

输入：
nums1 = [1,2,3,0,0,0], m = 3
nums2 = [2,5,6],       n = 3

输出：[1,2,2,3,5,6]
```

方法一：直接将nums2的元素添加到nums1的后半部，对nums1排序
```python
class Solution:
    def merge(self, nums1: List[int], m: int, nums2: List[int], n: int) -> None:
        """
        Do not return anything, modify nums1 in-place instead.
        """
        
        nums1[:] = nums1[:m] + nums2
        nums1.sort()
        
```

方法二：双指针，从后往前指针插入

**在涉及到指针，需要画图，可以考虑极限和边界情况，本例中，[5,6,7]与[1,2] ； [1,2,3]与[7,8]**
```python
class Solution:
    def merge(self, nums1: List[int], m: int, nums2: List[int], n: int) -> None:
        """
        Do not return anything, modify nums1 in-place instead.
        """
        p = m + n - 1
        p1 = m - 1
        p2 = n - 1
        while p1 >= 0 and p2 >= 0 :
            if nums2[p2] >= nums1[p1]:
                nums1[p] = nums2[p2]
                p2 -= 1
                p -= 1
            else:
                nums1[p] = nums1[p1]
                p1 -= 1
                p -= 1
        if p2 >= 0:
            nums1[:p+1] = nums2[:p2+1]
```
注意最后两行，若p2 < 0，说明nums都已经插入完毕，`nums1[:p+1] = nums2[:p2+1]`这种写法必须要求p2 >= 0。

**这里`nums1[:p2+1] = nums2[:p2+1]`就很巧妙的包含了p2大于小于0的两种情况**

方法三：双指针，从前往后插入。注意，最后要求是nums1，因此要赋值，这里的整个元素赋值要用nums1[:]，用nums1则会报错。
```python
class Solution:
    def merge(self, nums1: List[int], m: int, nums2: List[int], n: int) -> None:
        """
        Do not return anything, modify nums1 in-place instead.
        """
        nums = []
        p1 = 0
        p2 = 0
        while p1 <= m -1 and p2 <= n - 1:
            if nums1[p1] <= nums2[p2]:
                nums.append(nums1[p1])
                p1 += 1
            else:
                nums.append(nums2[p2])
                p2 += 1
        if p1 > m - 1:
            nums.extend(nums2[p2:])
        elif p2 > n - 1:
            nums.extend(nums1[p1:m])
        nums1[:] = nums[:]
```




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
3. 方法三：记录下前一天的最低价格和最高利润，与当前天价格计算出的价格和最大利润做比较，得到**截止到当天**的最低价格和最高利润，一直迭代完所有天的价格得到所有天的最高利润。
```python
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        # 初始化
        minprice = int(1e9)
        maxprofit = 0
        for price in prices:
            maxprofit = max(maxprofit, price - minprice)
            minprice = min(minprice, price)
        return maxprofit
```

另一种变式，也是对的，但不好理解
```python
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        minprice = float('inf')
        maxprofit = 0
        for price in prices:
            minprice = min(minprice, price)
            maxprofit = max(maxprofit, price - minprice)
        return maxprofit

```
