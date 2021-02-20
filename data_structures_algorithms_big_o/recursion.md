# Recursion

## Terms
- _base case_: the branch of a recursive method which doesn't use recursion (see example below).
- Steps for understanding a recursive function:
  1. Identify the base case.
  2. Walk through the function as it deals with the base case.
  3. Identify the step immediately before the base case.
  4. Walk through the function as it deals with this step. 
  5. Repeat.

- Recursion can allow simpler code in situations where you need to go arbitrarily levels deep. 
  - eg: printing subdirectories of a given directory. Without recursion, you'd need to repeatedly rewrite the same code to access each level of subdirectory.


## Example recursive function
This Ruby method calculates the factorial of a number. (The factorial of 5 is 5 * 4 * 3 * 2 * 1).

```ruby
def factorial(number)
  ## Base case is if the input is 1
  if number == 1
    return 1
  else
    return number * factorial(number - 1)
  end
end
```

## Quicksort
- _partition_
- _pivot_
- _left pointer_
- _right pointer_

- Divide the array into sub-arrays based on a pivot (a random element).
- After the partition, everything to the left of the pivot will be smaller than the pivot.
- Everything to the right of the pivot will be larger.
- Then, treat each sub-array (left and right of the pivot, not including the pivot) as a separate array, and partition each of them.
- Keep going until the base case: the array contains 0 or only 1 element - and thus is already sorted.
