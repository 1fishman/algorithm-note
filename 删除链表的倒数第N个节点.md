### 删除链表的倒数第N个节点
[https://leetcode-cn.com/problems/remove-nth-node-from-end-of-list/description/](https://leetcode-cn.com/problems/remove-nth-node-from-end-of-list/description/)

给定一个链表，删除链表的倒数第 n 个节点，并且返回链表的头结点。  

示例：
~~~
给定一个链表: 1->2->3->4->5, 和 n = 2.

当删除了倒数第二个节点后，链表变为 1->2->3->5.
~~~

进阶： 用一趟扫描实现。

一趟扫描，主要用双指针，一个在前pre，一个在后next，pre与next中间相隔N个节点，当next到最后一个节点时候，pre就代表倒数第N个节点。删除pre就好了。

实现代码：
~~~
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode* removeNthFromEnd(ListNode* head, int n) {
        ListNode * L = NULL;
        ListNode * R  = head;
        int i=0;
        while(R!=NULL){
            if(L!=NULL){
                L=L->next;
                R = R->next;
            }else if(i!=n){ 
                i++;
                R = R->next;
            }else if(i==n){
                L = head;
                R = R->next;
            }
        }
        if(L==NULL){
            return head->next;
        }
        if(L->next!=NULL){
            L->next = L->next->next;
        }
        return head;
    }
};
~~~