### [24.两两交换链表中的节点](https://leetcode.cn/problems/swap-nodes-in-pairs/description/)

给你一个链表，两两交换其中相邻的节点，并返回交换后链表的头节点。你必须在不修改节点内部的值的情况下完成本题（即，只能进行节点交换）。

**示例 1：**

![img](https://assets.leetcode.com/uploads/2020/10/03/swap_ex1.jpg)

```
输入：head = [1,2,3,4]
输出：[2,1,4,3]
```

**状态**：自己先画图推导了一遍，在循环中设置了两个变量用于存放两个交换节点的前后相邻节点，默认最少有两个节点才能进入循环，`pre=head`，`cur=head.next`，设置temp为`cur.next`，存放好后一个节点的位置，然后将pre和cur节点进行交换操作，如果前节点`pretemp`存在，则让它指向现在的交换到前面的cur节点，不存在则说明是第一次交换，让head指向现在的cur节点。上述操作完成后，移动cur和pre节点，刷新pretemp节点的位置，如果`temp`为None，说明已经交换到了最后两位，可以结束循环，反之则令`pretrmp=pre`，`cur=temp.next` 以及`pre = temp`继续循环交换。

#### 自己推导的解法

````python
class Solution:
    def swapPairs(self, head: Optional[ListNode]) -> Optional[ListNode]:
        if head==None or head.next == None:
            return head
        flag = 0
        pre,cur = head,head.next
        pretemp = None
        while cur:
            temp = cur.next
            cur.next=pre
            pre.next = temp
            if pretemp:
                pretemp.next = cur
            else:
                head = cur
            if temp:
                cur = temp.next
                pretemp = pre
                pre = temp
            else:
                break
        return head
````



#### 递归解法

解法参考：[代码随想录](https://programmercarl.com/0024.%E4%B8%A4%E4%B8%A4%E4%BA%A4%E6%8D%A2%E9%93%BE%E8%A1%A8%E4%B8%AD%E7%9A%84%E8%8A%82%E7%82%B9.html#%E5%85%B6%E4%BB%96%E8%AF%AD%E8%A8%80%E7%89%88%E6%9C%AC)

````python
class Solution:
    def swapPairs(self, head: Optional[ListNode]) -> Optional[ListNode]:
        if head==None or head.next == None:
            return head
        pre,cur = head,head.next
        temp = cur.next

        cur.next = pre
        pre.next = self.swapPairs(temp)

        return cur   
````



### [19. 删除链表的倒数第 N 个结点](https://leetcode.cn/problems/remove-nth-node-from-end-of-list/)
给你一个链表，删除链表的倒数第 `n` 个结点，并且返回链表的头结点。

**示例 1：**

![img](https://assets.leetcode.com/uploads/2020/10/03/remove_ex1.jpg)

```
输入：head = [1,2,3,4,5], n = 2
输出：[1,2,3,5]
```

**状态：**第一反应是另写一个反转链表的函数，先反转，根据n删除节点之后再转回来，暴力解决。

**只扫描一遍的思路：**设置**快慢指针**slow,fast，让fast先走n+1步，然后slow和fast同时向后移动，当fast=null时，说明fast的前一个节点是倒数第一个节点，根据fast和slow的距离可以得知，此时slow在倒数第n个节点的前一个节点处，因此可以直接执行删除操作。

````python
class Solution:
    def removeNthFromEnd(self, head: Optional[ListNode], n: int) -> Optional[ListNode]:
        dhead = ListNode(0,head)
        slow,fast = dhead,dhead
        for i in range(n+1):
            fast = fast.next
        while fast:
                slow = slow.next
                fast = fast.next
        slow.next = slow.next.next
        return dhead.next
````

解法参考：[代码随想录](https://programmercarl.com/0019.%E5%88%A0%E9%99%A4%E9%93%BE%E8%A1%A8%E7%9A%84%E5%80%92%E6%95%B0%E7%AC%ACN%E4%B8%AA%E8%8A%82%E7%82%B9.html#%E5%85%B6%E4%BB%96%E8%AF%AD%E8%A8%80%E7%89%88%E6%9C%AC)



### [2.7.链表相交](https://leetcode.cn/problems/intersection-of-two-linked-lists-lcci/description/)

给你两个单链表的头节点 `headA` 和 `headB` ，请你找出并返回两个单链表相交的起始节点。如果两个链表没有交点，返回 `null` 。

图示两个链表在节点 `c1` 开始相交**：**

[![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/14/160_statement.png)](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/14/160_statement.png)

题目数据 **保证** 整个链式结构中不存在环。

**注意**，函数返回结果后，链表必须 **保持其原始结构** 。

 

**示例 1：**

[![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/14/160_example_1.png)](https://assets.leetcode.com/uploads/2018/12/13/160_example_1.png)

```
输入：intersectVal = 8, listA = [4,1,8,4,5], listB = [5,0,1,8,4,5], skipA = 2, skipB = 3
输出：Intersected at '8'
解释：相交节点的值为 8 （注意，如果两个链表相交则不能为 0）。
从各自的表头开始算起，链表 A 为 [4,1,8,4,5]，链表 B 为 [5,0,1,8,4,5]。
在 A 中，相交节点前有 2 个节点；在 B 中，相交节点前有 3 个节点。
```

状态：我一开始是直接暴力计算的，先计算出A和B的长度，然后套用两个for循环进行节点对比，设置flag标记是否相交。时间复杂度是O(m*n)

**思路：**先计算出A，B的长度，然后算出二者长度差值len，较长的链表指针移动len次，短链表指针指向开头。然后A,B两个指针同时出发,比较节点是否相等，有交点则直接返回，若循环解释则表示无交点，时间复杂度为O(n+m)

解法参考：[代码随想录](https://programmercarl.com/%E9%9D%A2%E8%AF%95%E9%A2%9802.07.%E9%93%BE%E8%A1%A8%E7%9B%B8%E4%BA%A4.html#%E6%80%9D%E8%B7%AF)

#### 常规解法

````python
class Solution:
    def getIntersectionNode(self, headA: ListNode, headB: ListNode) -> ListNode:
        sizeA = 0
        sizeB = 0
        currenta = headA
        currentb = headB
        flag = 0
        while currenta:
            sizeA+=1
            currenta = currenta.next
        while currentb:
            sizeB+=1
            currentb=currentb.next
        currenta = headA
        currentb = headB
        
        if sizeA>sizeB:
            len = sizeA-sizeB
            flag=1
        else:
            len = sizeB-sizeA
            flag=2
        for i in range(len):
            if flag==1:
                currenta = currenta.next
            else:
                currentb = currentb.next
        while currenta:
            if currenta == currentb:
                return currenta
            else:
                currenta = currenta.next
                currentb = currentb.next
        return None
````



#### 等比例解法

与上面计算链表长度差值的核心思想差不多，这个方法将差值体现在pointerA和pointerB各自走过的A/B链表的长度差。假设链表 A 的长度为 `a + c`，链表 B 的长度为 `b + c`，其中 `c` 是公共部分的长度。如果没有交点，`c` 为 0。由于两个指针遍历的总节点数是相等的（`a + c + b = b + c + a`），因此它们会在第二次遍历时在交点处相遇。如果没有交点，最终它们会同时到达链表的末尾（即 `None`）。

````python
class Solution:
    def getIntersectionNode(self, headA: ListNode, headB: ListNode) -> ListNode:
        # 处理边缘情况
        if not headA or not headB:
            return None
        
        # 在每个链表的头部初始化两个指针
        pointerA = headA
        pointerB = headB
        
        # 遍历两个链表直到指针相交
        while pointerA != pointerB:
            # 将指针向前移动一个节点
            pointerA = pointerA.next if pointerA else headB
            pointerB = pointerB.next if pointerB else headA
        
        # 如果相交，指针将位于交点节点，如果没有交点，值为None
        return pointerA
````



### [142.环形链表 II](https://leetcode.cn/problems/linked-list-cycle-ii/description/)

给定一个链表的头节点  `head` ，返回链表开始入环的第一个节点。 *如果链表无环，则返回 `null`。*

如果链表中有某个节点，可以通过连续跟踪 `next` 指针再次到达，则链表中存在环。 为了表示给定链表中的环，评测系统内部使用整数 `pos` 来表示链表尾连接到链表中的位置（**索引从 0 开始**）。如果 `pos` 是 `-1`，则在该链表中没有环。**注意：`pos` 不作为参数进行传递**，仅仅是为了标识链表的实际情况。

**不允许修改** 链表。



 

**示例 1：**

![img](https://assets.leetcode.com/uploads/2018/12/07/circularlinkedlist.png)

```
输入：head = [3,2,0,-4], pos = 1
输出：返回索引为 1 的链表节点
解释：链表中有一个环，其尾部连接到第二个节点。
```

**思路+状态：**有肌肉记忆的一道题，设置快慢指针，快指针步长比慢指针长，如果为环形链表，快慢指针会在环内相遇，相遇后再将慢指针置为head，从头出发，同时移动快指针，快慢指针会在环形链表入口处相遇。

````python
class Solution:
    def detectCycle(self, head: Optional[ListNode]) -> Optional[ListNode]:
        if head ==None or head.next == None:
            return None
        
        flag = 0
        res = head
        fast,slow = head,head

        while fast and fast.next:
            fast = fast.next.next
            slow = slow.next
            if slow == fast:
                flag = 1
                break
        if flag == 0:
            return None
        while res!=slow:
            slow = slow.next
            res = res.next
        return slow
````

#### 集合法

利用集合无重复元素的特性

````python
class Solution:
    def detectCycle(self, head: Optional[ListNode]) -> Optional[ListNode]:
        if head ==None or head.next == None:
            return None
        
        visited = set()
        while head:
            if head in visited:
                return head
            visited.add(head)
            head = head.next
        return None
````

