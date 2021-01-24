# WEEK 8

## 6 Graph Algorithms

### 6.1 Definitions

- $G( V, E )$ where $G$ = graph, $V = V( G )$ = finite nonempty set of vertices, and $E = E( G )$ = finite set of edges.

#### Undirected graph 

- $( v_i , v_j ) = ( v_j , v_i )$ = the same edge.

#### Directed graph(diagraph)

<img src="picture/8-1.png" alt="8-1" style="zoom:120%;" />

#### Restrictions

- **Self loop** is illegal.
- **Multigraph** is not considered.

#### Complete graph

- A graph that has the maximum number of edges.

![image-20210124163038265](picture/image-20210124163038265.png)

#### Adjacent

<img src="picture/image-20210124163324744.png" alt="image-20210124163324744" style="zoom:120%;" />

<img src="picture/image-20210124163301833.png" alt="image-20210124163301833" style="zoom:120%;" />

#### Subgraph

$$
G'\subset G=V(G')\subseteq V(G) \&\& E(G')\subseteq E(G)
$$

#### Path

- Path($\subset G$) from $v_p$ to $v_q$ = $\{v_p,v_{i1},v_{i2},\cdots,v_{in},v_q\}$ such that $(v_p,v_{i1}),(v_{i1},v_{i2}),\cdots,(v_{in},v_q)$ belong to $E(G)$

#### Length of a path

- number of edges on the path

#### Simple path

- $v_{i1},v_{i2},\cdots,v_{in}$ are distinct.

#### Cycle

- simple path with $v_p=v_q$

#### Connected

- $v_i$ and $v_j$ in an undirected $G$ are connected if there is a path from $v_i$ to $v_j$ (and hence there is also a path from $v_j$ to $v_i$)
- An undirected graph $G$ is connected if every pair of distinct $v_i$ and $v_j$ are connected

#### (Connected) Component of an undirected G

- the maximal connected subgraph

#### Tree

- a graph that is connected and acyclic(非循环的)

#### DAG

- a directed acyclic graph

#### Strongly connected directed graph G

- For every pair of $v_i$ and $v_j$ in $V( G )$, there exist directed paths from $v_i$ to $v_j$ and from $v_j$ to $v_i$.  
- If the graph is connected without direction to the edges, then it is said to be weakly connected

#### Strongly connected component

- the maximal subgraph that is strongly connected

#### Degree

- number of edges incident to v

- For a directed G, we have **in-degree** and **out-degree**.

- Given G with $n$ vertices and $e$ edges, then
  $$
  e=(\sum_{i=0}^{n-1}d_i)/2\quad where\quad d_i=degree(v_i)
  $$
  

---

### 6.2 Representation of Graphs

#### Adjacency Matrix

![image-20210124163641976](picture/image-20210124163641976.png)

> Note : If G is undirected, then adj_mat[]\[] is symmetric. Thus we can save space by storing only half of the matrix.

<img src="picture/image-20210123194735917.png" alt="image-20210123194735917"  />

- This representation wastes space if the graph has a lot of vertices but very few edges.

- To find out whether or not $G$ is connected, we’ll have to examine all edges. In this case $T$ and $S$ are both $O( n^2 )$.

#### Adjacency Lists

- Replace each row by a linked list

![image-20210124164723362](picture/image-20210124164723362.png)

> Note : The order of nodes in each list does not matter.

- For undirected $G$, $S$ = $n$ heads + $2e$ nodes  = $(n+2e)$ ptrs + $2e$ ints
- Degree(i) = number of nodes in graph[i]\(if $G$ is undirected)
- $T$ of examine $E(G)$ = $O(n+e)$

![image-20210124165346405](picture/image-20210124165346405.png)

#### Adjacency Multilists

![image-20210124164607434](picture/image-20210124164607434.png)

- Sometimes we need to mark the edge after examine it, and then find the next edge.

#### Weighted Edges

- adj_mat [ i ] [ j ] = weight
- adjacency lists / multilists :  add a weight field to the node

---

### 6.3 Topological Sort

#### AOV Network

- digraph $G$ in which $V( G )$ represents activities and $E( G )$ represents precedence relations 
- Feasible AOV network must be a directed acyclic graph.
- $i$  is a **predecessor** of $j$ = there is a path from $i$  to $j$
- $i$  is an **immediate predecessor** of  $j$ = $< i,  j > \in E( G )$. Then $j$ is called a **successor**(**immediate successor**) of $i$

#### Partial order

- a precedence relation which is both **transitive** and **irreflexive** 

> Note : If the precedence relation is reflexive, then there must be an $i$ such that $i$ is a predecessor of $i$.  That is, $i$ must be done before $i$ is started. Therefore if a project is **feasible**, it must be **irreflexive**.

#### [Definition] A *topological order* is a linear ordering  of the vertices of a graph such that, for any two vertices, $i$, $j$, if $i$ is a predecessor of $j$ in the network then $i$ precedes $j$ in the linear ordering.

> Note : The topological orders may **not be unique** for a network.

```c
/*Test an AOV for feasibility, and generate a topological order if possible*/
void Topsort( Graph G )
{   
	int Counter;
    Vertex V, W;
    for ( Counter = 0; Counter < NumVertex; Counter++ ) 
    {
		V = FindNewVertexOfDegreeZero( );
		if ( V == NotAVertex ) 
        {
	    	Error ( “Graph has a cycle” );   
            break;  
        }
		TopNum[ V ] = Counter; /* or output V */
		for ( each W adjacent to V )
	    	Indegree[ W ]––;
    }
}
```

$$
T=O(|V|^2)
$$

```c
/*Improvment:Keep all the unassigned vertices of degree 0 in a special box (queue or stack)*/
void Topsort( Graph G )
{   
	Queue Q;
    int Counter = 0;
    Vertex V, W;
    Q = CreateQueue( NumVertex );  
    MakeEmpty( Q );
    for ( each vertex V )
		if ( Indegree[ V ] == 0 ) Enqueue( V, Q );
    while ( !IsEmpty( Q ) ) 
    {
		V = Dequeue( Q );
		TopNum[ V ] = ++Counter; /* assign next */
		for ( each W adjacent to V )
	    	if (––Indegree[ W ] == 0 ) Enqueue( W, Q );
    }  /* end-while */
    if ( Counter != NumVertex )
	Error( “Graph has a cycle” );
    DisposeQueue( Q ); /* free memory */
}
```

$$
T=O(|V|+|E|)
$$

---

