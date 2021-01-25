# Data Structures, Algorithms, and Big O

## Questions

## Big O
- constant time vs linear time


## Terms
- _Array_
  - A list of elements located next to each other in memory.
- _Operations_
  - The four basic actions you can perform on a data structure.
  1) _Read_: get a value by index.
   - O(1) for an array: because:
     - A computer can read data from a specific memory address in a single step.
     - The memory address of any element in an array can be inferred from the position of the 0th element (stored in array's metadata), and the index of the targeted element.
     - So reading data from an array has a time complexity of O(1) (it always takes 1 step).
  2)_Search_: check whether a value exists.
  3)_Insert_: add an element.
  4)_Delete_: remove an element.
- _Linear search_: check every element in an array to see if it matches target.
- _Binary search_: 
- _Big O_
  - describes the efficiency of an algorithm.
  - usually the worst-case scenario (unless otherwise stated).
    - eg, for an array: inserting the first element is worst-case (because every subsequent element must be shifted in memory).
  - _O(1)_
    - the number of steps is constant, regardless of the size of the dataset.
    - not necessarily "1 step". It could be 25k steps. But if the number of sets (whatever it is) does not change as the data size increases, then it's `O(1)`.
  - _O(log N)_
  - _O(N)_
    - The number of steps required increases linearly with the number of elements.
  - usually worst-case estimate
  - binary search vs linear search
- _Time complexity_ / _speed_ / _efficiency_ / _performance_
  - A count of the number of "steps" (elementary operations) required by an algorithm.
  - Big O is a measurement of time complexity.
- _Constant time_
- _Linear time_
- _Log time_
- _Binary search_
- _Linear search_

## Sources
- [A Common-Sense Guide to Data Structures and Algorithms](https://pragprog.com/titles/jwdsal2/a-common-sense-guide-to-data-structures-and-algorithms-second-edition/)
