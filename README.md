---
Peter Feghali @ Fall 2018
CS32 Final Study Guide
---
#### Introduction:
This is an incomplete final study guide.
There is def material missing from here, albeit most should be faily valid. 
# Compiling Review
Compiling is the process of taking your cpp code and generating an executable.
## C++ Build Process
The build process is made up of three distinct parts. Preprocessing, compiling, and linking.
	PP is code verification, type checking, and other considerations
	Compiling taked the code and generates object files
	Linking takes all said files, and puts them together to make the executable
## Variables in Makefiles
There are two useful types of variable sin makefiles, normal vars, and makefile 'Macros'
	A var is defined with ALL_CAPS='' and is called with ${ALL_CAPS}
	Variable macros such as $^ -> dependencies, and $@ -> target
## Vectors
Don't forget to #include <vector> and use std::; vectors are LL. Creates a list which can be pushed back upon, and can iterate through elemnts with iterators, or get elements with [].
Elements are stored with arrays, and when max size is reached C++ doubles the array and copies elements to the new one.
## Iterators
Iterators are a part of the std library and are extended upon by different classes.
For example, std::vector<TYPE>::iterator x = myVec.begin();
Then you can easily advance through the iterator! Drerefrence iterator with \*x;
### Erasing using iterators
myVec.erase(x);
## Sets
Set is a dtype with holds elements, but only unique elements.
## Maps
A map is an ordered DS which hold a kwy value pair. Uses red black trees, O(log n) for ALL ops
## Pairs
A std::pair is part of utils std::pair<T1, T2> x;
# Class Design
## Abstract Data Types (ADTs)
A model for a class, without all of the implementation. It is the high level options of what a class can do.
## Accessor and Mutator Functions
Rather than make all vars public, we use private to limit access. We use accessor and mutator functions to make sure that our code is not modified in an unintended manner.
## Scope Resolution Operator
We use the scope resolution operator to state which namespace to get our code from. Think ::x() for global, or std::x() for in std.
We make a namespace with namespace x{int meh;}
## Big Three (Rule of Three)
The rule of three states that if you implement the assignment operator, then you need to implement the destructor and copy constructor. 
This holds for any arbitrary one of the above.
## Memory Organization of classes / structs
The only difference is that in classes members are private by default. For structs, members are private by default.
## Dynamic Array Allocation
We use `new` to allocate an array.
## Padding
Padding si offsets which are automatically built into the compiler. When we allocate memory, the padding is based off of the largest expeced word size in 4 byte increments. Either it is 4 (int) or 8 (double)
## Hex
We use hex by doing 16 counting. 1s place, 16s place, etc.
# Binary Search
Take a sorted array, greater of less than? div/2, div/2, etc. O(log n)
```cpp
void search(const int a[], size_t first, size_t size,
	int target, bool &found, size_t &location) {	
	size_t middle;
	// base case
	if (size == 0){
		found = false;
	} else {
		middle = first + size/2;
		// found the target
		if (target == a[middle]) {
			location = middle;
			found = true;
		} else if (target < a[middle]) {
			// recurse lower-half
			search(a, first, middle, target, found, location);
		} else {
			// recurse upper-half
			search(a, middle + 1, (size - 1)/2, target, found, location);
		}
	}
}
```	
# Quadratic-time Sorting
Some sorting algorithims are not particulary effecient. The quadratic algorithims are those with need to recycle through elements, becoming O(N^2) worst case.
## Bubble Sort
Bubble sort compares adjacent elements, and swaps elements. Can be optimized -- if no swaps, then sorted and return.
Therefore best case with optimization is O(n)
```cpp
void bubbleSort(int a[], size_t size) {
	int temp;
	bool swapped;
	for (int i = size - 1; i > 0; i--) {
		swapped = false;
		for (int j = 0; j < i; j++) {
			if (a[j] > a[j + 1]) {
				// swap
				temp = a[j];
				a[j] = a[j+1];
				a[j+1] = temp;
				swapped = true;
			}
		}
		if (!swapped) {
			// in order
			cout << "i = " << i << endl;
			cout << "already in order... returning" << endl;
			return;
		}
	}
}
```
## Selection Sort
Selection sort looks through the array and finds the largest element, then swaps the current element with that element.
O(N^2)
```cpp
void selectionSort(int a[], size_t size) {
	int temp;
	int largestIndex;
	int largest;
	for (int i = size - 1; i > 0; i--) {
		largestIndex = 0;
		largest = a[0];
		for (int j = 1; j <= i; j++) {
			if (a[j] > largest) {
				largest = a[j];
				largestIndex = j;
			}
		}
		temp = a[i];
		a[i] = a[largestIndex];
		a[largestIndex] = temp;
	}
}
```
## Insertion Sort
Make place for element then sort
```cpp
void insertionSort(int a[], size_t size) {
	int item;
	int shiftIndex;
	for (int i = 1; i < size; i++) {
		item = a[i];
		shiftIndex = i - 1;

		while (shiftIndex >= 0 && a[shiftIndex] > item) {
			a[shiftIndex + 1] = a[shiftIndex];
			shiftIndex -= 1;
		}
		a[shiftIndex + 1] = item;
	}	
}
```
# Hashing
Hashing is the process of taking some sort of an input, then converting it to a uniqu value using a set function. That function will hash the leemnt in a consistent manner and place it assign it a value within some sort of array.
## Collisions
Collissions occur when two elements hash to the same place. Imagine a hash function `x % CAP` This will clearly hash to the same spot for `x` and `x + capacity` even though they're two different elements.
How do we deal with this?
## Open-address Hashing
Open address hashing deals with collisions by moving an element that has a collision to the next linearly available spot.
This is dumb because then we end up with an uneuqal distribution of elements within the table. Also this makes sure that the array/table will max out.
## Double Hashing
Double hashing attempts to solve the distribution problem. First we hash the value. If there is a collission, then we use a second hash function to determine howmany slots we will then iterate by to look for the next element. To assure that we do not have an infinite loop, it is good practice to make the array length prime. 
## Load factor
The load factor is how full the table is, `LF = (slotsFull)/total`
## Chained Hashing
So what if we just want to keep on adding to our table to our satisfaction?
We deal with collissions by having each array element be a vector. In this vector, we just keep on pushing back elements if there is a collission.
## std::map
A `std::map` is a red-black tree with acts like a hash table. It keeps elements in-order, so it is easy to use an iterator to go through the elements.
O(log n for all ops)
## std::unordered_map
A true hash table. O(1) for insert and accss best case. If the elements are have collisisions, then the worst is O(N). Since this is unlikely, average is O(1)
# Divide and conquer
## Quicksort 
## Mergesort
# Testing
## Complete Test
## Unit Testing
# Inheritance
## Redefining inherited functions
## Inheritance Types
## Memory Slicing
## Pointers of base types
## Destructors and Inheritance
# Polymorphism
## Array of Pointers
## Pure Virtual Functions and Abstract Classes
# Exception Handling
## Throwing / Catching Multiple Exceptions
## Inheritance and Exceptions
# Function Pointers
### std::transform
## functions as paramaters
# Basic OS Concepts
## Application / OS / Hardware Stack
## Unix Processes / Threads
## PS unix
## Top unix
## Kill unix
## Foreground / background
## Suspend / Resume
## Unix Fork
## Execv
# Heaps
## Maxheap
### Minheap
## Heap as array
## Insertion and removing elements
### Heap as priority queue
## Heapsort
Read the reader sections related to this fun stuff