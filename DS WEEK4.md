# WEEK 4

## 3 Trees

### 3.1 Preliminaries

#### [Definition] A tree is a  collection of nodes. The collection can be empty; otherwise, a tree consists of (1)  a distinguished node r, called the root; (2) and zero or more nonempty (sub)trees, each of whose roots are connected by a directed edge from r.

- Subtrees must not connect together.  Therefore every node in the tree is the root of some subtree.

- There are N-1 edges in a tree with N nodes

#### Terminologies

>- degree of a node : 结点的子树个数
>- degree a tree : 结点的度的最大值
>- parent : 有子树的结点
>- children : the roots of the subtrees of a parent
>- siblings : children of the same parent
>- leaf(terminal node) : a node with degree 0(no children)
>- path from $n_1$ to $n_k$ : a **unique** sequence of nodes $n_1,n_2,\cdots,n_k$ such that $n_i$ is the parent of $n_{i+1}$ for $1\leq i<k$ 
>- length of path : 路径上边的条数
>- depth of $n_i$ : 从根结点到$n_i$结点的路径的长度($Depth(root)=0$)
>- height of $n_i$ : 从$n_i$结点到叶结点的最长路径的长度($Height(leaf)=0$)
>- height/depth of a tree : 根结点的高度/最深的叶结点的深度
>- ancestors of a node : 从此结点到根结点的路径上的所有结点
>- descendants of a node : 此结点的子树中的所有结点

#### List Representation

- The size of each node depends on the number of branches.

#### FirstChild-NextSibling Representation

<img src="picture/4-1.png" alt="4-1" style="zoom:80%;" />

- The representation is **not unique** since the children in a tree can be of any order.

---

### 3.2 Binary Trees

#### [Definition] A binary tree is a tree in which no node can have more than two children.

#### Tree Traversals (visit each node exactly once)

1. Preorder Traversal

   ```pseudocode
   void preorder( tree_ptr tree )
   { 
       if( tree )   
       {
           visit ( tree );
           for (each child C of tree )
               preorder ( C );
       }
   }
   ```

2. Postorder Traversal

   ```pseudocode
   void postorder( tree_ptr tree )
   {  
   	if( tree )   
   	{
           for (each child C of tree )
       		postorder ( C );
           visit ( tree );
       }
   }
   ```

3. Levelorder Traversal

   ```pseudocode
   void levelorder( tree_ptr tree )
   {   
   	enqueue ( tree );
       while (queue is not empty) 
       {
           visit ( T = dequeue ( ) );
           for (each child C of T )
               enqueue ( C );
       }
   }
   ```

4. Inorder Traversal

   ```pseudocode
   void inorder( tree_ptr  tree )
   {  
		if( tree )   
   	{
       	inorder ( tree->Left );
        	visit ( tree->Element );
        	inorder ( tree->Right );
      }
   }
   ```
   
   Iterative Program
   
   ```pseudocode
   void iter_inorder( tree_ptr tree )
   { 
   	Stack  S = CreateStack( MAX_SIZE );
   	for ( ; ; )  
   	{
       	for ( ; tree; tree = tree->Left )
           	Push ( tree, S ) ;
        	tree = Top ( S );  Pop( S );
        	if ( ! tree )  break;
        	visit ( tree->Element );
        	tree = tree->Right; 
       }
   }
   ```

#### Threaded Binary Trees

- A full binary tree with $n$ nodes has $2n$ links, and $n+1$ of them are NULL.

- Replace the NULL links by “threads” which will make traversals easier.

**Rules** :

> - If **Tree->Left** is null, replace it with a pointer to the inorder **predecessor(中序前驱)** of Tree.
> - If **Tree->Right** is null, replace it with a pointer to the inorder **successor(中序后继)** of Tree.
> - There must not be any loose threads.  Therefore a threaded binary tree must have a **head node** of which the left child points to the first node.

```c
typedef struct ThreadedTreeNode *PtrToThreadedNode;
typedef struct PtrToThreadedNode ThreadedTree;
typedef struct ThreadedTreeNode 
{
	int LeftThread;        /* if it is TRUE, then Left */
	ThreadedTree Left;     /* is a thread, not a child ptr.*/
	ElementType	Element;
	int RightThread;       /* if it is TRUE, then Right */
	ThreadedTree Right;    /* is a thread, not a child ptr.*/
}
```

- 线索化的实质就是将二叉链表中的空指针改为指向前驱或后继的线索。由于前驱和后继信息只有在遍历该二叉树时才能得到，所以，线索化的过程就是在遍历的过程中修改空指针的过程。