# WEEK 9

### 6.4 Shortest Path Algorithms

Given a digraph $G = ( V, E )$, and a cost function $c( e )$ for $e \in E( G )$. 

The **length** of a path $P$ from **source** to **destination** is $\sum_{e_i\subset P} c(e_i)$(also called **weighted path length**).

#### Single-Source Shortest-Path Problem

Given as input a weighted graph, $G = ( V, E )$, and a distinguished vertex $s$, find the shortest weighted path from $s$ to every other vertex in $G$.

> Note: If there is no negative-cost cycle, the shortest path from $s$ to $s$ is defined to be zero.

##### Unweighted Shortest Path

- Breadth-first search 广度优先遍历

Implementation : 

- Table[ i ].Dist ::= distance from $s$ to $v_i$  /* initialized to be $\infin$ except for $s$ */
- Table[ i ].Known ::= 1 if $v_i$ is checked; or 0 if not
- Table[ i ].Path ::= for tracking the path   /* initialized to be 0 */

```c
void Unweighted( Table T )
{   
    int CurrDist;
    Vertex V, W;
    for ( CurrDist = 0; CurrDist < NumVertex; CurrDist++ ) 
    {
        for ( each vertex V )
			if ( !T[ V ].Known && T[ V ].Dist == CurrDist ) 
            {
	    		T[ V ].Known = true;
	    		for ( each W adjacent to V )
	        		if ( T[ W ].Dist == Infinity ) 
                    {
						T[ W ].Dist = CurrDist + 1;
						T[ W ].Path = V;
	        		} /* end-if Dist == Infinity */
			} /* end-if !Known && Dist == CurrDist */
    }  /* end-for CurrDist */
}
```

The worst case : 

<img src="picture/9-1.png" alt="9-1" style="zoom: 50%;" />
$$
T(N)=O(|V|^2)
$$
Improvement :

```c
void Unweighted( Table T )
{   
    /* T is initialized with the source vertex S given */
    Queue Q;
    Vertex V, W;
    Q = CreateQueue( NumVertex );
    MakeEmpty( Q );
    Enqueue( S, Q ); /* Enqueue the source vertex */
    while ( !IsEmpty( Q ) ) 
    {
        V = Dequeue( Q );
        T[ V ].Known = true; /* not really necessary */
        for ( each W adjacent to V )
			if ( T[ W ].Dist == Infinity ) 
            {
	    		T[ W ].Dist = T[ V ].Dist + 1;
	    		T[ W ].Path = V;
	    		Enqueue( W, Q );
			} /* end-if Dist == Infinity */
    } /* end-while */
    DisposeQueue( Q ); /* free memory */
}
```

$$
T=O(|V|+|E|)
$$

##### Weighted Shorted Path

###### Dijkstra’s Algorithm

- Let S = { $s$ and $v_i$’s whose shortest paths have been found }
- For any $u\notin S$,  define distance [ u ] = minimal length of path { $s\rightarrow(v_i\in S)\rightarrow u$ }.  If the paths are generated in non-decreasing order, then :
  - the shortest path must go through **only** $v_i\in S$
  - **Greedy Method** : $u$ is chosen so that distance[ u ] = min{ $w \notin S$ | distance[ w ] }  (If $u$ is not unique, then we may select any of them)
  - if distance[$u_1$] < distance[$u_2$] and add $u_1$ into $S$, then distance [ $u_2$ ] may change.  If so, a shorter path from $s$ to $u_2$ must go through $u_1$ and distance [ $u_2$ ] = distance [ $u_1$ ] + length(< $u_1$, $u_2$>).

```c
typedef int Vertex;
struct TableEntry
{
	List Header; /*Adjacency list*/
	int Known;
	DistType Dist;
	Vertex Path;
};
/*Vertices are numbered from 0*/
#define NotAVertex (-1)
typedef struct TableEntry Table[ NumVertex ];
```

```c
void InitTable(Vertex Start, Graph G, Table T)
{ 
	int i;
	ReadGraph(G, T); /* Read graph somehow */
	for(i = 0; i < NumVertex; i++)
	{
		T[ i ].Known = False;
		T[ i ].Dist = Infinity;
		T[ i ].Path = NotAVertex;
	}
	T[ Start ].dist = O;
}
```

```c
/*Print shortest path to V after Dijkstra has run*/
/*Assume that the path exists*/
void PrintPath(Vertex V, Table T)
{
	if (T[ V ].Path != NotAVertex)
	{
		PrintPath(T[ V ].Path, T);
		printf(" to") ;
	}
	printf("%v", V) ; /* %v is pseudocode * /
```

```c
void Dijkstra( Table T )
{ 
    Vertex V, W;
    for ( ; ; ) 
    {
        V = smallest unknown distance vertex;
        if ( V == NotAVertex ) break; 
        T[ V ].Known = true;
        for ( each W adjacent to V )
			if ( !T[ W ].Known ) 
	    		if ( T[ V ].Dist + Cvw < T[ W ].Dist ) 
            	{
	    			Decrease( T[ W ].Dist to T[ V ].Dist + Cvw );
					T[ W ].Path = V;
	    		} /* end-if update W */
    } /* end-for( ; ; ) */
}
```

###### Implementation 1

- Simply scan the table to find the smallest unknown distance vertex.——$O(|V|)$
- Good if the graph is dense

$$
T=O(|V|^2+|E|)
$$

###### Implementation 2

- 堆优化

- Keep distances in a priority queue and call `DeleteMin` to find the smallest unknown distance vertex.——$O(\log|V|)$

- 更新的处理方法

  - **Method 1** : `DecreaseKey`——$O(\log|V|)$

    $T=O(|V|\log|V|+|E|\log|V|)=O(|E|\log|V|)$

  - **Method 2** : insert W with updated Dist into the priority queue

    Must keep doing `DeleteMin` until an unknown vertex emerges

    $T=O(|E|\log|V|)$ but requires $|E|$ `DeleteMin` with `|E|` space

- Good if the graph is sparse

###### Improvements

- Pairing heap
- Fibonacci heap

##### Graphs with Negative Edge Costs

```c
void WeightedNegative( Table T )
{
    Queue Q;
    Vertex V, W;
    Q = CreateQueue (NumVertex );  
    MakeEmpty( Q );
    Enqueue( S, Q ); /*Enqueue the source vertex*/
    while ( !IsEmpty( Q ) ) 
    {
        V = Dequeue( Q );
        for ( each W adjacent to V )
		if ( T[ V ].Dist + Cvw < T[ W ].Dist ) 
        {
	    	T[ W ].Dist = T[ V ].Dist + Cvw;
	    	T[ W ].Path = V;
	    	if ( W is not already in Q )
	        	Enqueue( W, Q );
		} /*end-if update*/
    } /*end-while */
    DisposeQueue( Q ); /*free memory*/
}
```

> Note : Negative-cost cycle will cause indefinite loop

$$
T=O(|V|\times|E|)
$$

##### Acyclic Graphs

- If the graph is acyclic, vertices may be selected in **topological order** since when a vertex is selected, its distance can no longer be lowered without any incoming edges from unknown nodes.
- $T=O(|E|+|V|)$ and no priority queue is needed.

###### AOE(Activity on Edge) Networks

![image-20210124185420522](picture/image-20210124185420522.png)

![image-20210124185541801](picture/image-20210124185541801.png)

![image-20210124185508683](picture/image-20210124185508683.png)

#### All-Pairs Shortest Path Problem

- For all pairs of $v_i$ and $v_j$ ( $i\neq j$ ), find the shortest path between.

##### Method 1 

- Use **single-source algorithm** for $|V|$ times.
- $T=O(|V|^3)$, works fast on sparse graph.

##### Method 2

- 动态规划
- $O(|V|^3)$ algorithm given in Chapter 10, works faster on dense graphs.

---