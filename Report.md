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
        i, v = PANCAKE-GOD-ORACLE(P,Is...)
        remove(Is[i])
        if j == P.len+1
            break
        append(Is, j)
    
    return Is
```

Therefore, totoal iteration is $n-1$ times $\in O(n)$

### 2. (20pts)


