---
title: "143. Reorder List"
description: "Day VII"
date: 04/09/2024
---

Quite an intriguing problem.

Initially, I thought this solution would require extra storage to store some `ListNode`s as the order is `L0 -> Ln -> L1 -> L(n-1)`...

I played around with some examples and observed the following pattern:
- First element -> last element -> second element -> second last element ...

### Initial Observations:
- **Ex1:** `[]` -> No reordering as the list is empty.
- **Ex2:** `[1]` -> No reordering as there is only one element.
- **Ex3:** `[1, 2]` -> No reordering as there are only two elements.
- **Ex4:** `[1, 2, 3]` -> `[1, 3, 2]`, reordering happens as we have three elements.

So, for reordering to occur, there must be at least three nodes in the input.

```cpp
//Base Cases
if(head==NULL || head->next==NULL || head->next->next==NULL) return ;
```

Had this been an array, I would need two pointers: one at the start and one at the end. I would increment the start pointer and decrement the end pointer. That's when stacks came to mind. As I process a node, if I keep them in a stack, I will have the last element first, then the second last, and so on. I already have the head pointer, so now I have the head and `ListNode` elements from the end in reverse order in the stack, allowing me to do the reordering.

### Do I Need to Store All the ListNodes in the Stack?
No, I would only need to store the right half of the nodes in the stack. Using the `head` pointer, I can traverse the left half.

The goal now is to find the halfway point.

### Approach I:
In the first loop, I can compute the length, and then in the second loop, I can reach halfway.

### Approach II:
I can use one loop with two pointers: `slowPtr` and `fastPtr`. I increment `fastPtr` by two and `slowPtr` by one. At the end, `slowPtr` will point to the halfway node. Let's call `slowPtr` the `halfWay` pointer.


```cpp
ListNode* halfWay = head;
ListNode* fastPtr = head->next->next;
//Compute Half way
while(fastPtr!=NULL){
    halfWay = halfWay->next;
    fastPtr = fastPtr->next;
    if(fastPtr==NULL)  break;
    else fastPtr = fastPtr->next;
}
```

### Considering the Case for Odd Length Lists:
- **Ex:** `[1, 2, 3, 4, 5]`: My halfway point is `3`, which is correct.
- **Ex:** `[1, 2, 3, 4, 5, 6]`: My halfway point is still `3`, but the correct halfway point should be `4`.

For even-length lists, I need to take the upper middle point. Therefore, `halfWay` should be `halfWay->next`.

### How Do I Know Whether the Length Is Even or Odd?
This can be determined based on whether `fastPtr` can be incremented or not. Note that `fastPtr` is incremented twice. If it reaches the `NULL` pointer after the first increment, the list has an odd length. Otherwise, it has an even length. Incorporating this into the code:

```cpp
ListNode* halfWay = head;
ListNode* fastPtr = head->next->next;
bool oddLength = false;
//Compute Half way
while(fastPtr!=NULL){
    halfWay = halfWay->next;
    fastPtr = fastPtr->next;
    if(fastPtr==NULL){
        oddLength = true;
        break;
    }
    else fastPtr = fastPtr->next;
}
if(!oddLength) halfWay = halfWay->next;
```

Now that I have the halfway point, let me first put all the elements to the right of halfway into the stack.
Note: `halfWay` should not go into the stack and will point to `NULL`.

```cpp
//All nodes in front of half way will go to stack
stack<ListNode*> stk;
//And halfway will point to null
ListNode* pastHalfWay = halfWay->next;
halfWay->next = NULL;
while(pastHalfWay!=NULL){
    stk.push(pastHalfWay);
    pastHalfWay = pastHalfWay->next;
}
```

### Now Let's Do the Reordering
We need the current element and its next element. We also need the popped element from the stack.

While the stack is not empty, keep popping and perform the reordering.

```cpp
while(!stk.empty()){
    stkTop = stk.top();
    stk.pop();
    otherStore = curr->next;
    curr->next = stkTop;
    stkTop->next = otherStore;
    curr = otherStore;
}
```

### Complete Code:

```cpp
void reorderList(ListNode* head) {
    //If length belongs to {0, 1, 2} no interleaving.
    if(head==NULL || head->next==NULL || head->next->next==NULL) return ;
    //Length is >=3
    ListNode* halfWay = head;
    ListNode* fastPtr = head->next->next;
    bool oddLength = false;
    //Compute Half way
    while(fastPtr!=NULL){
        halfWay = halfWay->next;
        fastPtr = fastPtr->next;
        if(fastPtr==NULL){
            oddLength = true;
            break;
        }
        else fastPtr = fastPtr->next;
    }
    if(!oddLength) halfWay = halfWay->next;

    //All nodes in front of half way will go to stack
    stack<ListNode*> stk;
    //And halfway will point to null
    ListNode* pastHalfWay = halfWay->next;
    halfWay->next = NULL;
    while(pastHalfWay!=NULL){
        stk.push(pastHalfWay);
        pastHalfWay = pastHalfWay->next;
    }
    ListNode* stkTop;
    ListNode* curr = head;
    ListNode* otherStore;

    while(!stk.empty()){
        stkTop = stk.top();
        stk.pop();
        otherStore = curr->next;
        curr->next = stkTop;
        stkTop->next = otherStore;
        curr = otherStore;
    }
    return ;
}
```