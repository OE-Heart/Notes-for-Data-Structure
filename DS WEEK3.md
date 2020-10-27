# WEEK 3

### 2.3 The Stack ADT

- Last-In-First-Out (LIFO)
- **Objects** : A finite ordered list with zero or more elements.
- **Operations** :
  -  IsEmpty
  - CreatStack
  - DisposeStack
  - MakeEmpty
  - Push
  - Top
  - Pop
- A Pop(or Top) on an empty stack in an error in the stack ADT.
- Push on a full stack is an implementation error but not an ADT error.

#### Linked List Implementation (with a header node)

![3-1](picture/3-1.png)

- The calls to malloc and free are expensive. Simply keep another stack as a recycle bin.

#### Array Implementation of Stacks

```c
struct StackRecord {
	int Capacity;          /* size of stack */
	int TopOfStack;        /* the top pointer */
	/* ++ for push, -- for pop, -1 for empty stack */
	ElementType *Array;    /* array for stack elements */
}; 
```

- The stack model must be well **encapsulated(封装)**.  That is, no part of your code, except for the stack routines, can attempt to access the Array or TopOfStack variable.
- Error check must be done before Push or Pop (Top).

#### Application

1. Balancing Symbols

   检查括号是否平衡

   ```pseudocode
   Algorithm  {
       Make an empty stack S;
       while (read in a character c) {
           if (c is an opening symbol)
               Push(c, S);
           else if (c is a closing symbol) {
               if (S is empty)  { ERROR; exit; }
               else  {  /* stack is okay */
                   if  (Top(S) doesn’t match c)  { ERROR, exit; }
                   else  Pop(S);
               }  /* end else-stack is okay */
           }  /* end else-if-closing symbol */
       } /* end while-loop */ 
       if (S is not empty)  ERROR;
   }
   ```

2. Postfix Evaluation 后缀表达式

3. Infix to Postfix Conversion

   - The order of operands is the **same** in infix and postfix.
   - Operators with **higher** precedence appear **before** those with **lower** precedence.
   - Never pop a ’(‘ from the stack except when processing a ‘)’.
   - When ‘(’ is not in the stack, its precedence is the highest; but when it is in the stack, its precedence is the lowest. 
   - Exponentiation associates right to left.

4. Function Calls (System Stack)

    <img src="picture/3-2.png" alt="3-2" style="zoom:80%;" />

***

### 2.4 The Queue ADT

- First-In-First-Out (FIFO)
- **Objects** : A finite ordered list with zero or more elements.
- **Operations** : 
  - IsEmpty
  - CreatQueue
  - DisposeQueue
  - MakeEmpty
  - Enqueue
  - Front
  - Dequeue

#### Array Implementation of Queues

```c
struct QueueRecord {
	int Capacity ;       /* max size of queue */
	int Front;           /* the front pointer */
	int Rear;            /* the rear pointer */
	int Size;            /* Optional - the current size of queue */
	ElementType *Array;  /* array for queue elements */
 }; 
```

**Circular Queue** :

<img src="picture/3-3.png" alt="3-3" style="zoom: 64%;" /><img src="picture/3-4.png" alt="3-4" style="zoom: 64%;" />

