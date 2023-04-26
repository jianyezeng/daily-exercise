删除链表的倒数第N个节点

[19. 删除链表的倒数第 N 个结点 - 力扣（LeetCode）](https://leetcode.cn/problems/remove-nth-node-from-end-of-list/)

> 给你一个链表，删除链表的倒数第 n 个结点，并且返回链表的头结点。

> 示例 1：
>
> 输入：head = [1,2,3,4,5], n = 2
> 输出：[1,2,3,5]

### Python

解：

```python
# Definition for singly-linked list.
# class ListNode(object):
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution(object):
    def removeNthFromEnd(self, head, n):
        """
        :type head: ListNode
        :type n: int
        :rtype: ListNode
        """
        p = ListNode(next = head)
        q = ListNode(next = head)
        index = 0
        while p.next != None and index < n:
            p = p.next
            index += 1
        if index != n:
            return head
        if p.next == None:
            return head.next
        while p.next != None:
            p = p.next
            q = q.next
        q.next = q.next.next
        return head
```

### 笔记

1. 删除链表倒数第N个节点，最无脑粗暴的方法就是直接先遍历一遍，得到链表长度，之后确定要删除节点的位置，再进行删除，但这种方法时间复杂度较高，不是特别好；
2. 第二种方法即为使用双指针法，因为要删除节点位置与尾端的距离是一定的，我们可以利用这个关系，在一开始就让两个指针指向节点之间距离与其相同，即先让p走n步，之后再一起走，当p走到尾端时，q指向即为要删除的节点（解中使用了虚拟头节点，因此p.next ，q.next才是笔记中的p，q），要注意一些特殊情况，例如n大于链表长度。