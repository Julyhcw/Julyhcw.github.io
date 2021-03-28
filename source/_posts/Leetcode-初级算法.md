title: LeetCode
categories: LeetCode
tags: 
   - leetcode
---

这篇博客主要是记录每个月的Leetcode刷题,方便日后的复习,虽然Leetcode平台也能记录,但是有些题目明明自己IDE提交可以,在它那边老是各种问题,虽然很多是算法不完善,但也有一些毫无头绪的bug,Maybe我还不是很适应那种操作,所以这一个月的正确率低到不能看,Anyway,刷刷算法对编程很多细节的理解还是很有帮助,尽量能够一直刷下去.  

<!-- more -->
## 从排序数组中删除重复项  
### 问题描述:
>给定一个排序数组，你需要在原地删除重复出现的元素，使得每个元素只出现一次，返回移除后数组的新长度。
不要使用额外的数组空间，你必须在原地修改输入数组并在使用 O(1) 额外空间的条件下完成  

### 说明
>给定数组 nums = [1,1,2],   
函数应该返回新的长度 2, 并且原数组 nums 的前两个元素被修改为 1, 2。   
你不需要考虑数组中超出新长度后面的元素。



```python
def removeDuplicates(nums):
    j = 0
    for i in range(len(nums)):
        if nums[j] != nums[i]:
            j += 1
            nums[j] = nums[i]
    del nums[j+1:]
    return nums
        
```


```python
nums = removeDuplicates([1,1,2,3,3,4,5,5,5,9])
print(nums)
```

    [1, 2, 3, 4, 5, 9]

## 买卖股票  
### 问题描述  
>给定一个数组，它的第 i 个元素是一支给定股票第 i 天的价格。  
设计一个算法来计算你所能获取的最大利润。你可以尽可能地完成更多的交易（多次买卖一支股票）。  
注意：你不能同时参与多笔交易（你必须在再次购买前出售掉之前的股票）。  
  
### 实例  
>输入: [7,1,5,3,6,4]  
输出: 7  
解释: 在第 2 天（股票价格 = 1）的时候买入，在第 3 天（股票价格 = 5）的时候卖出, 这笔交易所能获得利润 = 5-1 = 4 。
     随后，在第 4 天（股票价格 = 3）的时候买入，在第 5 天（股票价格 = 6）的时候卖出, 这笔交易所能获得利润 = 6-3 = 3 。




```python
def maxprofit(nums):
    j = 0
    sum = 0
        
    if len(nums) <= 1:
        return sum
    
    
    while True:
        max = min = nums[j]
        for i in range(j+1,len(nums)):
            if nums[i] < max:
                break
            max = nums[i]
        sum += (max - min)
        j = i
        if j+1 == len(nums):
            break
    return sum
```


```python
p = maxprofit([2,3,5,6])
print(p)
```

    4



```python
def Maxprofit(nums):
    sum = 0        
    for i in range(len(nums) - 1):
        if nums[i] < nums[i+1]:
            sum += (nums[i+1] - nums[i])
    return sum
```


```python
sum = Maxprofit([1,2,3,5,6])
print(sum)
```

    5

## 旋转数组  
### 问题描述  
>给定一个数组，将数组中的元素向右移动 k 个位置，其中 k 是非负数。  
### 实例  
>输入: [1,2,3,4,5,6,7] 和 k = 3  
输出: [5,6,7,1,2,3,4]  
解释:  
向右旋转 1 步: [7,1,2,3,4,5,6]  
向右旋转 2 步: [6,7,1,2,3,4,5]  
向右旋转 3 步: [5,6,7,1,2,3,4]  


```python
class Solution:
    def rotate(self, nums, k):
        new = []
        for i in range(-k,-1):
            new.append(nums[i])
        
        new.append(nums[-1])
        for j in range(len(nums)-k):
            new.append(nums[j])
        return new
```


```python
if __name__ == '__main__':
    new = Solution().rotate([1,2,3,4,1,2,7,8],1)
    print(new)
```

    [8, 1, 2, 3, 4, 1, 2, 7]



```python
class Solution:
    def rotate(self, nums, k):
        """
        :type nums: List[int]
        :type k: int
        :rtype: void Do not return anything, modify nums in-place instead.
        """
        n = len(nums)
        a1 = nums[:(n-k)]
        a2 = nums[(n-k):]
        return a2+a1
if __name__ == '__main__':
    new = Solution().rotate([1,2,3,4,5,6,7],3)
    print(new)
```

    [5, 6, 7, 1, 2, 3, 4]

## 存在重复  
### 问题描述  
>给定一个整数数组，判断是否存在重复元素。  
如果任何值在数组中出现至少两次，函数返回 true。如果数组中每个元素都不相同，则返回 false。  

### 实例  
>输入: [1,2,3,1]  
输出: true  
输入: [1,2,3,4]  
输出: false 
输入: [1,1,1,3,3,4,3,2,4,2]  
输出: true  



```python
class Solution:
    def containsDuplicate(self, nums):
        """
        :type nums: List[int]
        :rtype: bool
        """
        new = set(nums)
        a = len(new)
        b = len(nums)
        if a == b:
            return False
        else:
            return True
            
            
```


```python
a = Solution().containsDuplicate([1,1,3,1])
print(a)
```

    True

## 只出现一次的数字  
### 问题描述  
>给定一个非空整数数组，除了某个元素只出现一次以外，其余每个元素均出现两次。找出那个只出现了一次的元素。  
说明：  
你的算法应该具有线性时间复杂度。 你可以不使用额外空间来实现吗？  

### 实例  
>输入: [2,2,1]  
输出: 1  
输入: [4,1,2,1,2]  
输出: 4  





```python
class Solution:
    def singleNumber(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        a = 0
        for i in nums:
            a ^= i  
        return a
        
```


```python
a = Solution().singleNumber([2,4,6,6,4,2,3])

print(a)
```

    3


## 给定两个数组，写一个方法来计算它们的交集。
>例如:  
给定 nums1 = [1, 2, 2, 1], nums2 = [2, 2], 返回 [2, 2].  

### 注意  
>   输出结果中每个元素出现的次数，应与元素在两个数组中出现的次数一致。  
   我们可以不考虑输出结果的顺序。  
### 跟进:  
>如果给定的数组已经排好序呢？你将如何优化你的算法？  
如果 nums1 的大小比 nums2 小很多，哪种方法更优？  
如果nums2的元素存储在磁盘上，内存是有限的，你不能一次加载所有的元素到内存中，你该怎么办？  


```python
class Solution:
    def intersect(self, nums1, nums2):
        """
        :type nums1: List[int]
        :type nums2: List[int]
        :rtype: List[int]
        """
        li = []
        for i in nums1:
            for j in nums2:
                if i == j:
                    li.append(j)
                else:
                    continue
        new = set(li)
        li = list(new)
        return li
```


```python
nums1 = [1]
nums2 = [1,1]
a = Solution().intersect(nums1, nums2)
print(a)
```

    [1]

## 加一  
### 问题描述  
>给定一个非负整数组成的非空数组，在该数的基础上加一，返回一个新的数组。  
最高位数字存放在数组的首位， 数组中每个元素只存储一个数字。  
你可以假设除了整数 0 之外，这个整数不会以零开头。  

### 示例  
>输入: [1,2,3]  
输出: [1,2,4]  
解释: 输入数组表示数字 123。  


```python
class Solution:
    def plusOne(self, digits):
        """
        :type digits: List[int]
        :rtype: List[int]
        """
        for i in range(len(digits)-1,-1,-1):
            if digits[i]<9:
                digits[i]=digits[i]+1
                return digits
            else:
                digits[i]=0
        digits.insert(0,1)
        return digits
```


```python
digits = [1,9,8]
a = Solution().plusOne(digits)
print(a)
```

    [1, 9, 9]

## 移动零
### 问题描述    
>给定一个数组 nums，编写一个函数将所有 0 移动到数组的末尾，同时保持非零元素的相对顺序。  
1.必须在原数组上操作，不能拷贝额外的数组。  
2.尽量减少操作次数。  

### 实例  
>输入: [0,1,0,3,12]  
输出: [1,3,12,0,0]


```python
class Solution:
    def moveZeroes(self, nums):
        """
        :type nums: List[int]
        :rtype: void Do not return anything, modify nums in-place instead.
        """
        for i in nums:
            if i == 0:
                nums.remove(i)
                nums.append(0)
        return nums
```


```python
nums = [0,1,0,3,6]
a = Solution().moveZeroes(nums)
print(a)
```

    [1, 3, 6, 0, 0]



```python
class Solution:
    def moveZeroes(self, nums):
        """
        :type nums: List[int]
        :rtype: void Do not return anything, modify nums in-place instead.
        """
        pos = 0
        for i in range(len(nums)):
            if nums[i]: #过滤掉0
                nums[i], nums[pos] = nums[pos], nums[i]
                pos += 1
```


```python
nums = [0,1,0,3,6]
Solution().moveZeroes(nums)
print(nums)
```

    [1, 3, 6, 0, 0]

## 两数之和
### 问题描述    
>给定一个整数数组和一个目标值，找出数组中和为目标值的两个数。  
你可以假设每个输入只对应一种答案，且同样的元素不能被重复利用。

### 实例  
>给定 nums = [2, 7, 11, 15], target = 9  
因为 nums[0] + nums[1] = 2 + 7 = 9  
所以返回 [0, 1]  


```python
class Solution(object):
    def twoSum(self, nums, target):
        """
        :type nums: List[int]
        :type target: int
        :rtype: List[int]
        """      
        for i in range(len(nums)-1):
            for j in range(i+1,len(nums)):
                if nums[i] + nums[j] == target:
                    return [i,j]
                
```


```python
nums = [3,2,4,5]
target = 6
num = Solution().twoSum(nums,target)
print(num)
```

    [1, 2]



```python
class Solution(object):
    def twoSum(self, nums, target):
        """
        :type nums: List[int]
        :type target: int
        :rtype: List[int]
        """ 
        dic={}  
        for i in range(len(nums)):  
            if nums[i] in dic:  
                return [dic[nums[i]],i]  
            else:  
                dic[target-nums[i]]=i
```


```python
nums = [3,4,2]
target = 6
num = Solution().twoSum(nums,target)
print(num)
```

    [1, 2]

## 旋转图像  
### 问题描述  
>给定一个 n × n 的二维矩阵表示一个图像。  
将图像顺时针旋转 90 度。  

### 说明  
>你必须在原地旋转图像，这意味着你需要直接修改输入的二维矩阵。请不要使用另一个矩阵来旋转图像。  

### 实例  
>给定 matrix =   
[
  [1,2,3],  
  [4,5,6],  
  [7,8,9]],    
原地旋转输入矩阵，使其变为:  
[
  [7,4,1],  
  [8,5,2],  
  [9,6,3]]    




```python
class Solution:
    def rotate(self, matrix):
        """
        :type matrix: List[List[int]]
        :rtype: void Do not return anything, modify matrix in-place instead.
        """
        m = len(matrix)
        n = len(matrix[1])
        matrix = zip(*matrix)
        matrix = list(map(list, matrix))
        for i in range(m):
            for j in range(int(n/2)):
                matrix[i][j], matrix[i][n-j-1] = matrix[i][n-j-1], matrix[i][j]
            return matrix           
```


```python
matrix =[[ 5, 1, 9],[ 2, 4, 8],[13, 3, 6]]
matrix = Solution().rotate(matrix)
print(matrix)
```

    [[13, 2, 5], [1, 4, 3], [9, 8, 6]]

## 有效的数独
### 问题描述  
>判断一个 9x9 的数独是否有效。只需要根据以下规则，验证已经填入的数字是否有效即可。  
1. 数字 1-9 在每一行只能出现一次。  
2. 数字 1-9 在每一列只能出现一次。  
3. 数字 1-9 在每一个以粗实线分隔的 3x3 宫内只能出现一次。  

### 实例
>输入:  
[
  ["5","3",".",".","7",".",".",".","."],  
  ["6",".",".","1","9","5",".",".","."],  
  [".","9","8",".",".",".",".","6","."],  
  ["8",".",".",".","6",".",".",".","3"],  
  ["4",".",".","8",".","3",".",".","1"],  
  ["7",".",".",".","2",".",".",".","6"],  
  [".","6",".",".",".",".","2","8","."],  
  [".",".",".","4","1","9",".",".","5"],  
  [".",".",".",".","8",".",".","7","9"]  
]
输出: true  


```python
class Solution(object):
    def isValidSudoku(self, board):
        """
        :type board: List[List[str]]
        :rtype: bool
        """
        seen = set()
        for i in range(9):
            for j in range(9):
                c = board[i][j]
                if c == '.':
                    continue
                if (i, c) in seen or (c, j) in seen or (i/3, j/3, c) in seen:
                    return True
                seen.add((i, c))
                seen.add((c, j))
                seen.add((i/3, j/3, c))
        return False
```


```python
a = [["8","3",".",".","7",".",".",".","."],["6",".",".","1","9","5",".",".","."],[".","9","8",".",".",".",".","6","."],["8",".",".",".","6",".",".",".","3"],["4",".",".","8",".","3",".",".","1"],["7",".",".",".","2",".",".",".","6"],[".","6",".",".",".",".","2","8","."],[".",".",".","4","1","9",".",".","5"],[".",".",".",".","8",".",".","7","9"]]
b = Solution().isValidSudoku(a)
print(b)
```

    True


>这个算法是参考别人,感觉很简单,但是好像是错的,先记下来,还没看懂.


```python
class Solution(object):
    def isValidSudoku(self, board):
        """
        :type board: List[List[str]]
        :rtype: bool
        """ 
        # write your code here
        if board is None or len(board) != 9 or len(board[0]) != 9:
            return None
        s = set()
        # 判断行和列
        for i in range(9):
            # 判断每行
            for j in range(9):
                if board[i][j] != '.' and board[i][j] in s:
                    return False
                else:
                    s.add(board[i][j])
            s.clear()

            # 判断每列
            for k in range(9):
                if board[k][i] != '.' and board[k][i] in s:
                    return False
                else:
                    s.add(board[k][i])
            s.clear()

        # 判断每块
        for i in range(0, 9, 3):
            for j in range(0, 9, 3):
                for k in range(i, i + 3):
                    for p in range(j, j + 3):
                        if board[k][p] != '.' and board[k][p] in s:
                            return False
                        else:
                            s.add(board[k][p])
                s.clear()

        return True
```


```python
a = [["8","3",".",".","7",".",".",".","."],["6",".",".","1","9","5",".",".","."],[".","9","8",".",".",".",".","6","."],["8",".",".",".","6",".",".",".","3"],["4",".",".","8",".","3",".",".","1"],["7",".",".",".","2",".",".",".","6"],[".","6",".",".",".",".","2","8","."],[".",".",".","4","1","9",".",".","5"],[".",".",".",".","8",".",".","7","9"]]
b = Solution().isValidSudoku(a)
print(b)

```

    False


>上面刷的都是最基本的算法,但还是很多都是参考别人的代码或者思路,多刷几轮,希望能有点提升.
