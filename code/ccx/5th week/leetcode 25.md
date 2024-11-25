

### ##题目描述：

#### 		##题目描述：

给你链表的头节点 head ，每 k 个节点一组进行翻转，请你返回修改后的链表。

k 是一个正整数，它的值小于或等于链表的长度。如果节点总数不是 k 的整数倍，那么请将最后剩余的节点保持原有顺序。

你不能只是单纯的改变节点内部的值，而是需要实际进行节点交换。

#### 		##输入描述：

无

#### 		##输出描述：

无

### ##题目链接：

[25. K 个一组翻转链表 - 力扣（LeetCode）](https://leetcode.cn/problems/reverse-nodes-in-k-group/description/)

### ##解题思路：

1.将每一组翻转并记录翻转后的新“头”和“尾”

2.每翻转一组后将新的尾接上新的头

### ##code：

```c
	/**
    * Definition for singly-linked list.
    * struct ListNode {
    *     int val;
    *     struct ListNode *next;
    * };
    */
    struct ListNode* reverseKGroup(struct ListNode* head, int k) {
        struct ListNode* cur=head;
        int cnt=0;
        while(cur){
            cur=cur->next;
           cnt++;
       }
       int n=cnt/k;
       cur=head;
       struct ListNode *pre=NULL,*next=head->next;
       struct ListNode* curtail;
       while(n>0){
           curtail=cur;
           for(int i=0;i<k;i++){
               cur->next=pre;
               pre=cur;
               cur=next;
               if(next)next=next->next;
           }
           if(n==cnt/k)head=pre;
           struct ListNode* tem=cur;
           if(n>1){
           for(int j=0;j<k-1;j++)tem=tem->next;
           curtail->next=tem;
           }else curtail->next=cur;
           n--;
       }
       return head;
    }








```



### ##题后反思：

此题初看很简单，但是里面的坑却很多

1.翻转后头节点需要改变

2.新尾需要接上”新“头，而不是下一个节点

3.最后一次翻转的尾需要接上下一个节点否则造成缺失