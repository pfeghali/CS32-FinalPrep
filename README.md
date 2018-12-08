---
Peter Feghali @ Fall 2018
UCSB `CS32` Final Study Guide
---
#### Introduction:
This is an incomplete final study guide.
There is def material missing from here, albeit most should be faily valid.

---
##### *All code blocks are Richert Wang's unless otherwise noted, and is available on the [course's Github site](https://ucsb-cs32-f18.github.io/).*
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
class parentClass{
	public:
		int x = 0;
		void f(int z);
	}
class child : public parentClass{
	public:
		void f(int z);
	}
child x = child();
x.x = 1;
//@Feghali
```
## Redefining inherited functions
When inheriting from a parent we can redefine base class fuctionality. There are two distinct ways of doing this well.
First, we use normal nomenclature and redefine code in an expected fashion with no base class requirements. This will assure that the correct function is used when the type is expected and known. Two examples
```cpp
Child x = child();
x.f(0); // calls childs f
parentClass* x = new child();
x->f(0); // calls parent's f
//@Feghali
```
The second way of doing this is with the `**virtual**` keyword.
### Virtual
The virtual keyword means that a function can be called from the child class (essentially, will go into more detail later). Define it as early as possible.
This leads to the following code:
```cpp
class parentClass{
	public:
		virtual int x = 0;
		void f(int z);
	}
class child : public parentClass{
	public:
		void f(int z);
	}
Child x = child();
x.f(0); // calls childs f
parentClass* x = new child();
x->f(0); // calls child's f!!
//@Feghali
```
## Inheritance Types
There are three types of inheritance: Public, protected, private. Each provide an upper bound on accesibility of parent classes within the new class.
## Memory Slicing
```cpp
class parentClass{
	public:
		int x = 0;
		void f(int z);
	}
class child : public parentClass{
	public:
		int a;
		void f(int z);
	}
child x = child();
parentClass = x; // SLICED int a !
//@Feghali
```
The idea is that when you assign a child to parent object, while you can, you lose the e=memory associated with the child in the copy. Albeit, data is not lost but solely out of scope when using pointers..
## Pointers of base types
We can use pointer of base types to create arrays of pointers to children effectively. What is useful about this is that this allows for general functions of such data structures. This will only call functions within the scope which is defined by the pointer, unless it is virtual.
## Destructors and Inheritance
Destructors need to be labeled as virtual to be called with a delete. Why?
Let's imagine if they were not. If we do not a have a virtual desructor, then lets try calling `delete[] anArrayOfParentObjects`. Works great! All of the objects seem to be delted. But if we have objects of the child class within that array, then while the destructor for the parent is called, the children are never destructed. That is why it so important to be call destructors virtual, since otherwise memory could very easily be lost.
In addition, it is important to see where in the chain of heireachy the virtual is labelled.
# Polymorphism
Virtual functions and the relabelling of different parts of code in different ways. We basically use poly to use a single interface to call a bunch of different code dynamically.
## Array of Pointers
We can use an array of pointers to effectively point to elements of base and child types.
```cpp
class parentClass{
	public: 
		int x = 0;
		void f(int z);
	}
class child : public parentClass{
	public:
		void f(int z);
	}
parentClass x[2];
x[0] = new child();
x[1] = new parentClass();
x.x = 1;
//@Feghali
```
## Pure Virtual Functions and Abstract Classes
Pure virtual functions are when we set a function to not be defined within a base class.
```cpp
class parentClass{
	public:
		int x = 0;
		void f(int z) = 0;
	}
class child : public parentClass{
	public:
		 void f(int z);
	}
child x = child();				// VALID
parentClass x = parentClass();	// INVALID
//@Feghali
```
When we do so, we lose the ability to instantiate objects of that class. We CAN create pointers of that type though.
Any pure virtual functions ___MUST___ be defined in the children, otherwise the class cannot be compiled. Therefore when we inherit from a class with at least one pure virtual function, we call that inheriting from an *Abstract Class*.
## We should discuss virtual destructors more...
Imagine we have the three following classes.
```cpp
class x {public: void f() {std::cout << "x" << std::endl};}
class y : public x {public: virtual void f() {std::cout << "y" << std::endl};}
class z : public y {public: void f() {std::cout << "z" << std::endl};}
```
Let's  now add some code to call these functions:
```cpp
x a = x();
y b = b();
z c = z();
a.f(); //x
b.f(); //y
c.f(); //z
//@Feghali
```
Perfect! Now lets play with it!
```cpp
x* a = new x();
a->f(); // x

y* b = new y();
b->f(); // y

z* c = new z();
c->f(); // z

y* bb = b;
b = c;
b->f(); // z

delete a;
a = b;
a->f(); // x

delete a;
delete bb;

a = new z();
a->f(); // x

delete a;
//@Feghali
```
So what is going on here? Why is the virtual function of z not being called? That is ridiculous!? It is not being called since even though the function is relabled as virtual down the inheritance heirachy, pointers of type x do not know that! Therefore it will only be able to call methods associated with its own scope.
# Exception Handling
Let's throw things around! Exceptions are the anwser to what to do when code breaks and you want to blame someone else. 
Imagine we have some code which is going to do division over 10000 random values and return the average, and we have code like this:
```cpp
double averageR(){
	double sad = 0;
	double tmp = 0;
	for(int i = 0; i < 10000; i++){
		tmp = random(-100,100);
		sad = (sad + tmp)/tmp;
	}
	return sad;
}
//@Feghali
```
This is pretty sad code obviously. This'll get a divide by zero, the computer will fail, and the code will be sad.
A smart way to fix this would be to do a zero case if statement and move on with life, but we're not smart. Let's use exceptions instead!
```cpp
class divideByZero{};
double averageR() throws divideByZero(){
	double sad = 0;
	double tmp = 0;
	for(int i = 0; i < 10000; i++){
		tmp = randomdbl(-100,100);
		if(tmp == 0) throw divideByZero();
		sad = (sad + tmp)/tmp;
	}
	return sad;
}
bool notdone = true;
while(notdone){
	try{
		averageR();
		notdone = false;
	}catch (divideByZero e){
		std::cout << "Divide by zero error!" << std::endl;
	}
}
//@Feghali
```
## Throwing / Catching Multiple Exceptions
Well let's take a look back at our code, and do something else. How about we also bound our code so that we cannot end up with infinity? Inf occurs with a tiny divisor.
```cpp
class divideByZero{};
class toosmall : public divideByZero{};
double averageR() throws divideByZero(){
	double sad = 0;
	double tmp = 0;
	for(int i = 0; i < 10000; i++){
		tmp = randomdbl(-100,100);
		if(tmp == 0) throw divideByZero();
		if(tmp  < .00000000001 && tmp > -.00000000001) throw toosmall();
		sad = (sad + tmp)/tmp;
	}
	return sad;
}
bool notdone = true;
while(notdone){
	try{
		averageR();
		notdone = false;
	}catch (divideByZero e){
		std::cout << "Divide by zero error!" << std::endl;
	}catch (toosmall e){
		std::cout << "Divide by too small a value error!" << std::endl;
	}
}
//@Feghali
```
Cool! Seems and feels like happy memes.
Nope, it isn't. Since `toosmall` inherits from `divideByZero`, toosmall's exception will never be caught!
Fixed version: 
```cpp
//Imagine the code from above...
bool notdone = true;
while(notdone){
	try{
		averageR();
		notdone = false;
	}catch (toosmall e){
		std::cout << "Divide by too small a value error!" << std::endl;
	}catch (divideByZero e){
		std::cout << "Divide by zero error!" << std::endl;
	}
}
//@Feghali
```
It is really important to be careful with how you throw exceptions. While the compiler will generate a warning for this case, it is important to be mindful of this behavior.
## Inheritance and Exceptions
Look above!
# Function Pointers
Literally a pointer to a function. Be sure to understand the syntax of defining a function pointer: `return_type (*function_pointer_name)(type arg1, type arg2) = function_with_same_signature;`
```cpp
double functionThatReturnsADouble(int z, int y, char t, const double m) {
	return m;
};
double (*func2)(int z, int y, char t, const double m) = functionThatReturnsADouble;
std::cout << func2(2 , 567363993, '\t', 8.001) << std::endl;
//@Feghali
```
## Functions as parameters
This is helpful for passing functions around, and using functions dynamically for a multiplcty of tasks. One can imagine implementing a version of quicksort, mergesort, etc whihch utilize function pointers.
#### Let's look at my code from midterm 2.
I implemented a `typedef` for comparisons. Why? i wanted to assure myself that the functions were consistent with parameters. 
```cpp
typedef bool (*compareFunc)(int,int);
bool compareInts(int a, int b){
	return a > b;
}
//@Feghali
```
Now lets look at **quicksort** (This code may differ from Wang's):
```cpp
void partition(int a[], size_t size, size_t& pivot, compareFunc cmp) {
	int pivot_value = a[0];
	size_t left_pt = 1;
	size_t right_pt = size - 1;
	int temp = -1;
	while(left_pt <= right_pt){
		while(left_pt < size && cmp(a[left_pt], pivot_value)){
			left_pt++;
		}
		while(!(cmp(a[right_pt], pivot_value)) && a[right_pt] != pivot_value){
			right_pt--;
		}
		if(left_pt < right_pt){
			temp = a[left_pt];
			a[left_pt] = a[right_pt] ;
			a[right_pt] = temp;
		}
	}
	pivot = right_pt;
	temp = a[0];
	a[0] = a[pivot];
	a[pivot] = temp;
}

void quicksort(int a[], size_t size) {
	size_t pivot;
	size_t leftSize;
	size_t rightSize;
	if(size > 1){
		partition(a, size, pivot, compareInts);
		leftSize = pivot;
		rightSize = size - pivot - 1;

		quicksort(a, leftSize);

		quicksort(a + leftSize + 1, rightSize);
	}
}
//@Feghali
```
It should be pretty clear that there is no big difference between this and the normal quicksort. This is simply more generalizable for different types of sorting.
Now lets look at **mergesort** (This code may differ from Wang's):
```cpp
void merge(size_t left, size_t right, int src[], compareFunc cmp){
	size_t total_size = left + right;
	int* temp = new int[total_size];
	size_t counter = 0;
	size_t leftCount = 0;
	size_t rightCount = 0;
	for(int i = 0; leftCount < left && rightCount < right; i++){ //Merge while it still can check;
		if(cmp(src[leftCount], src[left + rightCount])){
			temp[i] = src[leftCount];
			leftCount = leftCount + 1;
		}else{
			temp[i] = src[left + rightCount];
			rightCount = rightCount + 1;
		}
		counter = i;
	}
	if(leftCount != left){
		for(int i = leftCount; i < left; i++){
			temp[right + i] = src[i];
		}
	}else{
		for(int i = rightCount; i < right; i++){
			temp[left + i] = src[left + i];
		}
	}
	for(int i = 0; i < left + right; i++){
		src[i] = temp[i];
	}
	delete[] temp;
	return;
}
void mergesort(int a[], size_t size) {
	size_t leftArr = size/2;
	size_t rightArr = size - leftArr;
	if(size > 1){
		mergesort(a, leftArr);
		mergesort(a+leftArr, rightArr);
		merge(leftArr, rightArr, a, compareInts);
	}
	return;
}
//@Feghali
```
### std::transform
```cpp
#include <algorithm>
#include <string>

std::string str = "Hello World";
std::transform(str.begin(), str.end(),str.begin(), ::toupper);
//https://stackoverflow.com/a/735215/
```
## Callbacks
A callback is when we throw function pointers around to do particular computation later.
# Templates
Templates allow users to generalize coe with arbitrary types. This is helpful for scenarios with a *code once deploy everywhere* sort of throught process, where the particulars of the data that you're working with is unimportant.
Here is an example of a templated function:
```cpp
template<class class_name_to_use_in_func>
std::vector<class_name_to_use_in_func> returnAVec(class_name_to_use_in_func obj1, class_name_to_use_in_func obj2){
	std::vector<class_name_to_use_in_func> temp_vec_with_long_name;
	temp_vec_with_long_name.push_back(obj1);
	temp_vec_with_long_name.push_back(obj2);
	return temp_vec_with_long_name;
}
//@Feghali
```
# Basic OS Concepts
An operating system is this huge massive abstraction which allows users to do computation. We define computation as literally anything that has to do with a computer doing an action. Want to ping a server? Computation. Read a PMC? Computation. Want to blink a pixel? Computation. Want to add two numbers? Do that mentally.
An OS gives users a streamlined way of interacting with code, interfacing with other programs, and requiring programmers to not reinvent the wheel. Particularly, this comes into importance with the kernel. The kernel is a set of predefined library functions which define fundemental OS code.
## OS
An operating system is a peice of software which isolated you from the complexities of hardware resources. There are 3 types of operating systems. 
1. Single user, single process system - One user and one process at at time.
2. Single user, multi-process system - One user, but the OS allows users to execute multiple processes simultaneously.
## Application / OS / Hardware Stack
The app stack is where everythig exists. I really don't want to get into this, so I won't. This should be implicitly well defined with everything else.
## Unix Processes
If I want to run a program , I use a process. A process is the home of where your program runs. THe process is the defined memory area where your process exists, with whatever other relevant metadata is necessary. Key things are the `Process ID`, as well as the `Parent Process ID`. When one opens a bash shell and runs a program, that running program is a process. So how does that work?
## How bash shells work
Users are bad. Inherently a computer is a precise instrument, and users are stochastic brutes. The bash shell is built to allow for mistakes, safely recover from them, and move on. So how does that work? We can safely run programs from a parent through isolation. The bash shell isolates processes by creating some sort of a container for them to live in by duplicating the current process space and providing a space to survive in. After the program ends, the result is sent back, and the shell continues. But particularly, we should keep in mind that any output is piped back to the original shell no matter what. This is die to the fact that the program running is in isolation, and won't kill the parent.
## Unix Fork
The Unix fork splits a process by copying the process space and opening a new place to run a process. In cpp, fork is implemented with the `#include <unistd.h>` library. When calling fork(), the process id of the new process is returned to the parent, and the child gets a return value of 0.
```cpp
#include <unistd.h> // sleep(), fork(), pid_t (in sys/types.h)

pid_t result = fork();
// parent──fork─┬────parent_result = PID of child
//              └────child_result = 0
//@Wang + @Feghali
```
Pretty straightforward! The fork copies over, and once forked, the fork runs code independently. You can have interprocess communication (IPC) either through shared memory, specific memory pointers with careful memory locking, or potentially file IO.
## ps unix
The `ps` command displays information about the current running processes.
```console
[peterfeghali@csil-04 ~]$ ps
  PID TTY          TIME CMD
28937 pts/0    00:00:00 bash
29000 pts/0    00:00:00 ps
```
#### ps -e
The -e flag gets all current processes on the system.
```console
[peterfeghali@csil-04 ~]$ ps -e
  PID TTY          TIME CMD
    1 ?        00:00:04 systemd
    2 ?        00:00:00 kthreadd
...
29690 pts/0    00:00:00 bash
29729 pts/0    00:00:00 ps
31424 ?        00:00:00 NFSv4 callback
```
## top unix
From man: "The top program provides a dynamic real-time view of a running system.  It can display system summary information as well as a list of processes or threads currently being managed by the Linux kernel. The types of system summary information shown and the types, order and size of information displayed for processes are all user configurable and that configuration can be made persistent across restarts."
In other words, top allows for easy visualization of what particulary is happening on your computer.
```console
[peterfeghali@csil-04 ~]$ top
top - 20:20:11 up 1 day,  2:11, 10 users,  load average: 0.50, 0.33, 0.19
Tasks: 281 total,   1 running, 225 sleeping,   0 stopped,   0 zombie
%Cpu(s):  0.2 us,  0.8 sy,  0.9 ni, 97.7 id,  0.0 wa,  0.2 hi,  0.1 si,  0.0 st
KiB Mem :  8091892 total,  2242632 free,  2086268 used,  3762992 buff/cache
KiB Swap:  8229884 total,  8229884 free,        0 used.  5497840 avail Mem

  PID USER      PR  NI    VIRT    RES    SHR S  %CPU %MEM     TIME+ COMMAND
23240 charanp+  39  19 3919016 247760 102496 S   1.3  3.1   1:10.96 gnome-shell
...
30313 peterfe+  39  19  164464   4576   3732 R   0.3  0.1   0:00.06 top
    2 root      20   0       0      0      0 S   0.0  0.0   0:00.03 kthreadd
```
## kill unix
Kill fundementally sends signals to programs. Certain signals can be sent, such as the terminate signal or logout signal to end processes.
```console
kill: usage: kill [-s sigspec | -n signum | -sigspec] pid | jobspec ... or kill -l [sigspec]
```
## jobs unix
If we run `top`, then suspend it with `ctrl+z`, the output will be
```console
[peterfeghali@csil-04 ~]$ jobs
[1]+  Stopped                 top
```
## Foreground / background
Now that we know how to see suspended jobs, how can we resume those jobs?
```console
[peterfeghali@csil-04 ~]$ jobs
[1]+  Stopped                 top
[peterfeghali@csil-04 ~]$ fg %1
top - 20:41:28 up 1 day,  2:32, 10 users,  load average: 1.07, 0.59, 0.33
Tasks: 284 total,   1 running, 234 sleeping,   0 stopped,   0 zombie
%Cpu(s):  0.1 us,  1.0 sy,  1.5 ni, 97.1 id,  0.0 wa,  0.3 hi,  0.1 si,  0.0 st
KiB Mem :  8091892 total,  2112940 free,  2190584 used,  3788368 buff/cache
KiB Swap:  8229884 total,  8229884 free,        0 used.  5382548 avail Mem

  PID USER      PR  NI    VIRT    RES    SHR S  %CPU %MEM     TIME+ COMMAND
24342 charanp+  39  19 2074404 261272 137324 S   2.9  3.2   0:45.86 Web Content
30773 tinghaur  39  19  158288   9308   5700 S   1.3  0.1   0:14.19 vim
```
This is called running jobs in the foreground, which were previously in the background. What is we want to run a job in the background, then bring it back to the foreground? We can do so with the `&` when running programs. It'll send programs to execute in the background, which we can then bring back to the foreground with jobs and such.
## Suspend / Resume
We can suspend the currently running programs with `ctrl+z`
## exec
`exec` executes a program, then rather than allowing the shell to continue by forking the process, it doesn't. This kills the terminal after running a program with this notation.
```console
[peterfeghali@csil-04 ~]$ exec date
Thu Dec  6 20:49:56 PST 2018
~ shell is now dead ~
```
## Device and OS interaction
### Drivers and such
## The entire reader in a few paragraphs
Compilers main purpose is to translate symbolic references to actual memory address references.
### Compiling
Compilation is the process of linking a group of object files together to form some sort of executable.
Linking truly is two distinct processes, relocation and linking. Similarly, compiling is also a two step process of compiling and linking.
C++ programs are commonly in some sort of a flat-table layout. The keyword `static` refers to global variables which cannot be referenced in a different source file.
## Object modules
`
┌────────────────────────────────┐  
│ Header section                 │  
├────────────────────────────────┤  
│ Machine code section           │  
├────────────────────────────────┤  
│ Initialized data section       │  
├────────────────────────────────┤  
│ Symbol table section           │  
├────────────────────────────────┤  
│ Relocation information section │  
└────────────────────────────────┘
`
1. Relocation is when object files are merged together and internal memory addresses are updated to reflect offset changes.
2. Linking resolves external memory addresses.
3. Memory addresses must be mapped from relative memory addresses to physical addresses.

## Overflow
Overflow occurrs when we try storing something larger than what can be physically fit into a memory address.
If overflow occurs due to a numeric operation, then left end overflow. Otherwise, it is allowed to happen.

# Threads
Threads are individual parts of a program in execution. We can consider a thread a running subsect of a program. We can spawn threads to run code, functions, whatevr in parallel. In the past this was a fairly substantial amount of work due to pthreads. With C++11 this has been simplified with the introduction of the `thread` std library. This library provides a set of functions to simplify the process of working with threads. We can spwan threads on initialization. We then can call `myThread.join()` to pause runtime to get back to concurrency. We can also call `myThread.detach()` to detach a thread. Either of the aformentioned actions must be completed to correctly destroy a thread.
## C++ 11 Threads
We must include the new library to call threads.
```cpp
#include <thread>

void foo(int x, double y, char t){ ... }
std::thread th1(foo, 5, 6.02, 'r');

th1.join();
```
## Race conditions
A race condition is when you have two threas trying to acess the same portion of memory at the same time. This causes the function to be a 'race' to do computation.
## Mutex
The mutex library provides a method of locking states and assuring that there is no duplicated code execution happening at the same time. Only a single thread can lock or unlock the code at one time.
## Deadlocks
Deadlocks occur when two threads are refusing to lock or unlock their code, as their locks are interdependent.
# Heaps
## Maxheap
### Minheap
## Heap as array
## Insertion and removing elements
### Heap as priority queue
## Heapsort
Read the reader sections related to this fun stuff