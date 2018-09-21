# What is the difference between a stack versus a heap?

##### By Korel Hayrullah

##### [GitHub]( https://github.com/korelhayrullah)

##### [LinkedIn](https://www.linkedin.com/in/korel-chairoula-238882121)

In computers *stack* and *heap* data structues are used for memory management. The main difference between stack and heap is that stack is used for a static memory allocation and on the contrary, heap is for a dynamic memory allocation in which they both share the computer's RAM (Random Access Memory). Stack can be visualized as a linear memory and heap as a cloud of memory arrangement.

### Stack

The stack uses the LIFO (last in first out) data structure. There are two main operations of a stack which are: pop and push. The amount of stack's each memory block is fixed and cannot be expanded or minimized. However, the access time is very fast and the allocation and deallocation of memory is done automatically. Going deeper, the cost of popping and pushing is very cheap and its done by just adjusting the pointer of the stack, incrementing or decrementing, varies according to its implemention. Also, if we inspect the cost of its instruction cycle, there are ready machine instructions to do pop and push operation in once cycle and thanks to pipelining, in todays most computers in one cycle there can be made 3 pop or push instructions. Thinking of an example of the stack's usage, when a function in your code is called, the computer stacks its current position (its current programm address or return address), parameters and local variables, and jumps to the function's memory address. When the execution has finished, it returns from that function and pops the latest address from the stack in order to continue its job wherever it has been left. Also, invoking the stack takes O(N) time. This is a very simple example, behind the scenes for that example, there are more complicated operations made, but as an idea, these steps occur. 

### Heap

In contrast, the heap memory arrangement, is a distributed memory arrangement also known as dynamic memory. Whenever, a data needs to be stored, first it must find enough space from the heap memory to store the data via allocating the memory dynamically in which later on can be resized as needed instead of a fixed memory space like stacks. The acces time is slower and the cost is high, however, the invoking time complexity is O(1). If the data is no longer in need, the memory is deallocated and the occupied memory space is freed. As mentioned at the beginning, we can visualize the heap memory as a cloud or pool, that is beacuse the data is kept randomly in that space.  When a data is allocated and put in that memory a reference (pointer) is kept to that address in order to access it. All the allocation, deallocation and access is done manually and costs more than a stack's operations's instruction cycle.

### More about stack and heaps

Since the memory allocation of the stack is done automatically there is no need to worry about memory leaks. During the runtime of a function the data is there but as soon as the function returns there is not any information left about the function and its local variables. Whenever we want to allocate a memory space (generally it is known as the  `new` keyword in programming languages), a dynamic memory is allocated and kept in heap. Therefore, after its lifetime of that allocated memory space it needs to be freed in order to avoid memory leaks in which is the programmer's duty to deal with them. In some programming languages there are garbage collectors in which they free up the memory identifying the memory leaks. On the other hand, in general, the local variables and every other data that its size is known beforehands is kept in stack. 



*Benefitted from*

*<https://stackoverflow.com/questions/79923/what-and-where-are-the-stack-and-heap>*

*<https://techdifferences.com/difference-between-stack-and-heap.html>*

*<https://medium.com/fhinkel/confused-about-stack-and-heap-2cf3e6adb771>*
