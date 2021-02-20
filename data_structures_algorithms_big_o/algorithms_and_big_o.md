# Algorithms and Big O

## Terms
- _Time complexity_ / _speed_ / _efficiency_ / _performance_
  - A count of the number of "steps" (elementary operations) required by an algorithm.
  - Big O is a measurement of time complexity.
  - Assume worst-case scenario, unless otherwise stated.
- _Constant time_
  - describes an algorith with a time complexity of _O(1)_
- _Linear time_
  - describes an algorithm with a time complexity of _O(N)_
- _Log time_
  - describes an algorithm with a time complexity of _O(log N)_
- _Quadratic time_
  - describes an algorithm with a time complexity of _O(N<sup>2</sup>)_

## _Big O_
- describes the efficiency of an algorithm.
- usually the worst-case scenario (unless otherwise stated).
  - eg, for an array: inserting the first element is worst-case (because every subsequent element must be shifted in memory).
- Pronounced "Oh of ...", eg: `O(N)` is "Oh of N"
- Constants are ignored
  - eg `O(100N)` and `O(N/2)` are both treated as `O(N)`
  
### Big O Notation

- _O(1)_
  - the number of steps is constant, regardless of the size of the dataset.
  - not necessarily "1 step". It could be 25k steps. But if the number of sets (whatever it is) does not change as the data size increases, then it's `O(1)`.
  - aka _constant time_
  - eg: reading from an array.
- _O(log N)_
  - The number of steps increases by one each time the data is doubled.
  - aka _log time_
  - eg: binary search of an array.
- _O(N)_
  - The number of steps required increases linearly with the number of elements.
  - aka _linear time_
  - eg: linear search of an array.
- _O(N<sup>2</sup>)_
  - The number of steps increases exponentially with the number of elements.
  - aka _quadratic time_
  - eg: bubble sort of an array.
  - any method with a doubly-nested loop will be in quadratic time.
  
## Sort Algorithms
- _Bubble Sort_: Compare 2 sequential items in an array. Move the larger to the right. Repeat over and over until the largest have 'bubbled' to the end.
  - Quadratic time
- _Selection Sort_
- _Insertion Sort_
- _Quicksort_

## Search Algorithms
- _Linear search_
  - iterate through every element and check if it's the target.
  - O(N) time complexity
    - (every new element must be checked, so number of steps increases linearly with the number of elements)
- _Binary search_
  - eliminate 50% of the elements in each step.
  - take the middle element, and ask: 'is this greater than, less than, or equal to my target?'
    - if equal: you've found it.
    - if greater than: choose all elements from lower bound up to the middle, and repeat.
    - if less than: choose all elements from middle to upper bound, and repeat.
  - `O(log N)`
