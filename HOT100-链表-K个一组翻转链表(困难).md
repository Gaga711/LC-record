### HOT100-链表-25.K个一组翻转链表(困难)

#### 题目描述

给你链表的头节点 `head` ，每 `k` 个节点一组进行翻转，请你返回修改后的链表。

`k` 是一个正整数，它的值小于或等于链表的长度。如果节点总数不是 `k` 的整数倍，那么请将最后剩余的节点保持原有顺序。

你不能只是单纯的改变节点内部的值，而是需要实际进行节点交换。

 

**示例 1：**

![img](https://assets.leetcode.com/uploads/2020/10/03/reverse_ex1.jpg)

```
输入：head = [1,2,3,4,5], k = 2
输出：[2,1,4,3,5]
```

**示例 2：**

![img](https://assets.leetcode.com/uploads/2020/10/03/reverse_ex2.jpg)

```
输入：head = [1,2,3,4,5], k = 3
输出：[3,2,1,4,5]
```

 

**提示：**

- 链表中的节点数目为 `n`
- `1 <= k <= n <= 5000`
- `0 <= Node.val <= 1000`

 

**进阶：**你可以设计一个只用 `O(1)` 额外内存空间的算法解决此问题吗？



#### 提交记录

##### 1. 遍历求解(首次提交)

```python
class Solution:
    def reverseKGroup(self, head: Optional[ListNode], k: int) -> Optional[ListNode]:
        dummy=ListNode(0)
        node1,node2,count=dummy,head,0
        current=[]
        while count<=k:
            if not node2:
                if current:
                    node1.next=current[0]
                else:
                    node1.next=None
                break
            current.append(node2)
            node2=node2.next
            count+=1
            if count==k:
                while current:
                    node1.next=current.pop()
                    node1=node1.next
                count=0
        return dummy.next
```

<img src="images\image-20240301163018145.png" alt="image-20240301163018145" style="zoom:50%;" />










