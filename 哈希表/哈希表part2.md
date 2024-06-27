### [454. 四数相加 II](https://leetcode.cn/problems/4sum-ii/)

给你四个整数数组 `nums1`、`nums2`、`nums3` 和 `nums4` ，数组长度都是 `n` ，请你计算有多少个元组 `(i, j, k, l)` 能满足：

- `0 <= i, j, k, l < n`
- `nums1[i] + nums2[j] + nums3[k] + nums4[l] == 0`

**示例 1：**

```
输入：nums1 = [1,2], nums2 = [-2,-1], nums3 = [-1,2], nums4 = [0,2]
输出：2
解释：
两个元组如下：
1. (0, 0, 0, 1) -> nums1[0] + nums2[0] + nums3[0] + nums4[1] = 1 + (-2) + (-1) + 2 = 0
2. (1, 1, 0, 0) -> nums1[1] + nums2[1] + nums3[0] + nums4[0] = 2 + (-1) + (-1) + 0 = 0
```

**示例 2：**

```
输入：nums1 = [0], nums2 = [0], nums3 = [0], nums4 = [0]
输出：1
```

 

状态：完全没有思路，想用暴力法解决，担心会超时

**思路：**暴力法要四个for循环嵌套，用map解决的话可以变成两个双循环，时间复杂度指数减小。设置一个字典map，将a+b的以key存在map中，value是对应的出现次数。同理，两个for循环计算c+d,key设置为0-c-d，如果key出现在map中，则总数加map[key]。最后返回计数。

````python
class Solution:
    def fourSumCount(self, nums1: List[int], nums2: List[int], nums3: List[int], nums4: List[int]) -> int:
        sum_map = dict()
        count = 0
        for a in nums1:
            for b in nums2:
                if a+b in sum_map:
                    sum_map[a+b] += 1
                else:
                    sum_map[a+b] = 1
        for a in nums3:
            for b in nums4:
                key = 0-a-b
                if key in sum_map:
                    count += sum_map[key]

        return count
````



### [383. 赎金信](https://leetcode.cn/problems/ransom-note/)

给你两个字符串：`ransomNote` 和 `magazine` ，判断 `ransomNote` 能不能由 `magazine` 里面的字符构成。

如果可以，返回 `true` ；否则返回 `false` 。

`magazine` 中的每个字符只能在 `ransomNote` 中使用一次。

**示例 1：**

```
输入：ransomNote = "a", magazine = "b"
输出：false
```

**示例 2：**

```
输入：ransomNote = "aa", magazine = "ab"
输出：false
```

**示例 3：**

```
输入：ransomNote = "aa", magazine = "aab"
输出：true
```

状态：直接秒，设一个map存`magazine`的字母和出现的次数，for循环`ransomNote`中的子母，如果不存在于map，return False，如果存在但值为0，return False。循环结束，return true.

#### Map解法

````python
class Solution:
    def canConstruct(self, ransomNote: str, magazine: str) -> bool:
        m_map = dict()
        for s in magazine:
            if s in m_map:
                m_map[s] += 1
            else:
                m_map[s] = 1
        for m in ransomNote:
            if m in m_map and m_map[m]!=0:
                m_map[m] -= 1
            else:
                return False
        return True
````

#### 数组解法

数组消耗的空间比map小。

````python
class Solution:
    def canConstruct(self, ransomNote: str, magazine: str) -> bool:
        record = [0] * 26
        for m in magazine:
            record[ord(m)-ord('a')] += 1
        
        for r in ransomNote:
            if record[ord(r)-ord('a')] !=0 :
                record[ord(r)-ord('a')] -= 1
            else:
                return False
        return True
````



[15. 三数之和](https://leetcode.cn/problems/3sum/)

一个整数数组 `nums` ，判断是否存在三元组 `[nums[i], nums[j], nums[k]]` 满足 `i != j`、`i != k` 且 `j != k` ，同时还满足 `nums[i] + nums[j] + nums[k] == 0` 。请

你返回所有和为 `0` 且不重复的三元组。

**注意：** **答案中不可以包含重复的三元组。**

**示例 1：**

```
输入：nums = [-1,0,1,2,-1,-4]
输出：[[-1,-1,2],[-1,0,1]]
解释：
nums[0] + nums[1] + nums[2] = (-1) + 0 + 1 = 0 。
nums[1] + nums[2] + nums[4] = 0 + 1 + (-1) = 0 。
nums[0] + nums[3] + nums[4] = (-1) + 2 + (-1) = 0 。
不同的三元组是 [-1,0,1] 和 [-1,-1,2] 。
注意，输出的顺序和三元组的顺序并不重要。
```

**示例 2：**

```
输入：nums = [0,1,1]
输出：[]
解释：唯一可能的三元组和不为 0 。
```

**示例 3：**

```
输入：nums = [0,0,0]
输出：[[0,0,0]]
解释：唯一可能的三元组和为 0 。
```



**思路：**题目要求为返回三元组和为0，三元组的元素是数组元素而不是下标，因此下标不重要，先将数组排序，然后设置`left`和`right`两个指针，在for循环中，left=i+1，right=len(nums)-1，当right>left时（防止指针移动错位记录重复元组）,`sum=nums[i]+nums[left]+nums[right]`,如果sum> 0 ,则向左移动右指针，sum<0则向右移动左指针，直到左右指针相遇或者sum=0，如果sum=0，由于题目要求返回的三元组**不重复**,因此，记录下这一组sum之后，要挪动left和right，如果下一个left与right所对应的值与此时相等，则在不相遇的情况下继续挪动，直到值与下一个left/right不相等了再退出循环，挪动left和right。循环结束后得到答案数组。

#### 双指针法

````python
class Solution:
    def threeSum(self, nums: List[int]) -> List[List[int]]:
        nums.sort()
        res = []
        for i in range(len(nums)):
            if nums[i]>0:
                return res
            if i>=1 and nums[i] == nums[i-1]:
                continue

            left,right = i+1,len(nums)-1
            while right>left:
                sum = nums[i] + nums[right] + nums[left]
                if sum>0:
                    right -= 1
                elif sum<0:
                    left += 1
                else:
                    res.append([nums[i],nums[left],nums[right]])
                    while right>left and nums[left] == nums[left+1]:
                        left += 1
                    while right>left and nums[right] == nums[right-1]:
                        right -= 1
                    right -=1
                    left+=1
        return res
````



#### [18. 四数之和](https://leetcode.cn/problems/4sum/)

给你一个由 `n` 个整数组成的数组 `nums` ，和一个目标值 `target` 。请你找出并返回满足下述全部条件且**不重复**的四元组 `[nums[a], nums[b], nums[c], nums[d]]` （若两个四元组元素一一对应，则认为两个四元组重复）：

- `0 <= a, b, c, d < n`
- `a`、`b`、`c` 和 `d` **互不相同**
- `nums[a] + nums[b] + nums[c] + nums[d] == target`

你可以按 **任意顺序** 返回答案 。

**示例 1：**

```
输入：nums = [1,0,-1,0,-2,2], target = 0
输出：[[-2,-1,1,2],[-2,0,0,2],[-1,0,0,1]]
```

**示例 2：**

```
输入：nums = [2,2,2,2,2], target = 8
输出：[[2,2,2,2]]
```

**思路**：这道题在上面那道题的基础上多套了一层for循环，最外层循环i，然后内部循环`i+1`~`len(nums)-1`。可以根据数组排序的特点进行剪枝，提高算法效率

#### 双指针解法

````python
class Solution:
    def fourSum(self, nums: List[int], target: int) -> List[List[int]]:
        nums.sort()
        res = []
        if nums[0]>target and target>0: #剪枝
            return res
        for i in range(len(nums)):
            if nums[i] > target and nums[i] > 0 and target > 0:# 剪枝，后面全比target大
                break
            if i>0 and nums[i]==nums[i-1]:
                continue
            for j in range(i+1,len(nums)):
                if nums[i]+nums[j] > target and nums[j]>=0: #剪枝，nums[j]大于零，后面相加不可能得target
                    break
                if j>i+1 and nums[j]==nums[j-1]:
                    continue
                left = j+1
                right = len(nums)-1
                while right>left and right<len(nums):
                    sum = nums[i]+nums[j]+nums[left]+nums[right]
                    if sum >target:
                        right -= 1
                    elif sum<target:
                        left +=1
                    else:
                        res.append([nums[i],nums[j],nums[left],nums[right]])
                        while right>left and nums[left] == nums[left+1]:
                            left += 1
                        while right > left and nums[right] == nums[right-1]:
                            right-=1
                        right-=1
                        left+=1
        return res
````

#### 字典法

三层循环，时间复杂度拉满了

````python

class Solution(object):
    def fourSum(self, nums, target):
        # 创建一个字典来存储输入列表中每个数字的频率
        freq = {}
        for num in nums:
            freq[num] = freq.get(num, 0) + 1
        
        # 创建一个集合来存储最终答案，并遍历4个数字的所有唯一组合
        ans = set()
        for i in range(len(nums)):
            for j in range(i + 1, len(nums)):
                for k in range(j + 1, len(nums)):
                    val = target - (nums[i] + nums[j] + nums[k])
                    if val in freq:
                        # 确保没有重复
                        '''
                        确保在nums中出现的val的次数比在前三个数(nums[i], nums[j], nums[k])中出现						 的次数多。这意味着在构成四元组时，我们至少有一个额外的val在数组中。
                        '''
                        count = (nums[i] == val) + (nums[j] == val) + (nums[k] == val)
                        if freq[val] > count:
                            ans.add(tuple(sorted([nums[i], nums[j], nums[k], val])))
        
        return [list(x) for x in ans]


````

