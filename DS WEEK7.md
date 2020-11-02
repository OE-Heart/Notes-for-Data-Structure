# WEEK 7

## 5 The Disjoint Set

### 5.1 Equivalence Relations

#### [Definition] 

#### [Definition]

#### [Definition]

---

### 5.2 The Dynamic Equivalence Problem

- Given an equivalence relation ~, decide for any a and b if a ~ b

  ```pseudocode
  Algorithm:(Union / Find)
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

- **Elements**
- **Sets**
- **Operations** :
  - Union( i, j )
  - Find( i )\

---

### 5.3 Basic Data Structure

#### Union( i, j )

- **Implementation 1** :

- **Implementation 2** :

  

  初始化全为0

#### Find( i )

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