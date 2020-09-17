# K个一组翻转链表

题目：

**给你一个链表，每 k 个节点一组进行翻转，请你返回翻转后的链表。**

**k 是一个正整数，它的值小于或等于链表的长度。**

**如果节点总数不是 k 的整数倍，那么请将最后剩余的节点保持原有顺序。**

 

示例：

给你这个链表：1->2->3->4->5

当 k = 2 时，应当返回: 2->1->4->3->5

当 k = 3 时，应当返回: 3->2->1->4->5

 
说明：

**你的算法只能使用常数的额外空间。**

**你不能只是单纯的改变节点内部的值，而是需要实际进行节点交换。**

这是一道面试常见的题目，虽然是hard难度，但是和使链表倒序这道题思路差不太多，只不过这题需要K个一组。

我们可以先做一次遍历，找出长度length，然后再确定外循环的次数即length/k组，内循环为k，每组k个节点。
设置一个前驱节点pre，一个后继节点temp，每次将cur.next接到temp.next，pre.next接到temp，这样就完成了一组中某一个节点的倒序。要注意，每一组完成之后，需要将pre赋值为cur，即本组的尾节点作为下一组的头结点。
而cur继续后移。


```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
class Solution {
    public ListNode reverseKGroup(ListNode head, int k) {
        ListNode node=new ListNode(0),pre=node,cur=head,temp=null;
        node.next=head;
        int length=0;
        while (head!=null)
        {
            head=head.next;
            length++;
        }
        for (int i = 0; i < length/k; i++) {
            for (int j = 0; j < k-1; j++) {
                temp=cur.next;
                cur.next=temp.next;
                temp.next=pre.next;
                pre.next=temp;
            }
            pre=cur;
            cur=pre.next;
        }
        return node.next;
    }
}
```
