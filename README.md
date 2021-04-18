 # Report-Markdown-Preview-Enhanced

[![hackmd-github-sync-badge](https://hackmd.io/nLmc8gAiQNCgLfrH7RHBbw/badge)](https://hackmd.io/nLmc8gAiQNCgLfrH7RHBbw)


Template for createing report PDF with Markdown Preview Enhanced 


## Prerequisite

- VScode
- Markdown-Preview-Enhanced
- Pandoc

## Notes

- Puppeet / Prince
    - not support MathJaX

---


# Reading Notes

# Heap: CH6


### How to acheive linear time complexity?

- 不用比的可以得到: $O(n)$
- 比較: $O(n\log n)$


## Sorting Comlexity 

![](https://i.imgur.com/d2LWkzP.png)


## Heaps

![](https://i.imgur.com/egnpKXr.png)

```
Parent(i)
    return [i/2]
Left(i)
    return 2i
Right(i)
    return 2i+1
```


### Heap operations

- 等比公式: $\frac{a(1-r^n)}{1-r}$



### Level / Height


![](https://i.imgur.com/ivFQRIr.png)


# Binary Search Trees: CH12


## Deletion


```cpp  
    
```

```cpp
TREE-DELETE(T,z)
    if z.left == NIL
        TRANSPLANT(T,z,z.right)
    else z.right == NIL
        TRANSPLANT(T,z,z.left)
    else 
        y = TREE-MINIMUM(z.right)
        if y.p != z
            TRANSPLANT(T, y, y.right)
            y.right = z.right
            y.right.p = y
        TRANSPLANT(T, z, y)
        y.left = z.left
        y.left.p = y
```