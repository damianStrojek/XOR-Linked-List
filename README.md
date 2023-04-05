# XOR Linked List Basic

The task was to implement a bidirectional list. A classic bidirectional list stores successor and predecessor addresses in nodes, in our task the list stores only xor of these addresses in nodes. This saves approximately 50% of memory used by the list structure.

## Theory

The terms list node and list element are used interchangeably and means the same. During adding a new node to the list, we write the xor value of the predecessor and the successor to this node. If it is the first or the last node, as the xor of addresses, we write the address of the successor or the predecessor, respectively, because the address xor with 0 (we take zero as NULL) does not change the address value. 

From this it follows that we need to store a pointer to the first and last element of the list, which will allow us to browse the list in both directions. If we want to go to the next node in the assumed direction, we read its address as xor values stored in the current and previously visited node. As described earlier, at the boundary node this will be the direct address (since the value of a hypothetical nonexistent earlier node is 0).

In the case of deleting a node, updating the xor values of the deleted node's predecessor and successor is required. When deleting an boundary node, there is only one neighbor that needs to be updated.

The task requires that we have an additional pointer showing the current value used by the ACTUAL command described later. In this case, we also need to keep one of the neighbors (successor or predecessor, preferably both) of pointer showing the current value. Otherwise, we will not be able to restore their addresses (successor or predecessor of the current element). In this case, we must additionally store one of the neighbors (successor or predecessor, preferably both), otherwise we will not be able to restore their addresses.  It would seem that this does not apply to the first and last node, but we are not able to conclude (without additional information) that the index to the current element just points to one of the boundary elements.

## Functionality

The list provides the following functionalities related to the following commands:

- `ACTUAL` - value of the currently pointed element. For an empty list it is NULL and after adding the first element to the list ACTUAL returns the value of that element as long as it is not moved by other operations.

- `NEXT` - prints the value of the ACTUAL successor setting it ACTUAL at the same time. If ACTUAL points to the last element of the queue, its successor will be the first element of the queue.

- `PREV` - prints the value of the predecessor ACTUAL setting it ACTUAL at the same time. If ACTUAL points to the first element of a queue, its predecessor will be the last element of the queue.

- `ADD_BEG N` - adding an element with value N to the beginning of the list.

- `ADD_END N` - adding an element with value N to the end of the list.

- `ADD_ACT N` - adding an element with the value of N as a predecessor of the currently selected element (ACTUAL).

- `DEL_BEG` - removes the first node from the list.

- `DEL_END` - removes the last node from the list.

- `DEL_VAL N` - removes from the list all nodes whose value is equal to N.

- `DEL_ACT` - removes from the list the node that ACTUAL points to, simultaneously setting ACTUAL to PREV. In the case of the PREV does not exist (ACTUAL was the first element of the list) ACTUAL shows the last element of the list.

- `PRINT_FORWARD` - prints the contents of the list from the first to the last element.

- `PRINT_BACKWARD` - prints the contents of the list from the last to the first element.

The behavior of the `NEXT`, `PREV` and `DEL_ACT` commands suggests action on a circular queue in which (except for an empty queue) there is always a successor and a predecessor for each node. When such a queue has only one element, the xor value of this node is 0. However, the implementation of the circular queue is not required and is not forbidden. Instead, it is possible to implement additional functionality of the `NEXT` and `PREV` commands. It will be run when an exceptional situation occurs, i.e. `NEXT` call when `ACTUAL` points to the last element of the queue, or `PREV` or `DEL_ACT` when `ACTUAL` points to the first element of a non-cyclic queue.

`DEL_BEG`, `DEL_END`, `DEL_VAL`, and `DEL_ACT` commands for an empty list do not remove anything. In each of these cases, removing the currently pointed element (`ACTUAL` command) should result in moving the currently pointed element pointer to the preceding element, and if it does not exist, to the last element of the list.

## Requirements

Standard C ++ Template Library (STL) cannot be used.

# XOR Linked List with FIFO Queue

The task is to implement FIFO queue using doubly or singly linked list. It is an extension of XOR linked list assignment XOR linked list and the list implemented for that assignment can be reused; but also an STL's list (`<list>`) can be used. In addition to the functions of the XOR list (ADD_BEG, ADD_END, DEL_BEG, DEL_END, PRINT_FORWARD, PRINT_BACKWARD) from the XOR list assignment 

## Functionality

XOR Linked List with FIFO Queue implements:

- `SIZE` - number of elements in the list
 
- `PUSH N` - add element N to the end of the queue.

- `POP` - remove the first element from the queue.

- `PRINT_QUEUE` - print the queue from the first element to the lastly added.

- `COUNT` - the number of elements in queue ≤ SIZE.

- `GARBAGE_SOFT` - change the values of all elements of the list that do not belong to the queue to 0.

- `GARBAGE_HARD` - remove all elements from the list that does not belong to the queue.

The FIFO queue extends the list to the pointers to the beginning (`FRONT`) and the end (`BACK`) of the queue. After inserting an element to the queue the pointer to the end of the queue (`BACK`) has to be changed so it will point to the next element of the list. After removing an element from the queue (`FRONT`) pointer has to start pointing at the element after the previously pointer by (`FRONT`). Notice, that by both removing and adding the element the queue has to “follow the list”. 

To avoid memory loss and to be able to reuse one can use a cyclical list or after reaching the end of the queue start from the beginning. Also, one has to handle exceptions: inserting an element to an empty queue (no elements), adding elements to full list (the number of elements of the queue equal to the number of elements of the list), or trying to remove elements from an empty queue.When the list is empty (no elements in it) the pointers point to nullptr. 

When adding an element (`PUSH N`) to the queue and the list is empty, then before executing the operation one element has to be added to the list. Similarly, the list has to be extended when the list is not empty, but all the elements are occupied by the queue. Inserted element should be inserted before the element pointed by pointer to the end of the queue (`BACK`).

From time to time it might be useful to remove the elements that do not contain elements from the queue. Such a situation may occur if there were many insertions and after the insertions there was a sequence of pops. The situation is quite common in queuing systems. Consider for example STOS system. Normally the queues are empty or contains a few tasks. But near the deadlines they may grow considerably, which can overload the system and crash it. The operation to shrink the size of the list is implemented by GARBAGE_HARD.