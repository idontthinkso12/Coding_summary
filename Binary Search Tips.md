# Type 1: to check whether the given list contains the target number
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

# Type 2: find the index of the last number that is no larger than the target number
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
