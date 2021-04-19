[![hackmd-github-sync-badge](https://hackmd.io/lQc0xN-7RsOa9Ux7Z9VVjQ/badge)](https://hackmd.io/lQc0xN-7RsOa9Ux7Z9VVjQ)


<center>

# Report - HW2

Shao-Ting Chiu
`r07945001@ntu.edu.tw`

</center>


## Problem 1



### 1. (10pts)

Because median is surely not one of the extemes, we can try $n-2$ times eliminiate the impossible i-of-median one by one. The remaining two $P$ will be the extremes. 

```cpp
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

- when `P[i] < P[j]`
    - `compare(i,j) == 1`
- Otherwise
    - `compare(i,j) == 2`
- because  $P_{ext}$ is always bigger or smaller than both of `P[i]` and `P[j]`


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

[^p-2]: https://stackoverflow.com/questions/19080059/algorithmsort-using-median-and-just-1-comparison-in-on-log-n






### 3. (10pts)

The insertion takes in two parts: 

1. Comparison to extrema
2. Binary search with extrema as a base number

Let another pancake be `P_new` with testiness `t_new`. And assume list `L` is sorted with extremes locate at `L[1]` and `L[end]`. We want to find a site `i` to insert  `P_new` such that `L[i]`

**Comparison to extrema**


```
Pancake-God-Oracle(L[1], L[end], P_new)
```


As described in [P1-2](#1-10pts), we can use extrema to build a **comparison function** from `Pancake-God-Oracle`. 
    

#### 想法


1. 如果在次大, 次小 之間, binary search
2. 如果比次大還大, 次小還小
    - 