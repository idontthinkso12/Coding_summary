# Sorting: from scratch (in-place)
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
