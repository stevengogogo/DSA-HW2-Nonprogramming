

<a href="https://hackmd.io/lQc0xN-7RsOa9Ux7Z9VVjQ"><img width=150 src=https://i.imgur.com/f4QaiGE.png></a>


<center>

# Homework 2: Non-Programming Part

邱紹庭
`r07945001@ntu.edu.tw`

</center>

**Table of Contents**

[toc]

---


## Problem 1 - Sort (80pt + 20pt)



### 1. (10pts)

Because median is surely not one of the extemes, we can try $n-2$ times eliminiate the impossible i-of-median one by one. The remaining two $P$ will be the extremes. 

```cpp=
Find-Extremes(P)
    Is = [1,2,3]
   
    for j = 4 to P.len+1
        i = PANCAKE-GOD-ORACLE(P,Is...)
        remove(Is[i])
        if j == P.len+1
            break
        append(Is, j)
    
    return Is
```

Therefore, totoal iteration is $n-1$ times $\in O(n)$

### 2. (20pts)


The main idea is how to convert `PANCAKE-GOD-ORACLE` into a **comparison function**, and use this comparison function to achieve the optimal of comparison sort.


**Making a comparison function**

Since we got the extremes, to say $P_{ext_1}$ and $P_{ext_2}$, we can use one of the $P_{ext}$ as a baseline. We need to prove the following equality

``` wrap=
compare(i,j) = i == PANCAKE-GOD-ORACLE(P[i], P[j], P_ext)
```

where $i,j\notin \{ P_{ext1}, P_{ext2} \}$

- when `P[i] < P[j]` and `P_ext` is maximum
    - `compare(i,j) == 1`
- Otherwise
    - `compare(i,j) == 2`
- because  $P_{ext}$ is always bigger or smaller than both of `P[i]` and `P[j]`
- For `P_ext` is a minimum, vice versa.
- We can not know `compare` is `argmin` or `argmax` function of `P[i]` and `P[j]`. But we can guarantee the monotonicity by using `compare` in quick sort.


**Apply to comparison sort**


Since we got the comparison function, we need to first allocate extremes to `P[1]` and `P[end]`. we can apply sorting algorithm, which is optimal in time complexity, to the sequence `P[2]...P[end-1]` (Noted that at this moment, $P[1]=P_{ext1}$, $P[end] = P_{ext2}$, $P[i] = P_k~if~k\in \{1,\cdots,end-1\}$). To acheive $O(n\lg n)$, I chose **quick sort** to the region with index `[2,...,end-1]` and use `compare` function proved above. For clarity, I listed the pseudo code below which is modified from *ITA P.171*:

```cpp=
Quicksort(A,p,r)
    if compare(p,r)
        q =Partition(A,p,r)
        Quicksort(A,p, q-1)
        Quicksort(A, q+1,r)
end 

Partition(A,p,r)
    x = A[r]
    i = p-1
    for j = p to r-1
        if compare(A[j],x) == 1
            i = i+1
            exchange A[j] with A[i]
    
    exchange A[r] with A[i+1]
    
    return i+1  
end

Sort(P)
    Is = Find-Extremes(P)
    allocate Is to P[1] and P[end]
    Quicksort(P,2,P.end-1)
end
```


**Query Complexity**

- For finding extrema: $O(n)$
- For sorting: It is implemented in quick sort that has the time complexity of $O(n\lg n)$. This complexity can be regarded the calling times of `compare` which uses the `PANCAKE-GOD-ORACLE`. Therefore, the overall query complexity is $n\lg n$.




**Reference**:
1. Algorithm: Sort using Median and Just 1 comparison in $O(n \log n)$. [Stackoverflow][^p-2]
2. Introduction to Algorithm. Page 171.

[^p-2]: [Algorithm Sort with median. StackOverflow](https://stackoverflow.com/questions/19080059/algorithmsort-using-median-and-just-1-comparison-in-on-log-n)






### 3. (10pts)

The insertion takes in two parts: 

1. Comparison to extrema
2. Binary search with extrema as a base number

Let another pancake be `P_new` with testiness `t_new`. And assume list `L` is sorted with extremes locate at `L[1]` and `L[end]`. We want to find a site `i` to insert  `P_new` such that `L[i]`

**Comparison to extrema**

```=
Pancake-God-Oracle(L[1], L[end], P_new)
```

- if `output=1`: Insert `P_new` at front
- else if `output=end`: Insert `P_new` at reat
- Else:
    - Use binary search to find location


**Binary Search**

As described in [P1-2](#1-10pts), we can use extrema to build a **comparison function** from `Pancake-God-Oracle`. After checking `P_new` is between `L[1]` and `L[end]`, we can use the comparison function to binary search which acheive $O(n\lg n)$ complexity.


```python=
Insert(L, P_new)
    i = Pancake-God-Oracle(L[1],L[end],P_new)
    if i==1
        insert P_new at 1
    elseif i==end
        insert P_new at end
    else
        I = Binary-Search(L[2:end],P_new)
        insert P_new at I+1
end

compare(i,j)
    I = Pancake-God-Oracle(i,j, L[1])
    if I==i
        return 1 # i>j
    else 
        return 2 
end

Binary-Search(L,P_new)
    high = L.end
    low = 1
    value ans;
    while (high>=low)
        mid = (high + low)/2
        if compare(L[mid], P_new) == 1
            high = mid - 1
        else
            ans = mid
            low = mid + 1
        end
    end
    
    return ans
end
```


The reason to use bianry search:

1. $L$ is sorted
2. `P_new` is in the middle of the extremas.


### 4. (5pts)



---

## Problem 2 - Tree (65pt)


### 1. (10pt)

**💡Idea**

- Because $k-1 < k$, $t_{k-1}$ is on the left side of $t_k$ 
- On the other hand, $t_{k-1}$ possesses largest index on the left side of $t_{k}$
- Therefore, one can use the policy to find $t_{k-1}$ from $t_k$
    > Turn left, and keep going right until reaching a NIL

**🔧Implementataion**

```cpp=
function findPrev(t_k)
    
    if(t_k.left == NIL)
        return NIL
    end
    
    t_prev = t_k.left
    
    while(t_prev.right != NIL)
        t_prev = t_prev.right
    end
    
    return  t_prev
end
```


### 2. (15pt)



Because avery sub-tree of $T$ is a binary search tree, and left sub-sub-tree contains data earlier than the sub-tree's root in time. That is, for the set $\{t_i | i\in left~side~of~t_k\}$ fullfills the inequality $t_{i} < t_{k}$.

$\textbf{t_1 < t_2 < ...} < t_k < \cdots < t_{n-1} < t_n$

where the set of left nodes is in **bold** font. We can first proof that we can find the previous node of $t_{1}$ that is an empty set (`NIL`). 

For finding $t_{k-1}$, there is no other value between $t_{k-1}$ and $t_{k}$ that is smaller than $t_k$. Therefore, we need to find the biggies value in the set $\{t_i | i\in left~side~of~t_k\}$. By keep reaching the **right leaf**, we can get the biggest value of $\{t_i | i\in left~side~of~t_k\}$. That is $t_{k-1}$.


### 3. (10pt)

<center>
<img width=300 src="https://i.imgur.com/k3pNorJ.jpg">

**Fig. 2-1.** Reconstructed binary tree[^lc:BT].
</center>


[^lc:BT]:[LeetCode: Binary Tree Traversal](https://leetcode.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/solution/)


### 4. (15pt)


**Definition of root element**

The definition of a root is always the **first** element of the **preorder traversal**. Thus, the determination of a root is unique, which is `inorder[1]`.

**Definition of leaves**

With a given root/subroot, the definition of left and right sets of leaves is unique (**Fig. 2-2**). Recursively, all the set of leaves will be uniquely distributed with complete inorder and preorder traversal.

**Conclusion**

Ther is no two binary trees share with the same `(inorder, preorder)` pair.

<center>
<img width=300 src="https://i.imgur.com/J4m4hog.jpg">

**Fig. 2-2.** Reucrsive algoritm of reconstructing a binary tree with preorder and inorder traversal 
[^Tutorial:BST-Construct].
</center>


### 5. (15pt)

**💡Idea**
- Use recursive method described in **Fig. 2-2**.
- Use a look-table (**Fig. 2-3**) to record the **preorder index** and **inorder index** in respect of a **key** which is the alphabet in **Fig. 2-2**. $O(n)$ time complexity is required for this process including screening these two arrays.
- **A recursive method** to build a binary tree. By intuition, the **worst case** of constructing a binary tree with a **left-connected linked list** that requires $O(n)$ time complexity



<center>
<img width=300 src="https://i.imgur.com/GGDW9tN.jpg">

**Fig. 2-3.** Data structure for storing the indexes of **inorder** and **postorder** array.
</center>

**🔧Implementation**


- Constructing a look-up array
    ```cpp=
    struct node
        iIN  //index in inorder array
        iPRE //index in preorder array
        left //leaves
        right
    end

    function getPairedArr(inorder, preorder)

        node arr[length(inorder)];

        for i = 1 to length(inorder)
            arr[inorder[i]].iIN = i
        end

        for i = 1 to length(preorder)
            arr[preorder[i]].iPRE = i
        end

        return arr
    end
    ```
    
- Reconstructing a binary tree


```cpp= 
buildBST(inorder, preorder, iIN)
    map = getPairedArr(inorder, preorder)
    iNA = 1
    root= _buildBst(map, inorder, preorder, &iNA)
    return root
end


_buildBST(map, inArr, preArr, *iNA)
    
    //Reach end
    if (length(inArr)==0 || *iNA > length(preArr))
        return NULL
    end
    
    //Asign new root
    root = map[preArr[*iNA]]
    
    Iin = map[preArr[*iNA]].iIN
    
    *iNA++
    
    root.left = _buildBst(map, inArr[1:Iin-1], preArr, iNA)
    root.right = _buildBst(map, inArr[Iin+1:end], preArr, iNA)
    
    
    return root
end

```
Note: 
1. This framework is majorly inspired by [^Tutorial:BST-Construct] and [^leet:BSTrecursive]
2. In Julia: `a[3:end] == []` when `length(a) == 2`[^juliaArr]


 [^leet:BSTrecursive]: [BST recursive](https://leetcode.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/discuss/1204516/recursive)


---


## Problem 3 - Heap (55pt)

### 1. (15pts)

**💡Idea**

- **Direction of change**: First of all, if `x.key == v`, there is no need to move the element `x` We need to know the new assigned value `v` is greater or smaller. 
- **Case 1: `x.key` is increased**:   
    - There is only one possibility: going downward to leaf.
- **Case 2: `x.key` is decreased**:
    - Going upward to root.

Both *Case 1* and *Case 2* can be recursively (or loop) implemented. The only difference is the direction of movement.

<center>
<img width=300 src="https://i.imgur.com/gc9HtiV.jpg">

**Fig 3-1.** Moving direction of node `x`.
</center>


**🔧Implementation**

```cpp=
h.modify(x, v){
    if (x.key==v)
      //Do nothing
    else if(x.key>v)
        h.increaseKey(x,v)
    else
        h.decreaseKey(x,v)
}


h.decreaseKey(x,v){
    assert(x.key > v)
    x.key = v
    while(parent(x) != NULL and parent(x).key > v){
        swap( parent(x), x) 
    }
}


h.increaseKey(x,y){
    assert(x.key < v)
    x.key = v
    
    while( not all of x.leaves[:] is NULL ){
        //Get the node with minimum key
        min_node = argmin(x.leaves[:]) 
        if(min_node.key < x.key){
            swap(min_node, x)
        }
    }
    
}
```

**Description**

- `h.decreaseKey`: For going upward, there is only one neighbor. That is `parent`
- `h.increaseKey`: Going downward is more complicated. We need to choose the direction in the following conditions:
    - **NUll end**: Terminate here
    - **One NULL end**: Choose the node with key
    - **Both leaves have key**: Choose the `minimum` to swap

**Notes**

1. I use `x.leaves[:]` is for generosity, that means there can be leaves more than two like **d-ary heap**
2. The `swap` function switches two variables. 


### 2. (10pt)

**Definition**

`u`: undefine variable

#### (a)


|N\M|0|1|2|3|
|---|---|---|---|---|
|0|u|u|u|u|
|1|u|u|u|u|
|2|u|u|u|1|
|3|4|u|u|2|

```!
D.add(2, 3, 1); D.add(3, 3, 2); D.add(3, 0, 4); ShowState();
```


##### (b)

|N\M|0|1|2|3|
|---|---|---|---|---|
|0|u|u|u|u|
|1|u|u|u|u|
|2|u|u|u|1|
|3|4|u|u|u|

```!
 D.extractMinRow(3) ; ShowState();
```


##### `(c)`

|N\M|0|1|2|3|
|---|---|---|---|---|
|0|u|u|u|u|
|1|u|u|u|3|
|2|u|u|u|1|
|3|4|u|u|u|

```c!
D.add(1, 3, 3); ShowState();
```


##### (d)

|N\M|0|1|2|3|
|---|---|---|---|---|
|0|u|u|u|u|
|1|u|u|u|3|
|2|u|u|u|u|
|3|4|u|u|u|

```c!
D.delete(2, 3); ShowState();
```

##### (e)

|N\M|0|1|2|3|
|---|---|---|---|---|
|0|u|u|u|u|
|1|u|u|u|u|
|2|u|u|u|u|
|3|4|u|u|u|

```c!
D.extractMinCol(3 ); ShowState();
```

### 3.(10pt)

**💡Idea**

- **$\log(NM)$ is the sum of two heaps' operation**
    - Since all of operations mentioned `O(log(nm))` time compleixty, it is likey the result of **two-heap-operation**, which causes `O(log(m) + log(n))` operation of a paired heap
- How to know the **availability** of an item?
    - For each element $A_{i,j}$, we need to labelled the availability by using `struct` or build another one-to-one map to store the data of availability. The status of an item can be:
        1. Undefined
        2. Available
        3. Extracted


- Each column/row requires a heap to keep the track on the minimum. There will be $M+N$ heaps. 
- Each node should keep track on its location `i` and `j`. To 


<center>
<img width=300 src="https://i.imgur.com/kgjrWPg.jpg">

**Fig 3-2.** 2D array with two array of heaps.
</center>


**🔧Implementation: Data Structure**

1. Node $A_{i,j}$ 
    ```cpp=
    struct node{
        key
        i
        j
        iHr // location at row heap
        iHc //location at col heap
        avail//availability
    }
    ```
2. Heap
    ```cpp=
    struct heap{
        node* array[N] //M
        len //[0,N]/[0.M]
    }
    ```
3. Data Structure `D`
    ```cpp=
    struct D{
        heap Hc[M]
        heap Hr[N]
        node A[N,M]
    }
    ```


**🔧Implementation: Operations**



1. `D.add(i, j, v)`
    ```cpp=
    function add(d::D, i,j,v)
        //Register A[i,j]
        d.A[i,j].avail = true
        d.A[i,j].i = i
        d.A[i,j].j = j
        d.A[i,j].key = v

        insertHeap(d.Hr[i], &d.A[i,j])
        insertHeap(d.Hc[j], &d.A[i,j]) 
    end
    ```
    
    We have to extract all unavailable values until find the reachable minimum.

2. `D.extractMinRow(i)`

    ```cpp=
    function extractMinRow(d::D, r)

        d.Hr[0].avail = false
        extractMinHeap(d.Hr[r])
        deleteHeap(d.Hc, d.Hr[0].iHc) //heap deletion

    end
    ```
    Similar to `D.extractMinCol`


3. `D.extractMinCol(i)`

    ```cpp=
    function extractMinRow(d::D, c)

        d.Hc[0].avail = false
        extractMinHeap(d.Hc[c])
        deleteHeap(d.Hr, d.Hc[0].iHr) //heap deletion

    end
    ```

4. `D.delete(i, j)`

    ```cpp=
    function delete(d::D, i, j)
       d.A[i,j].avail = false
       deleteHeap(d.Hr, d.A[i,j].iHr)
       deleteHeap(d.Hc, d.A[i,j].iHc)
    end
    ```



### 4. (20pt)

In **Fig. 3-2**, $M+N$ heaps have been estbalished for tracking the minimum value of columns and rows. 

#### Add
We need two heap insertions to track the minimum values of column `j` and row `i`. Therefore, the time complexity is `O(log M + log N) = O(log MN)`


#### Extract

The extraction on either `col` or `row` requires a following heap deletion. Both of them requires `O(log(h))`. Therefore, the sum of complexity is `O(log N + log M)`


#### Deletion

We need to delete the same item in two heaps. Use the `iHc` and `iHr` to keep track on the location. The deletion can be done by simply set `d.Hrp[iHc] = -Inf` with heapify and extraction. All of them requires logarithmic time complexity on each side. In summary, the total time complexity is `O(log N + log M)`








[^Tutorial:BST-Construct]: [ Construction of unique Binary tree. Book chapter](https://www.google.com/url?sa=t&rct=j&q=&esrc=s&source=web&cd=&ved=2ahUKEwiIzf_Av8bwAhUvy4sBHem3BCgQFjABegQIBBAD&url=https%3A%2F%2Fs3-eu-west-1.amazonaws.com%2Fpfigshare-u-files%2F1800958%2FConstructionofuniqueBinarytree.pdf&usg=AOvVaw23KzE0wgqxMB83ms4ZTp6Y)


 [^juliaArr]: ```julia
    julia> a = [1,2,3,4]
    4-element Array{Int64,1}:
    1
    2
    3
    4

    julia> a[5:end]
    Int64[]
    ```