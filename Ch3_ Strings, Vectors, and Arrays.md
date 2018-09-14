


## Chapter 3.   Strings, Vectors, and Arrays
### 3.2 Library string Type
1\.  defining and initializing strings
```
	string s1; 				// default initialization; s1 is the empty string
	string s2 = s1; 		// s2 is a copy of s1
	string s3 = "hiya"; 	// s3 is a copy of the string literal
	string s4(10, 'c'); 	// s4 is cccccccccc
```
Initializing with '=' : copy initialize
Initializing without '=' : direct initialize

2\.  Some common operations:
```
	s.empty()				s.size()
	getline(is, s)			s[n]
```
3\.  size returns a **string::size_type** value. This type is unsigned. Under the new standard, we can ask the compiler to provide the appropriate type by using **auto** or **decltype**:
```
	auto len = line.size(); 		 // len has type string::size_type
```
4\. Adding Literals and strings
**When mix strings and string or character literals**, at least one operand to each \+ operator must be of string type:
```
	string s4 = s1 + ", ";			 // ok: adding a string and a literal
	string s5 = "hello" + ", "; 	 // error: no string operand
	string s6 = s1 + ", " + "world"; // ok: each + has a string operand
	string s7 = "hello" + ", " + s2; // error: can't add string literals
```
The initialization of s6 may appear surprising, this initialization groups as
```
	string s6 = (s1 + ", ") + "world"; // the subexpression s1 + ", " returns a string
	// but for s7
	string s7 = ("hello" + ", ") + s2; // error: can't add string literals
```
5\. Headers in C have names of the form **name .h**. The C++ versions of these headers are named c name—they remove the .h suffix and precede the name with the letter c. The c indicates that the header is part of the C library. Hence, **cctype** has the same contents as **ctype.h**, but in a form that is appropriate for C++ programs.
some cctype functions
```
	isdigit(c)	islower(c)	ispunct(c)	isspace(c)
	isupper(c)	tolower(c)	toupper(c)
```
6\.  To access every character in a string, we can use the **range for** statement:
```
	for(auto c : str)				for(auto &c : str) // value of c can be changed
		//......						//......
```
When compiling with g++ in ubuntu, there might be a warning:
>warning:  range-based ‘for’ loops only available with -std=c++11 or -std=gnu++11. 

Solution:  In cpp files, add
```
    #pragma GCC diagnostic error "-std=c++11"
```
or in command line,  add \" -std=c++11 \"
```
    g++ -std=c++11 xxx.cpp -o  xxx
```
###  3.3 Library Vector Type
1\. To use a **vector**, we must include the appropriate header.
```
	#include <vector>
	using std::vector;
```
2\. In the past, we had to supply a space between the closing angle bracket of the outer vector and its element type  vector<vector\<int\> > rather than vector<vector\<int\>>.

3\. Defining and initializing vectors
```
	vector<int> v1(10); 	// v1 has ten elements with value 0

	vector<int> v2{10};  	// v2 has one element  with value 10

	vector<int> v3(10, 1); 	// v3 has ten elements with value 1

	vector<int> v4{10, 1};  // v4 has two elements with values 10 and 1

	vector<string> svec(10); // ten elements, each an empty string

	vector<string> v5{"hi"}; // list initialization: v5 has one element

	vector<string> v6("hi"); // error: can't construct a vector from a string literal

	vector<string> v7{10};	 // v7 has ten default-initialized elements

	vector<string> v8{10, "hi"}; // v8 has ten elements with value "hi"
```
4\.  Short-cut for end-of-file:  **ctrl + D**  for ubuntu and **ctrl + Z** for vs.

5\. The subscript operator [] on **vector (and string)** fetches an existing element; it does not add an element.  **To add an element, use V.push_back(x)**
```
	vector<int> ivec;  // empty vector
	for (size_t ix = 0; ix != 10; ++ix)
		ivec[ix] = ix;  // disaster: ivec has no elements
_________________________________________________________________________
	for (decltype(ivec.size()) ix = 0; ix != 10; ++ix)
		ivec.push_back(ix);  // ok: adds a new element with value ix
_________________________________________________________________________	
	vector<int> ivec;  // empty vector
	cout << ivec[0];  // error: ivec has no elements!
```
6\.  common operations for vector
```
	v.empty()	v.size() 		v.capacity()    v.clear()	
	v.resize(size_type n)		v.reserve(size_type n)		
	v.erase(itr pos)			v.erase(itr first, itr last) 
	v.begin()	v.end()		// return iterator:
	v.front()   v.back()	// return reference:   
	v.pop_back()				v.push_back()	 
	v.swap(vector& x)
```
7\.    iterators for vectors
```
	auto b = v.begin(), e = v.end(); // b and e have the same type:  vector<T>::iterator 
	// b denotes the first element and e denotes one past the last element in v
	// *(e-1) denotes the reference to the last element in v
```
If the container is empty, begin returns the same iterator as the one returned by end.

8\.  Standard container iterator operations:

> ***iter , iter->mem , ++iter , --iter , iter1!=iter2 , iter1==iter2** 

9\. By routinely using **iterators** and **!=**, we don’t have to worry about the precise type of container we’re processing, rather than iterator and subscripts.

10\.  Iterator definitions in string and vector :
```
	vector<int>::iterator it; // it can read and write vector<int> elements
	string::iterator it2;  // it2 can read and write characters in a string
	vector<int>::const_iterator it3; // it3 can read but not write elements
	string::const_iterator it4;  // it4 can read but not write characters
```
11\. The library defines several other kinds of containers. **All of the library containers have iterators, but only a  few of them support the subscript operator.**  Technically speaking, **a string is not a container type**, but string supports many of the container operations. As we’ve seen string, like vector has a subscript operator. Like vectors, strings also have iterators.

12\. It is usually best to use a **const type (such as const\_iterator)** when we need to read but do not need to write to an object. To let us ask specifically for the const\_iterator type, the new standard introduced two new functions named **cbegin** and **cend**:   
```
	auto it3 = v.cbegin(); // it3 has type vector<int>::const_iterator
```
13\. The arrow operator combines dereference and member access into a single operation. That is,  **it->mem is a synonym for (\* it).mem**  

14\. It is important to realize that **loops that use iterators should not add elements to the container** to which the iterators refer. Any operation, such as push_back, that changes the size of a vector potentially invalidates all iterators into that vector.

### 3.5 Arrays
1\. If arrays are initialized from string literals, it is important to remember that string literals end with a null character.
```
	char a2[] = {'C', '+', '+', '\0'}; // list initialization, explicit null

	char a3[] = "C++";  // null terminator added automatically

	const char a4[6] = "Daniel";  // error: no space for the null!
```
The dimensions of a2 and a3 are both 4. The definition of a4 is in error. Although the literal contains only six explicit characters, the array size must be at least seven—six to hold the literal and one for the null.

2\.  We cannot initialize an array as a copy of another array, nor is it legal to assign one array to another.

3\.  Defining **arrays that hold pointers** is fairly straightforward, **defining a pointer or reference to an array** is a bit more complicated:
```
	int *ptrs[10];  		  //  ptrs is an array of ten pointers to int

	int &refs[10] = /* ? */;  //  error: no arrays of references (reference has no address)

	int (*Parray)[10] = &arr; //  Parray points to an array of ten ints
	// to access arr[3], use *(*p + 3) or *(p[0] + 3)

	int (&arrRef)[10] = arr;  //  arrRef refers to an array of ten ints

	int *(&arry)[10] = ptrs;  //  arry is a reference to an array of ten pointers
```
4\. size\_t is a machine-specific unsigned type that is guaranteed to be large enough to hold the size of any object in memory. The size\_t type is defined in the cstddef header.

5\.  ``int *e = &arr[10]; // pointer just past the last element in arr``

6\. **Arrays have functions begin and end** ,which are **defined in namespace std**.
```
	using std::begin; using std::end;

	int ia[] = {0,1,2,3,4,5,6,7,8,9}; // ia is an array of ten ints

	int *beg = begin(ia); // pointer to the first element in ia

	int *last = end(ia);  // pointer one past the last element in ia
```
7\. We cannot initialize a built-in array from another array. Nor can we initialize an array from a vector. However, **we can use an array to initialize a vector**. To do so, we specify the address of the first element and one past the last element that we wish to copy:
```	
	int int_arr[] = {0, 1, 2, 3, 4, 5};

	// ivec has six elements; each is a copy of the corresponding element in int_arr

	vector<int> ivec(begin(int_arr), end(int_arr));
```
As a result, ivec will have six elements.

### 3.6 Multidimensional Array
1\.  In a two-dimensional array, the first dimension is usually referred to as the row and the second as the column.

2\.  Using a Range for with Multidimensional Arrays
```
	size_t cnt = 0;
	for (auto &row : ia) 			 // for every element in the outer array
		for (auto &col : row) {		 // for every element in the inner array
			col = cnt; 				 // give this element the next value
			++cnt; 					 // increment cnt
		}
```
3\. Pointers and Multidimensional Arrays
```
	int ia[3][4]; 		// array of size 3; each element is an array of ints of size 4
	int (*p)[4] = ia; 	// p points to an array of four ints
	p = &ia[2]; 		// p now points to the last element in ia
```
### Chapter summary
Among the most important library types are vector and string. A string is a variable-length sequence of characters, and a vector is a container of objects of a single type.

Iterators allow indirect access to objects stored in a container. Iterators are used to access and navigate between the elements in strings and vectors.

Arrays and pointers to array elements provide low-level analogs to the vector and string libraries. In general, the library classes should be used in preference to low level array and pointer alternatives built into the language.
