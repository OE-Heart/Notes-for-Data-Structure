# WEEK 6

## 4 Priority Queues (Heaps)

### 4.1 ADT Model

- **Objects** :
- **Operations** :

---

### 4.2 Implementations

#### Array

#### Linked List 

#### Ordered Array

- Insertion — find the proper position ~$O(\log n)$

#### Ordered Linked List

#### Binary Search Tree

---

### 4.3 Binary Heap

#### Structure Property

**[Definition]** 

- A complete binary tree of height $n$ has between $2^k$ and $2^{k+1}-1$ nodes.
- Array Representation

**[Lemma]** 

1. $index\,of\,parent(i)= $
2. $index\,of\,left\_child(i)=$
3. $index\,of\,right\_child(i)=$

```

```

#### Heap Order Property

**[Definition]**

- We can declare a **max** heap by changing the heap order property.

#### Basic Heap Operations

1. Insertion

   

   ```
   
   ```

   $$
   T(N)=O(\log N)
   $$

2. DeleteMin (必考)

   

   ```
   
   ```

#### Other Heap Operations

- 查找除最小值之外的值需要对整个堆进行线性扫描

1. DecreaseKey — Percolate up

   

2. IncreaseKey — Percolate down

   

3. Delete

4. BuildHeap

   **[Theorem]** 

---

### 4.4 Applications of Priority Queues

#### Heap Sort

#### 查找一个序列中第k大的元素

---

### 4.5 $d$-Heaps — All nodes have $d$ children

**Note** :

> - 