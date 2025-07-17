## 1. ArrayList VS LinkedList
Both ArrayList and LinkedList are implementations of the List interface in the Java Collections Framework, but they differ significantly in their underlying data structures and performance characteristics. 

1. Underlying data structure

* ArrayList: Uses a dynamic array, storing elements in contiguous memory.

* LinkedList: Uses a doubly linked list, where each node points to the next and previous elements; elements are not necessarily contiguous. 



|Operation                                       |	    ArrayList                               |LinkedList               |
------------------------------------------------ |----------------------------------------------|-------------------------|
|Accessing elements by index (e.g., get(index))	 | O(1).                                        |   O(n)                  |
|Adding/removing elements at the beginning or end|	O(n) at beginning, O(1) (amortized) at end	|O(1).                    |
|Adding/removing elements in the middle	         |O(n)                                          |O(n) (requires traversal)|
|Iterating through elements	                     |Faster	                                    |Slower.                  |
|Memory consumption                              |	Generally lower, storing only elements. Can have wasted space if not full. Usually less than LinkedList.                                                             |	Higher, each node stores data and references.|


## 2. functional interface or Single Abstract Method in the interface. 

A functional interface in Java is an interface that contains exactly one abstract method. This single abstract method is also known as the "functional method" or "Single Abstract Method (SAM)." 

The defining characteristic is the presence of only one abstract method. This is crucial as it allows functional interfaces to serve as target types for lambda expressions and method references, which provide concise implementations for that single abstract method.

