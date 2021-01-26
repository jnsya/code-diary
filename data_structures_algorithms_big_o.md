# Data Structures, Algorithms, and Big O

## Questions

## Terms
- _Array_
  - A list of elements located next to each other in memory.
- _Operations_
  - The four basic actions you can perform on a data structure: read, search, insert, delete.
  - Here's an overview of the time complexity for each operation when performed on an array:
  1)_Read_: get a value by index.
   - O(1) for an array: because:
     - A computer can read data from a specific memory address in a single step.
     - The memory address of any element in an array can be inferred from the position of the 0th element (stored in array's metadata), and the index of the targeted element.
     - So reading data from an array has a time complexity of O(1) (it always takes 1 step).
  2)_Search_: check whether a value exists.
  3)_Insert_: add an element.
  4)_Delete_: remove an element.
- _Time complexity_ / _speed_ / _efficiency_ / _performance_
  - A count of the number of "steps" (elementary operations) required by an algorithm.
  - Big O is a measurement of time complexity.
  - Assume worst-case scenario, unless otherwise stated.
- _Constant time_
  - describes an algorith with a time complexity of _O(1)_
- _Linear time_
  - describes an algorithm with a time complexity of _O(n)_
- _Log time_
  - describes an algorithm with a time complexity of _O(log N)_

## _Big O_
- describes the efficiency of an algorithm.
- usually the worst-case scenario (unless otherwise stated).
  - eg, for an array: inserting the first element is worst-case (because every subsequent element must be shifted in memory).

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
  
## Sort Algorithms
- _bubble sort_

## Search Algorithms
- _Binary search_
  - eliminate 50% of the elements in each step.
  - take the middle element, and ask: 'is this greater than, less than, or equal to my target?'
    - if equal: you've found it.
    - if greater than: choose all elements from lower bound up to the middle, and repeat.
    - if less than: choose all elements from middle to upper bound, and repeat.
    - 
- _Linear search_
  - iterate through every element and check if it's the target.
  - O(n) time complexity
    - (every new element must be checked, so number of steps increases linearly with the number of elements)

## Sources
- [A Common-Sense Guide to Data Structures and Algorithms](https://pragprog.com/titles/jwdsal2/a-common-sense-guide-to-data-structures-and-algorithms-second-edition/)
