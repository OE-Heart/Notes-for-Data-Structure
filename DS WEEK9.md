# WEEK 9

### 6.4 Shortest Path Algorithms



#### Single-Source Shortest-Path Problem



##### Unweighted Shortest Path

- Breadth-first search 广度优先遍历

Implementation : 



```c
void Unweighted( Table T )
```

The worst case : 
$$
T(N)=O(|V|^2)
$$
Improvement :

```c
void Unweighted( Table T )
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