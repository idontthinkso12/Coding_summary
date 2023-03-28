# Binary Search
## Type 1: to check whether the given list contains the target number
```python
def find(list_in, target):
    # init l and r pointers
    l, r = 0, len(list_in)
    
    # corner cases
    if target < list_in[l] or target > list_in[r-1]:
        return False
    
    # iteration
    while l <= r:
        # no overflow
        mid = l + (r-l) // 2
        
        if list_in[mid] == target:
            return True
        elif list_in[mid] > target:
            r = mid - 1
        else:
            l = mid + 1
            
    return False
```

## Type 2: find the index of the last number that is no larger than the target number
```python
def find(list_in, target):
    # init l and r pointers
    l, r = 0, len(list_in) -1 
    
    # corner cases
    if target < list_in[l]:
        return -1
    if target > list_in[r]:
        return r
    
    # iteration
    while l <= r:
    
        # no overflow
        mid = l + (r-l) // 2
        
        # if equal, we check the right part
        if list_in[mid] <= target:
            res = mid
            l = mid + 1
        else:
            r = mid - 1 

  return res
```

# Bisection Algorithm
## bisect.bisect_left(list, ele, lo, hi), bisect.bisect_right(list, ele, lo, hi)
```python
import bisect

#   idx 0 1 2 3 4 5 6
nums = [0,1,3,3,3,5,6]

a = bisect.bisect_left(nums, 3)
b = bisect.bisect_right(nums, 3)
c = bisect.bisect(nums, 3)

print(a, b, c)

# a: 2
# b: 5
# b: 5

# domain: [3,3,3]
d = bisect.bisect_left(nums,-1,2,5)
e = bisect.bisect_right(nums,7,2,5)

print(d, e)

# d: 2
# e: 5
```
In both functions, `lo` and `hi` are set to $0$ and $len(list)$ by default, thus the effective domain is the whole list. If these two parameters are specified, the effective domain is `list[lo : hi]`, thus only `list[lo]` is inclusive. `bisect_left` returns the and index `idx` where all the elements left to `idx` is smaller and all the elements right to `idx` is no smaller. `bisect_right` is equivalent to `bisect`. It returns an index `idx` where all the elements left to `idx` is no larger and all the elements right to `idx` is larger.

## bisect.insort_left(list, ele, lo, hi), bisect.insort_right(list, ele, lo, hi)
```python
import bisect

#   idx 0 1 2 3 4 5 6
nums = [0,1,3,3,3,5,6]
bisect.insort_left(nums,3)
print(nums)
# [0, 1, 3, 3, 3, 3, 5, 6]

nums = [0,1,3,3,3,5,6]
bisect.insort_right(nums,3)
print(nums)
# [0, 1, 3, 3, 3, 3, 5, 6]

nums = [0,1,3,3,3,5,6]
bisect.insort_left(nums,-1, 2, 5)
print(nums)
# [0, 1, -1, 3, 3, 3, 5, 6]

nums = [0,1,3,3,3,5,6]
bisect.insort_left(nums,7, 2, 5)
print(nums)
# [0, 1, 3, 3, 3, 7, 5, 6]
```
Function `insort_left()` is equivalent to `bisect_left()` plus `insert()` and `insort_right()` equals correspondingly. In addition, `insort_right()` is the same as `insort()`. They are alias. 
