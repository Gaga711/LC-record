### HOT100-链表-19.删除链表的倒数第N个节点(中等)

#### 题目描述

给你一个链表，删除链表的倒数第 `n` 个结点，并且返回链表的头结点。

 

**示例 1：**

![img](https://assets.leetcode.com/uploads/2020/10/03/remove_ex1.jpg)

```
输入：head = [1,2,3,4,5], n = 2
输出：[1,2,3,5]
```

**示例 2：**

```
输入：head = [1], n = 1
输出：[]
```

**示例 3：**

```
输入：head = [1,2], n = 1
输出：[1]
```

 

**提示：**

- 链表中结点的数目为 `sz`
- `1 <= sz <= 30`
- `0 <= Node.val <= 100`
- `1 <= n <= sz`

 

**进阶：**你能尝试使用一趟扫描实现吗？



#### 提交记录

技巧：为了规避对头节点的判断，增加dummy node指向头节点。

##### 1. 遍历求解(首次提交)

遍历链表得到list，然后对list进行操作。

```python
class Solution:
    def removeNthFromEnd(self, head: Optional[ListNode], n: int) -> Optional[ListNode]:
        node=head
        list_node=[]
        while node:
            list_node.append(node)
            node=node.next
        final=None
        if len(list_node)==1:
            return None
        elif n==len(list_node):
            return list_node[1]
        elif n==1:
            final=list_node[-2]
            final.next=None
        else:
            final=list_node[-1*n-1]
            final.next=list_node[-1*n+1]
        return head
```

或者对列表遍历两次，第一次得到长度。官方提供的java代码：

````java
class Solution {
    public ListNode removeNthFromEnd(ListNode head, int n) {
        ListNode dummy = new ListNode(0, head);
        int length = getLength(head);
        ListNode cur = dummy;
        for (int i = 1; i < length - n + 1; ++i) {
            cur = cur.next;
        }
        cur.next = cur.next.next;
        ListNode ans = dummy.next;
        return ans;
    }

    public int getLength(ListNode head) {
        int length = 0;
        while (head != null) {
            ++length;
            head = head.next;
        }
        return length;
    }
}
````



##### 2. 栈求解

将所有节点入栈，pop出的第n个节点就是需要删除的节点。

````java
class Solution {
    public ListNode removeNthFromEnd(ListNode head, int n) {
        ListNode dummy = new ListNode(0, head);
        Deque<ListNode> stack = new LinkedList<ListNode>();
        ListNode cur = dummy;
        while (cur != null) {
            stack.push(cur);
            cur = cur.next;
        }
        for (int i = 0; i < n; ++i) {
            stack.pop();
        }
        ListNode prev = stack.peek();
        prev.next = prev.next.next;
        ListNode ans = dummy.next;
        return ans;
    }
}
````

时间黑洞：

<img src="images\image-20240301140322998.png" alt="image-20240301140322998" style="zoom:50%;" />



##### 3. 双指针求解

设置两个指针，一个比另一个领先n个，这样在指到末尾时，另一个指针就可以指向目标。

````java
class Solution {
    public ListNode removeNthFromEnd(ListNode head, int n) {
        ListNode dummy = new ListNode(0, head);
        ListNode first = head;
        ListNode second = dummy;
        for (int i = 0; i < n; ++i) {
            first = first.next;
        }
        while (first != null) {
            first = first.next;
            second = second.next;
        }
        second.next = second.next.next;
        ListNode ans = dummy.next;
        return ans;
    }
}
````



##### 4. 递归求解

函数的作用：给一个节点，返回该节点。

通过next指向下一个节点，并且count此时的倒数位置，如果count==目标，则return下一个节点，使得节点被删除。

时间复杂度比较高。

````python
class Solution:
    def removeNthFromEnd(self, head: Optional[ListNode], n: int) -> Optional[ListNode]:
        if not head: 
            # 这里居然不用显式地写明def init
            self.count = 0
            return head
        head.next = self.removeNthFromEnd(head.next, n)
        self.count += 1
        return head.next if self.count == n else head 
````

<img src="images\image-20240301141503641.png" alt="image-20240301141503641" style="zoom:50%;" />



#### 代码总结

- 栈的操作

  ````java
  //问栈顶，但并不pop
  ListNode prev = stack.peek();
  ````
  
- 自增符号

  ````java
  //++length和length++都是对变量进行自增操作
  //将会使length的值先自增为6，然后将6赋给result
  int result = ++length;
  //将会使result先被赋值为5，然后再将length的值自增为6
  int result = length++; 
  
  ````
