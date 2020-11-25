# WEEK 9

### 6.4 Shortest Path Algorithms

Given a digraph $G = ( V, E )$, and a cost function $c( e )$ for $e \in E( G )$. 

The **length** of a path $P$ from **source** to **destination** is $\sum_{e_i\subset P} c(e_i)$(also called **weighted path length**).

#### Single-Source Shortest-Path Problem

Given as input a weighted graph, $G = ( V, E )$, and a distinguished vertex $s$, find the shortest weighted path from $s$ to every other vertex in $G$.

- Note: If there is no negative-cost cycle, the shortest path from $s$ to $s$ is defined to be zero.

##### Unweighted Shortest Path

- Breadth-first search 广度优先遍历

Implementation : 

- Table[ i ].Dist ::= distance from $s$ to $v_i$  /* initialized to be $\infin$ except for s */
- Table[ i ].Known ::= 1 if $v_i$ is checked; or 0 if not
- Table[ i ].Path ::= for tracking the path   /* initialized to be 0 */

```c
void Unweighted( Table T )
{   
    int CurrDist;
    Vertex V, W;
    for ( CurrDist = 0; CurrDist < NumVertex; CurrDist ++ ) 
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

- Dijkstra’s Algorithm

  >1. the shortest path must go through **only** $v_i\in S$
  >2. $u$ is chosen so that 
  >3. if distance[$u_1$] < distance[$u_2$] and we add 

```c
void Dijkstra( Table T )
```





##### Graphs with Negative Edge Costs

```c
void WeightedNEgstive( Table T )
```



##### Acyclic Graphs



#### All-Pairs Shortest Path Problem

##### Method 1 

##### Method 2