# WEEK 14

## 8 Hashing

### 8.1 General Idea

<img src="picture/image-20210102212103035.png" alt="image-20210102212103035" style="zoom: 75%;" />

#### Symbol Table ADT

- **Objects** : A set of name-attribute pairs, where the names are unique
- **Operations** :
  -  SymTab Create(TableSize) 
  -  Boolean IsIn(symtab, name)
  -  Attribute  Find(symtab, name) 
  -  SymTab  Insert(symtab, name, attr)
  -  SymTab  Delete(symtab, name) 

#### Hash Tables

<img src="picture/image-20210102212608844.png" alt="image-20210102212608844" style="zoom:75%;" />

- A **collision** occurs when we hash two nonidentical identifiers into the same bucket.
- An **overflow** occurs when we hash a new identifier into a full bucket.

---

### 8.2 Hash Function

-  $f(x)$ must be **easy** to compute and **minimize** the number of **collisions**.
-  $f(x)$ should be unbiased. For any $x$ and any $i$, we have that $Probability(f(x)=i)=\frac{1}{b}$. Such kind of a hash function is called a **uniform hash function**.

---

### 8.3 Separate Chaining

- keep a list of all keys that hash to the same value

```c
struct ListNode; 
typedef struct ListNode *Position; 
struct HashTbl; 
typedef struct HashTbl *HashTable; 
struct ListNode { 
	ElementType Element; 
	Position Next; 
}; 
typedef Position List; 
/* List *TheList will be an array of lists, allocated later */ 
/* The lists use headers (for simplicity), */ 
/* though this wastes space */ 
struct HashTbl { 
	int TableSize; 
	List *TheLists; 
}; 
```

#### Create an empty table

```c
HashTable InitializeTable( int TableSize ) 
{   
    HashTable H; 
    int i; 
    if ( TableSize < MinTableSize ) 
    { 
	    Error( "Table size too small" );  
        return NULL;  
    } 
    H = malloc( sizeof( struct HashTbl ) );  /*Allocate table*/
    if ( H == NULL ) FatalError( "Out of space!!!" ); 
    H->TableSize = NextPrime( TableSize );  /*Better be prime*/
    H->TheLists = malloc( sizeof( List )* H->TableSize );  /*Array of lists*/
    if ( H->TheLists == NULL ) FatalError( "Out of space!!!" );
    H->TheList = malloc(H->TableSize*sizeof(struct ListNode));
    for( i = 0; i < H->TableSize; i++ ) 
    {   /*Allocate list headers*/
		//H->TheLists[ i ] = malloc( sizeof( struct ListNode ) ); /* Slow! */
		if ( H->TheLists[ i ] == NULL ) FatalError( "Out of space!!!" ); 
		else H->TheLists[ i ]->Next = NULL;
    } 
    return H; 
} 
```

#### Find a key from a hash table

```c
Position Find ( ElementType Key, HashTable H ) 
{ 
    Position P; 
    List L; 
    L = H->TheLists[ Hash( Key, H->TableSize ) ]; 
    P = L->Next; 
    while( P != NULL && P->Element != Key )  /*Probably need strcmp*/ 
		P = P->Next; 
    return P; 
} 
```

#### Insert a key into a hash table

```c
void Insert ( ElementType Key, HashTable H ) 
{ 
    Position Pos, NewCell; 
    List L; 
    Pos = Find( Key, H ); 
    if ( Pos == NULL ) 
    {   /*Key is not found, then insert*/
		NewCell = malloc( sizeof( struct ListNode ) ); 
		if ( NewCell == NULL ) FatalError( "Out of space!!!" ); 
		else 
		{ 
	     	L = H->TheLists[ Hash( Key, H->TableSize ) ]; /*Compute again is bad*/
	     	NewCell->Next = L->Next; 
	     	NewCell->Element = Key; /*Probably need strcpy!*/ 
	     	L->Next = NewCell; 
		} 
    } 
} 
```

> Note : Make the TableSize about as large as the number of keys expected (i.e. to make the loading density factor $\lambda\approx$1).

---

### 8.4 Open Addressing

- find another empty cell to solve collision(avoiding pointers)

```pseudocode
Algorithm: insert key into an array of hash table
{
    index = hash(key);
    initialize i = 0 ------ the counter of probing;
    while (collision at index) 
    {
		index = (hash(key)+f(i))%TableSize; /*f(i) is collision resolving function*/
		if (table is full) break;
		else i++;
    }
    if (table is full) ERROR (“No space left”);
    else insert key at index;
}
```

> Note : Generally $\lambda<0.5$.

#### Linear Probing

- $F(i)$ is a linear function of $i$, such as $F(i)=i$.
- 逐个探测每个单元(必要时可以绕回)以查找出一个空单元
- 使用线性探测的预期探测次数对于插入和不成功的查找来说大约是$\frac{1}{2}(1+\frac{1}{(1-\lambda)^2})$，对于成功的查找来说是$\frac{1}{2}(1+\frac{1}{1-\lambda})$
- Cause **primary clustering** : any key that hashes into the cluster will add to the cluster after several attempts to resolve the collision.