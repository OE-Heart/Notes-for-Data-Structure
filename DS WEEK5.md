# WEEK 5

- In a tree, the order of children does not matter. But in a binary tree, left child and right child are different.

#### Properties of Binary Trees

> - The maximum number of nodes on level $i$ is $2^{i-1},i\geq1$.
> - The maximum number of nodes in a binary tree of depth $k$ is $2^k-1,k\geq1$.
> -  For any nonempty binary tree, $n_0 = n_2 + 1$ where $n_0$ is the number of leaf nodes and $n_2$ is the number of nodes of degree 2.

---

### 3.3 Binary Search Trees

#### [Definition] A binary search tree is a binary tree.  It may be empty.  If it is not empty, it satisfies the following properties:

- 每个结点有一个互不不同的值
- 若左子树非空，则左子树上所有结点的值均小于根结点的值
- 若右子树非空，则右子树上所有结点的值均大于根结点的值
- 左、右子树也是是一棵二叉查找树

#### ADT

- **Objects** : A finite ordered list with zero or more elements.
- **Operations** :
  - SearchTree  MakeEmpty( SearchTree T )
  - Position  Find( ElementType X, SearchTree T )
  - Position  FindMin( SearchTree T )
  - Position  FindMax( SearchTree T )
  - SearchTree  Insert( ElementType X, SearchTree T )
  - SearchTree  Delete( ElementType X, SearchTree T )
  - ElementType  Retrieve( Position P )

#### Implementations

1. Find

   ```pseudocode
   Position Find( ElementType X, SearchTree T ) 
   { 
   	if ( T == NULL ) 
       	return NULL;  /* not found in an empty tree */
       if ( X < T->Element )  /* if smaller than root */
           return Find( X, T->Left );  /* search left subtree */
       else 
   		if ( X > T->Element )  /* if larger than root */
   	  		return  Find( X, T->Right );  /* search right subtree */
           else   /* if X == root */
   	  		return  T;  /* found */
   } 
   ```

   - $T(N)=S(N)=O(d)$ where $d$ is the depth of X

   Iterative program:

   ```pseudocode
   Position Iter_Find( ElementType X, SearchTree T ) 
   { 
   	while ( T )
   	{
       	if ( X == T->Element )  
   			return T ;  /* found */
           if ( X < T->Element )
               T = T->Left ; /*move down along left path */
           else
    			T = T-> Right ; /* move down along right path */
       }  /* end while-loop */
       return NULL ;   /* not found */
   } 
   ```

2. FindMin

   ```pseudocode
   Position FindMin( SearchTree T ) 
   { 
   	if ( T == NULL )   
       	return NULL; /* not found in an empty tree */
       else 
           if ( T->Left == NULL ) return T;  /* found left most */
           else return FindMin( T->Left );   /* keep moving to left */
   } 
   ```

3. FindMax

   ```pseudocode
   Position  FindMax( SearchTree T ) 
   { 
   	if ( T != NULL ) 
       	while ( T->Right != NULL )   
   			T = T->Right;   /* keep moving to find right most */
       return T;  /* return NULL or the right most */
   } 
   ```

4. Insert

   ```pseudocode
   SearchTree Insert( ElementType X, SearchTree T ) 
   { 
       if ( T == NULL ) /* Create and return a one-node tree */ 
       { 
   		T = malloc( sizeof( struct TreeNode ) ); 
   		if ( T == NULL ) 
   			FatalError( "Out of space!!!" ); 
   		else 
   		{ 
   			T->Element = X; 
   	    	T->Left = T->Right = NULL; 
   	    } 
       }  /* End creating a one-node tree */
       else  /* If there is a tree */
    		if ( X < T->Element ) 
   	   		T->Left = Insert( X, T->Left ); 
   		else 
   	   		if ( X > T->Element ) 
   	      		T->Right = Insert( X, T->Right ); 
   	   	/* Else X is in the tree already; we'll do nothing */ 
       return  T;   /* Do not forget this line!! */ 
   }
   ```

   - 内存越界后不会马上报错，在下一次free或malloc时会失败
   - Handle duplicated keys
   - $T(N)=O(d)$

5. Delete

   - Delete a leaf node : Reset its parent link to NULL
   - Delete a degree 1 node : Replace the node by its single child
   - Delete a degree 2 node : 用左子树最大值结点或右子树最小值结点替换

   ```pseudocode
   SearchTree Delete( ElementType X, SearchTree T ) 
   {    
   	Position TmpCell; 
       if ( T == NULL ) Error( "Element not found" ); 
       else if ( X < T->Element )  /* Go left */ 
   		T->Left = Delete( X, T->Left ); 
       else if ( X > T->Element )  /* Go right */ 
   	    T->Right = Delete( X, T->Right ); 
   	else  /* Found element to be deleted */ 
   	    if ( T->Left && T->Right ) {  /* Two children */ 
   	    /* Replace with smallest in right subtree */ 
   	    	TmpCell = FindMin( T->Right ); 
   	        T->Element = TmpCell->Element; 
   	        T->Right = Delete( T->Element, T->Right );  } /* End if */
   	    else 
   	    {  /* One or zero child */ 
   	        TmpCell = T; 
   	        if ( T->Left == NULL ) /* Also handles 0 child */ 
   		    	T = T->Right; 
   	        else if ( T->Right == NULL )  
   	        	T = T->Left; 
   	        free( TmpCell );  
   	     }  /* End else 1 or 0 child */
         return  T; 
   }
   ```

   - $T(N)=O(d)$
   - lazy deletion : 不做实际free操作，在结点上标记结点是否有效

6. Average-Case Analysis
   
   - The average depth over all nodes in a tree is $O(logN)$ on the assumption that all trees are equally likely.
   - 将$n$个元素存入二叉搜索树，树的高度将由插入序列决定

<img src="picture/5-1.png" alt="5-1" style="zoom:80%;" />

The correct answer is A.