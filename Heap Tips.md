## heapq.heapify(list)
Given a random list, it can be converted into a heap using this function. It runs in-place in linear time. The only heap in Python is min heap. To construct a max heap, all the elements should be mutiplied by $-1$.
```python
import heapq

nums1 = [5,3,4,1,2]
nums2 = [-1 * ele for ele in nums1]

heapq.heapify(nums1)
print(nums1)

heapq.heapify(nums2)
print(nums2)
```
The heapified lists are

```python
# nums1: [1, 2, 4, 3, 5]
# nums2: [-5, -3, -4, -1, -2]
```

They do not appear to be the expected order because heap is stored as a binary tree. We can pop each element to get the expected order, as will be discussed later.

## heapq.nlargest(n,list), heapq.nsmallest(n,list)
These two functions returns a list with $n$ largest/ smallest values in a random list. When $n=1$, they are equivalent to `max()` and `min()` but are not comparably efficient. When $n=len(list)$, they are equivalent to `sort()` but not comparably efficient.

```python
import heapq

nums = [5,3,4,1,2]
a = heapq.nsmallest(3,nums)
b = heapq.nlargest(3,nums)

print(a,b)

# a: [1,2,3]
# b: [5,4,3]
```

## heapq.heappush(list, ele), heapq.heappop(list)
They are conceptually identical to `push()` and `pop()` but maintain the sorted order of the list. Sometimes, we only want to peek the smallest element but not poping it. This can be done by consulting the first element, aka `list[0]`.

## heapq.heappushpop(list, ele), heapq.heapreplace(list, ele)
```python
import heapq

nums = [5,3,4,1,2]
heapq.heapify(nums)
# equivalent to heappush() + heappop(), but more efficient 
a = heapq.heappushpop(nums,-1)

nums = [5,3,4,1,2]
heapq.heapify(nums)
# equivalent to heappop() + heappush(), but more efficient
b = heapq.heapreplace(nums,-1)

print(a, b)

# a: -1
# b: 1
```

## heapq.merge(list1, list2, ...)
```python
import heapq

nums1 = [5,4,11,12,13]
nums2 = [15,14,1,2,3]

nums1.sort()
nums2.sort()

a = heapq.merge(nums1,nums2)

print(list(a))

# a: [1, 2, 3, 4, 5, 11, 12, 13, 14, 15]
```
Lists to be merged but be sorted.
