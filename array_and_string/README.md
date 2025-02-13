# Arrays and Strings

An array, at core, is a block of contiguous memory within a computer. The actual memory addresses in which data will be stored throughout the runtime of a program, are physically next to one another. Strings are merely a special case of array, which are given unique treatment and are often immutable.

To make programming faster for the human programmer, most high-level programming languages actually implement a *dynamic array*, also called an *array list*, a class which manages the underlying (static) array out-of-sight and out-of-mind. The python `list`, javascript `array`, and go `slice` are examples of this.

## Time complexity of array and string operations

| Operation                  | Array/List | String (Immutable) |
| :------------------------: | :--------: | :----------------: |
| Appending to end           | *O(1)*[^1] | *O(n)*             |
| Popping from end           | *O(1)*     | *O(n)*             |
| Insertion, not from end    | *O(n)*     | *O(n)*             |
| Deletion, not from end     | *O(n)*     | *O(n)*             |
| Modifying an element       | *O(1)*     | *O(n)*             |
| Random access              | *O(1)*     | *O(1)*             |
| Checking if element exists | *O(n)*     | *O(n)*             |

[^1]: Appending to the end of a list is [amortized O(1).](https://en.wikipedia.org/wiki/Amortized_analysis)