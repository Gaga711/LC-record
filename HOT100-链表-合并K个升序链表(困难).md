### HOT100-链表-23.合并K个升序链表(困难)

#### 题目描述

给你一个链表数组，每个链表都已经按升序排列。

请你将所有链表合并到一个升序链表中，返回合并后的链表。

**示例 1：**

```
输入：lists = [[1,4,5],[1,3,4],[2,6]]
输出：[1,1,2,3,4,4,5,6]
解释：链表数组如下：
[
  1->4->5,
  1->3->4,
  2->6
]
将它们合并到一个有序链表中得到。
1->1->2->3->4->4->5->6
```

**示例 2：**

```
输入：lists = []
输出：[]
```

**示例 3：**

```
输入：lists = [[]]
输出：[]
```

 

**提示：**

- `k == lists.length`
- `0 <= k <= 10^4`
- `0 <= lists[i].length <= 500`
- `-10^4 <= lists[i][j] <= 10^4`
- `lists[i]` 按 **升序** 排列
- `lists[i].length` 的总和不超过 `10^4`



#### 提交记录

##### 1. 遍历求解

通过遍历取出所有的元素，然后创建链表。

```python
class Solution:
    def mergeKLists(self, lists: List[Optional[ListNode]]) -> Optional[ListNode]:
        al = []
        for chain in lists:
            while chain:
                al.append(chain.val)
                chain = chain.next
        # 对 al 列表进行升序排序
        al.sort()

        # 创建合并后的链表
        dummy = ListNode(0)
        node = dummy
        for val in al:
            node.next = ListNode(val)
            node = node.next

        return dummy.next
```

<img src="images\image-20240301160512818.png" alt="image-20240301160512818" style="zoom:50%;" />



##### 2. 优先队列求解

优先队列（Priority Queue）是一种特殊的队列数据结构，其中每个元素都有一个相关的优先级。优先队列中的元素可以按照优先级顺序来访问，而不是按照它们被插入的顺序。

在Java中，优先队列通常通过堆（Heap）来实现。具体来说，`PriorityQueue`类使用了最小堆（Min Heap）来实现优先队列。

最小堆是一种完全二叉树，其中父节点的值小于或等于其子节点的值。在最小堆中，根节点的值是整个堆中的最小值。

````java
class Solution {
    public ListNode mergeKLists(ListNode[] lists) {
        Queue<ListNode> pq = new PriorityQueue<>((v1, v2) -> v1.val - v2.val);
        for (ListNode node: lists) {
            if (node != null) {
                pq.offer(node);
            }
        }

        ListNode dummyHead = new ListNode(0);
        ListNode tail = dummyHead;
        while (!pq.isEmpty()) {
            ListNode minNode = pq.poll();
            tail.next = minNode;
            tail = minNode;
            if (minNode.next != null) {
                pq.offer(minNode.next);
            }
        }

        return dummyHead.next;
    }
}
````

<img src="images\image-20240301161756656.png" alt="image-20240301161756656" style="zoom:50%;" />



##### 3. 两两合并-递归求解

````java
class Solution {
    public ListNode mergeKLists(ListNode[] lists) {
        if (lists.length == 0) {
            return null;
        }
        return merge(lists, 0, lists.length - 1);
    }

    private ListNode merge(ListNode[] lists, int lo, int hi) {
        if (lo == hi) {
            return lists[lo];
        }
        int mid = lo + (hi - lo) / 2;
        ListNode l1 = merge(lists, lo, mid);
        ListNode l2 = merge(lists, mid + 1, hi);
        return merge2Lists(l1, l2);
    }
}

````





##### 4. 两两合并-迭代求解

````java
class Solution {
    public ListNode mergeKLists(ListNode[] lists) {
        if (lists.length == 0) {
            return null;
        }
        int k = lists.length;
        while (k > 1) {
            int idx = 0;
            for (int i = 0; i < k; i += 2) {
                if (i == k - 1) {
                    lists[idx++] = lists[i];
                } else {
                    lists[idx++] = merge2Lists(lists[i], lists[i + 1]);
                }
            }
            k = idx;
        }
        return lists[0];
    }
}
````





#### 代码总结

- 优先队列

  ````java
  //创建优先队列
  //优先的比较方式是：当前node.val与其他node比较
  //确保元素按照节点值的升序顺序排列
  Queue<ListNode> pq = new PriorityQueue<>((v1, v2) -> v1.val - v2.val);
  ````
  
  
  
