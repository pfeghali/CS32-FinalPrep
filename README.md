---
Peter Feghali @ Fall 2018
`CS32` Final Study Guide
---
#### Introduction:
This is an incomplete final study guide.
There is def material missing from here, albeit most should be faily valid.

---
##### *All code blocks are Richert Wang's unless otherwise noted, and is available on the course's Github site.*
---
# Compiling Review
Compiling is the process of taking your cpp code and generating an executable.
## C++ Build Process
The build process is made up of three distinct parts. Preprocessing, compiling, and linking.
	PP is code verification, type checking, and other considerations
	Compiling taked the code and generates object files
	Linking takes all said files, and puts them together to make the executable
## Variables in Makefiles
There are two useful types of variable sin makefiles, normal vars, and makefile 'Macros'
	A var is defined with `ALL_CAPS=''` and is called with `${ALL_CAPS}`
	Variable macros such as `$^` -> dependencies, and `$@` -> target
## Vectors
Don't forget to `#include <vector>` and use `std::`; vectors are LL. Creates a list which can be pushed back upon, and can iterate through elemnts with iterators, or get elements with `[i]` or `myVec.at(i)`.
Elements are stored with arrays, and when max size is reached C++ doubles the array and copies elements to the new one.
## Iterators
Iterators are a part of the std library and are extended upon by different classes.
For example, `std::vector<TYPE>::iterator x = myVec.begin();`
Then you can easily advance through the iterator! Drerefrence iterator with \*x;
### Erasing using iterators
`myVec.erase(x);`
## Sets
Set is a dtype with holds elements, but only unique elements.
## Maps
A map is an ordered DS which hold a ewy value pair. Uses red black trees, O(log n) for ALL ops
## Pairs
A `std::pair` is part of utils `std::pair<T1, T2> x;`
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
Padding si offsets which are automatically built into the compiler. When we allocate memory, the padding is based off of the largest expected word size in 4 byte increments. Either it is 4 (int) or 8 (double)
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
Some sorting algorithims are not particulary effecient. The quadratic algorithims are those with need to recycle through elements, becoming O(n^2) worst case.
## Bubble Sort
Bubble sort compares adjacent elements, and swaps elements. Can be optimized -> if no swaps, then must be sorted and return.
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
O(n^2)
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
Make place for element then sort - O(n^2)
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
O(log n) for all
## std::unordered_map
A true hash table. O(1) for insert and accss best case. If the elements have collisisions, then the worst is O(n). Since this is unlikely, average is O(1)
# Divide and conquer
D/C algorithims take a large problem, divy it up, and deal with it. There are two that we focused on in class.
## Quicksort 
QS is a method of sorting by choosing some sort of median/pivot value, then moving elements greater than or less than depending on that pivot. QS is O(nlogn) in best, but totally depends on pivot choice. With a bad and biased pivot, O(n^2)
```cpp
void partition(int a[], size_t size, size_t& pivotIndex) {
	int pivot = a[0];		 // choose 1st value for pivot
	size_t left = 1;		 // index just right of pivot
	size_t right = size - 1; // last item in array
	int temp;
	while (left <= right) {
		// increment left if <= pivot
		while (left < size && a[left] <= pivot) {
			left++;
		}
		// decrement right if > pivot
		while (a[right] > pivot) {
			right--;
		}
		// swap left and right if left < right
		if (left < right) {
			temp = a[left];
			a[left] = a[right];
			a[right] = temp;
		}
	}
	// swap pivot with a[right]
	pivotIndex = right;
	temp = a[0];
	a[0] = a[pivotIndex];
	a[pivotIndex] = temp;
}
void quicksort(int a[], size_t size) {
	size_t pivotIndex;		// index of pivot
	size_t leftSize;		// num elements left of pivot
	size_t rightSize;		// num elements right of pivot
	if (size > 1) {
		// partition a[] based on pivotIndex
		partition(a, size, pivotIndex);
		// Compute the sizes of left and right sub arrays
		leftSize = pivotIndex;
		rightSize = size - leftSize - 1;
		// recursive call sorting the left array
		quicksort(a, leftSize);
		// recursive call sorting the right array
		quicksort((a + pivotIndex + 1), rightSize);
	}
}
```
## Mergesort
Mergesort is always O(nlogn). Basically takes an array and continuously divides it until an element exist in each subarray. Then will recombine all of the sub-arrays in a sorted order.
```cpp
void merge(int a[], size_t leftArraySize, size_t rightArraySize) {
	// Note: we are assuming the left and right sub arrays are sorted
	int* tempArray;		// tempArray to hold sorted elements
	size_t copied = 0; 	// num elements copied to tempArray
	size_t leftCopied = 0;	// num elements copied from leftArray
	size_t rightCopied = 0;	// num elements copied from rightArray
	// create temp array
	tempArray = new int[leftArraySize + rightArraySize];
	// merge left and right arrays into temp in sorted order
	while ((leftCopied < leftArraySize) && (rightCopied < rightArraySize)) {
		if (a[leftCopied] < (a + leftArraySize)[rightCopied]) {
			tempArray[copied++] = a[leftCopied++];
		} else {
			tempArray[copied++] = (a + leftArraySize)[rightCopied++];
		}
	}
	// copy remaining elements from left/right sub arrays into tempArray
	// if elements in leftArray still exist, then ...
	while (leftCopied < leftArraySize) {
		tempArray[copied++] = a[leftCopied++];
	}
	// if elements in rightArray still exist, then ...
	while (rightCopied < rightArraySize) {
		tempArray[copied++] = (a + leftArraySize)[rightCopied++];
	}
	// Replace the sorted values into the original array
	for (int i = 0; i < leftArraySize + rightArraySize; i++) {
		a[i] = tempArray[i];
	}
	// free up memory
	delete [] tempArray;
}
void mergesort(int a[], size_t size) {
	size_t leftArraySize;
	size_t rightArraySize;
	if (size > 1) {
		leftArraySize = size / 2;
		rightArraySize = size - leftArraySize;
		// call mergesort on left array
		mergesort(a, leftArraySize); 
		// call mergesort on right array
		mergesort((a + leftArraySize), rightArraySize);
		// left and right sorted arrays together
		merge(a, leftArraySize, rightArraySize);
	}
}
```
# Testing
Testing is a fundemental part of any programming project.
## Types of tests
Types of tests:
- Normal cases: When we have expected usage of functions and programs
- Boundary cases: Cases that test the edge values of possible inputs. Think MAXINT or out of bound integers.
- Error cases: Invalid cases (like passing a string instead of an int). <- particulary a python thing (if using untyped Python)

## Test driven development. *(TDD)*
Test driven development is the idea of developing based off expected tests which need to pass. General tests and other edge cases with unexpected user input.
## Complete Test
Testing every single possible path code could go, and how every input would act. This is theoretically possible but realistically impossible.
## Unit Testing
Unit testing is the individual testing of functions to verify expected behavior.
# Inheritance
Inheritance is what it sounds like, you can have a class which inherits from a parent class.
How?
```cpp
class parentClass{public: int x = 0;}
class child : public parentClass{}
child x = child();
x.x = 1;
```
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