# Data Structures

## Questions
- We count "the number of steps", but we're assuming that any kind of step takes as long as any other step. What about if one kind of step takes way longer than another - wouldn't that affect time complexity?
  - eg: in bubble sort there are 2 kinds of step: comparing 2 values, and swapping 2 values. Both count towards the "total number of steps" Isn't it possible that swapping the values takes way longer than comparing?

- In Ruby, we have methods for accessing arrays as if they were stacks or queues (ie, pop push etc). But do we still gain the performance benefits of using stacks and queues if we treat an array as if it were a stack or queue?
  - Wait: are there any performance benefits to using stacks or queues?

- Why doesn't Ruby have in-built data structures for linked lists as it does for hashes and arrays?
  - isn't it just really annoying to literally implement a linked list every time you need to use one? Is it common in web development to use them at all? (I can imagine putting a generic shared implementation in eg the lib/ directory).

- The categories for data structures seem blurry. ie, an array and a linked list are both lists, and both could be used to implement a stack or a queue. So is a queue a more abstract form of data structure than an array? Is there some name for this category of "more abstract" data structures?
  
- How does the call stack work, exactly? When do methods get added to the stack, and when do they get removed?
  - What is a _stack overflow_?
  
## Data Structures
- _Array_
  - A list of elements located next to each other in memory.
- _Stack_
  - An array, but with 3 additional restrictions:
    - Pop: can only remove final element
    - Push: can only add final element
    - Peek: can only read final element
  - LIFO: Last in first out
  - Top: the end of a stack is called the top
  - Useful for: implementing 'undo' function; 
  - Analogy: it's like a can of pringles.
- _Queue_
  - An array, but with 3 additional restrictions:
    - Add: can only add the final element (same as Stacks)
    - Remove: can only remove the FIRST element (opposite of stack)
    - Read: can only read the FIRST element (opposite of a stack)
  - FIFO: First in first out
- _Hash table_
  - aka a hash in Ruby.
  - When inserting, the key is hashed to produce the memory location where the value will be stored.
    - eg: imagine you want to insert the key/value pair `{ "bad": "evil" }` into a hash.
      - The key "bad" is hashed to produce an integer. This integer represents the location in memory where the value "evil" will be stored. The value is then written at that location.
      - To lookup the value of "bad": the word "bad" is hashed (with the same algorithm as used on insertion) to produce an integer representing the memory location where that key's value is stored. The value is then read from that memory location.

  - the name 'hash' derives from the hashing function used to map keys to memory location.
  - Search (aka lookup) has a time complexity of O(1).
  - Insertion has a time complexity of O(1).

## Operations
- The four basic actions you can perform on a data structure: read, search, insert, delete.
  - On a hash table, you can only perform search, insert and delete (not read).
1)_Read_: get a value by index.
  - O(1) for an array: because:
    - A computer can read data from a specific memory address in a single step.
    - The memory address of any element in an array can be inferred from the position of the 0th element (stored in array's metadata), and the index of the targeted element.
    - So reading data from an array has a time complexity of O(1) (it always takes 1 step).
  - Not possible for a hash.
2)_Search_: check whether a value exists.
  - Time complexity for an array depends on which search algorithm you use - see Algorithms.
  - O(1) for a hash.
3)_Insert_: add an element.
  - O(N) for an array. (In the worst-case scenario: the first element. Because every subsequent element must be shifted in memory.)
  - O(1) for a hash.
4)_Delete_: remove an element.
  - O(N) for an array. (In the worst-case scenario: the first element. Because every subsequent element must be shifted in memory.)
  - O(1) for a hash.
  
## Linked Lists
- Like an array, a linked list stores a list of elements.
- Unlike an array, the elements are not located next to each other in memory.
- In general, the advantage of a linked list is that k

- Linked lists can do _sequential access_, but not _random access_.
  - _Sequential access_: running through the list of elements to find a given value.
  - _Random access_: directly accessing a value based on its memory location.
  
## Sources
- [A Common-Sense Guide to Data Structures and Algorithms](https://pragprog.com/titles/jwdsal2/a-common-sense-guide-to-data-structures-and-algorithms-second-edition/)
