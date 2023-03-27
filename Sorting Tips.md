# Sorting: build from scratch (in-place)
## Bubble Sort: time $O(n^2)$ space $O(1)$
```python
def Bubble_sort(nums):
    nums_len = len(nums)
    for i in range(nums_len):
        for j in range(0, nums_len - 1 - i):
            if nums[j] > nums[j+1]:
                nums[j], nums[j+1] = nums[j+1], nums[j]
```

## Insertion Sort: time $O(n^2)$ space $O(1)$
 ```python
 def Insertion_sort(nums):
    nums_len = len(nums)
    for i in range(1, nums_len):
        tmp = nums[i]
        j = i - 1
        while j >= 0 and nums[j] > tmp:
            nums[j + 1] = nums[j]
            j -= 1
        nums[j+1] = tmp
 ```
 
 ## Selection Sort: time $O(n^2)$ space $O(1)$
 ```python
 def Selection_sort(nums):
    nums_len = len(nums)
    for i in range(nums_len):
        min_idx = i
        for j in range(i+1, nums_len):
            if nums[j] < nums[min_idx]:
                min_idx = j
        nums[min_idx], nums[i] = nums[i], nums[min_idx]
 ```
 
 ## Merge Sort: time $O(n\log n)$ space $O(n)$
 ```python
 def Merge_sort(nums):
    if len(nums) > 1:
        mid = len(nums)//2
        L = nums[:mid]
        R = nums[mid:]

        Merge_sort(L)
        Merge_sort(R)
        len_L, len_R = len(L), len(R)
        i = j = k = 0

        while i < len_L and j < len_R:
            if L[i] < R[j]:
                nums[k] = L[i]
                i += 1
            else:
                nums[k] = R[j]
                j += 1
            k += 1

        while i < len_L:
            nums[k] = L[i]
            i += 1
            k += 1

        while j < len_R:
            nums[k] = R[j]
            j += 1
            k += 1
 ```

## Quick Sort: time $O(n\log n)$ space $O(n\log n)$
```python
def quickSort(nums, low, high):
  def partition(nums_in, low_in, high_in):
      pivot = nums_in[high_in]
      i = low_in - 1
      for j in range(low_in, high_in):
        if nums_in[j] <= pivot:
          i = i + 1
          (nums_in[i], nums_in[j]) = (nums_in[j], nums_in[i])
      (nums_in[i + 1], nums_in[high_in]) = (nums_in[high_in], nums_in[i + 1])
      return i + 1
  if low < high:
    pi = partition(nums, low, high)
    quickSort(nums, low, pi - 1)
    quickSort(nums, pi + 1, high)
```

# Sorting: off-the-shelf implementations and tricks
## sort() VS sorted()
**sort()** modifies the list in-place and returns nothing. It is called as nums.sort(). On the other hand, **sorted()** creates a new list and does not modified the input list. It is called as nums2 = sorted(nums1). 

When the element of a list contains multiple domains, such as a list of lists, these elements are ordered by comparing each domain. For example,

```python
nums = [[2,3],[2,1],[1,9]]
nums.sort()
```

In the first domain of each element in this list, $1$ is ordered before $2$. When there is a tie, they are sorted by comparing the next unequal domain. Therefore, $[2,1]$ is placed before $[2,3]$. As a result, the sorted list is

```python
# nums: [[1, 9], [2, 1], [2, 3]]
```

Two paramters that can be passed to these two functions are **reverse** and **key**. **reverse** is set to **False** by default, which sorts the target list in ascending order. If set to **True**, the list is sorted in descending order.

Regarding the parameter **key**, it configures the sorting rule, which can be customized.

### lambda function
```python
A = [[2,'y',40],[1,'z',10],[3,'x',90]]
B = sorted(A, key = lambda x: x[1])
C = sorted(A, key = lambda x: x[2]/x[0])
```

List B is sorted according the second elements in list A in ascending order. List C is sorted by the ratio of the third and the first element in list A. Therefore, B and C are outputed as 

```python
# B: [[3, 'x', 90], [2, 'y', 40], [1, 'z', 10]]
# C: [[1, 'z', 10], [2, 'y', 40], [3, 'x', 90]]
```

### functools.cmp_to_key
```python
from functools import cmp_to_key

def my_func(x1,x2):
    if x1[0] < x2[0]:
        return -1
    elif x1[0] > x2[0]:
        return 1
    else:
        if x1[1] > x2[1]:
            return -1
        elif x1[1] < x2[1]:
            return 1
        else:
            if x1[2] < x2[2]:
                return -1
            elif x1[2] > x2[2]:
                return 1
            else:
                return 0

A = [[2,'y',40],[1,'z',10],[3,'x',90],[2,'a',100], [2,'y',1]]
D = sorted(A, key=cmp_to_key(my_func))
print(D)
```
 
Function **cmp_to_key** provides more freedom to design the comparison rule. In this case, **my_func()** specifies how two elements are compared in list A. If the domain is number, they are compared in ascending order, while if the domain is a letter, they are compared in descending order. Since **com_to_key** compares pairwise, we use $-1, 0,1$ to indicate that the first element (x1) is smaller than, equal to, or larger than the second element (x2). Therefore, the list D is 

```python
# D: [[1, 'z', 10], [2, 'y', 1], [2, 'y', 40], [2, 'a', 100], [3, 'x', 90]]
```
