# WEEK 7

## 5 The Disjoint Set

### 5.1 Equivalence Relations

#### [Definition] A *relation R* is defined on a set *S* if for every pair of elements *(a, b)*, *a, b $\in$ S*, *a R b* is either true or false.  If *a R b* is true, then we say that *a* is related to *b*.

#### [Definition] A relation, ~, over a set, *S*, is said to be an *equivalence relation* over *S* if it is symmetric, reflexive, and transitive over *S*.

#### [Definition] Two members *x* and *y* of a set *S* are said to be in the same *equivalence class* if *x ~ y*.

---

### 5.2 The Dynamic Equivalence Problem

- Given an equivalence relation ~, decide for any *a* and *b* if *a ~ b*

  ```pseudocode
  Algorithm: (Union/Find)
  {
  	/* step 1: read the relations in */
      Initialize N disjoint sets;
      while ( read in a ~ b ) 
      {
      	if ( !(Find(a) == Find(b)) )  /*Dynamic(on-line)*/
  			Union the two sets;
      } /* end-while */
      /* step 2: decide if a ~ b */
      while ( read in a and b ) 
          if ( Find(a) == Find(b) )
          	output( true );
          else   
          	output( false );
  }
  ```

- **Elements** of the sets : $1,2,3,\cdots,N$
- **Sets** : $S_1,S_2,\cdots\,and\,S_i\bigcap S_j=\emptyset\,(if\,i\neq j)$
- **Operations** :
  
  - Union( $i, j$ ) = Replace $S_i$ and $S_j$ by $S=S_i\bigcup S_j$
  - Find( $i$ ) = Find the set $S_k$ which contains the element $i$

---

### 5.3 Basic Data Structure

#### Union( $i, j$ )

- **Implementation 1** :

- **Implementation 2** :

  

  初始化全为0

#### Find( $i$ )

- **Implementation 1** :
- **Implementation 2** :

#### Analysis

---

### 5.4 Smart Union Algorithms

#### Union-by-Size

- Always change the smaller tree
- S[Root]
- **[Lemma]** 
- **Time complexity** of $N$ Union and $M$ Find operations is now $O(N+M\log_2N)$.

#### Union-by-Height

- Always change the shallow tree

  ```
  
  ```

---

### 5.5 Path Compression

```

```

- Note : 

---

### 5.6 Worst Case for Union-by-Rank and Path Compression

#### [Lemma] 

- Ackermann’s Function
  $$
  A(i,j)=
  $$
  
- $\alpha(M,N)=$



一共五种算法，注意看清题设

No smart union

Union by size

Union by height

Union by size + Union by rank

Union by height + Union by rank