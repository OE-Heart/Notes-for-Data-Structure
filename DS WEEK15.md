# WEEK 15

#### Quadratic Probing

- $F(i)$ is a quadratic function of $i$, such as $F(i)=i^2$.

##### [Theorem] If quadratic probing is used, and the table size is *prime*, then a new element can always be inserted if the table is *at least half empty*.

<img src="picture/image-20210104154133711.png" alt="image-20210104154133711" style="zoom:75%;" />

> Note : If the table size is a prime of the form $4k + 3$, then the quadratic probing  $f(i) = \pm i^2$ can probe the entire table.

```c
HashTable InitializeTable(int TableSize)
{
	HashTable H;
	int i;
	if(TableSize < MinTableSize)
	{
		Error("Table size too small");
		return NULL;
    }
	/*Allocate table*/
	H = malloc(sizeof(struct HashTbl));
	if(H == NULL)
		Fatal Error("Out of space!!!");
	H->TableSize = NextPrime(TableSize);
	
    /*Allocate array of Cells*/
	H->TheCells = malloc(sizeof(Cell)*H->TableSize);
	if(H->TheCells == NULL)
		FatalError("Out of space!!!");

    for(i = 0; i < H->TableSize; i++)
		H->TheCells[ i ].Info = Empty;
	return H;
}
```

```c
Position Find(ElementType Key, HashTable H) 
{   
	Position CurrentPos; 
    int CollisionNum; 
    CollisionNum = 0; 
    CurrentPos = Hash(Key, H->TableSize); 
    while(H->TheCells[ CurrentPos ].Info != Empty &&
          H->TheCells[ CurrentPos ].Element != Key) 
    { 
		CurrentPos += 2*++CollisionNum-1; 
		if (CurrentPos >= H->TableSize)  
            CurrentPos -= H->TableSize;   /*Faster than mod*/
    } 
    return CurrentPos; 
} 
```

```c
void Insert(ElementType Key, HashTable H) 
{ 
    Position Pos; 
    Pos = Find(Key, H); 
    if (H->TheCells[ Pos ].Info != Legitimate) 
    { /*OK to insert here*/ 
		H->TheCells[ Pos ].Info = Legitimate; 
		H->TheCells[ Pos ].Element = Key; /*Probably need strcpy*/ 
    } 
} 
```

> Note :
>
> - Insertion will be seriously slowed down if there are too many **deletions intermixed with insertions**.
> - Although primary clustering is solved, **secondary clustering** occurs, that is, keys that hash to the same position will probe the same alternative cells.

#### Double Hashing

- $f(i)=i*hash_2(x)$
- $hash_2(x)\not\equiv 0$
- make sure that all cells can be probed
- $hash_2(x)=R-(x\%R)$ with $R$ a prime smaller than TableSize, will work well.

> Note :
>
> - If double hashing is correctly implemented, simulations imply that the **expected** number of probes is almost the same as for a **random** collision resolution strategy.
> - Quadratic probing does not require the use of a second hash function and is thus likely to be **simpler and faster** in practice.

---

### 8.5 Rehashing

- Build another table that is about twice as big.
- Scan down the entire original hash table for non-deleted elements.
- Use a new function to hash those elements into the new table.
- When to rehash
  - As soon as the table is half full
  - When an insertion fails
  - When the table reaches a certain load factor

> Note : Usually there should have been N/2 insertions before rehash, so O(N) rehash only adds a constant cost to each insertion. However, in an interactive system, the unfortunate user whose insertion caused a rehash could see a slowdown.

```c
HashTable Rehash(HashTable H)
{
	int i, OldSize;
	Cell *OldCells;
	OldCells = H->TheCells;
	OldSize = H->TableSize;
	
    /*Get a new, empty table*/
	H = InitializeTable(2*OldSize);
	/*Scan through old table, reinserting into new*/
	for(i = 0; i < OldSize; i++)
		if(OldCells[i].Info == Legitimate)
			Insert(OldCells[i].Element, H);
	free(OldCells);
	
    return H;
}
```

---