# Chapter 1 ~ 6

## Chapter 3.   Strings, Vectors, and Arrays
1\.  size returns a **string::size_type** value. This type is unsigned. Under the new standard, we can ask the compiler to provide the appropriate type by using **auto** or **decltype**:
```
	auto len = line.size(); // len has type string::size_type
```

2\.  	``string s7 = ("hello" + ", ") + s2; // error: can't add string literals``

3\. Headers in C have names of the form **name .h**. The C++ versions of these headers are named c name—they remove the .h suffix and precede the name with the letter c. The c indicates that the header is part of the C library.Hence, **cctype** has the same contents as **ctype.h**, but in a form that is appropriate for C++ programs.

4\. When compiling with g++ in ubuntu, there might be a warning:
>warning:  range-based ‘for’ loops only available with -std=c++11 or -std=gnu++11. 

Solution:
In cpp files, add
 ``#pragma GCC diagnostic error "-std=c++11"``
or in command line,  add \" -std=c++11 \"
``g++ -std=c++11 xxx.cpp -o  xxx``

5\. To use a **vector**, we must include the appropriate header.
```
	#include <vector>
	using std::vector;
```
6\. In the past, we had to supply a space between the closing angle bracket of the outer vector and its element type  vector<vector<int> > rather than vector<vector<int>>.

7\. 
```
	vector<int> v1(10);  // v1 has ten elements with value 0

	vector<int> v2{10};  // v2 has one element  with value 10

	vector<int> v3(10, 1); // v3 has ten elements with value 1

	vecto<int> v4{10, 1}; // v4 has two elements with values 10 and 1

	vector<string> svec(10); // ten elements, each an empty string

	vector<string> v5{"hi"}; // list initialization: v5 has one element

	vector<string> v6("hi"); // error: can't construct a vector from a string literal

	vector<string> v7{10};  // v7 has ten default-initialized elements

	vector<string> v8{10, "hi"}; // v8 has ten elements with value "hi"
```
8\.  **short-cut for end-of-file :  ctrl + D**

9\. The subscript operator [] on **vector (and string)** fetches an existing element; it does not add an element.  **To add an element, use V.push_back(x)**
```
	vector<int> ivec;  // empty vector
	for (decltype(ivec.size()) ix = 0; ix != 10; ++ix)
		ivec[ix] = ix;  // disaster: ivec has no elements
_________________________________________________________________________
	for (decltype(ivec.size()) ix = 0; ix != 10; ++ix)
		ivec.push_back(ix);  // ok: adds a new element with value ix
_________________________________________________________________________	
	vector<int> ivec;  // empty vector
	cout << ivec[0];  // error: ivec has no elements!
```
10\.    ```auto b = v.begin(), e = v.end(); // b and e have the same type```
// the compiler determines the type of b and e; see § 2.5.2 (p. 68)
// b denotes the first element and e denotes one past the last element in v
// *(e-1) denotes the reference to the last element in v
If the container is empty, begin returns the same iterator as the one returned by end.

11\.  standard container iterator operations:

> ***iter , iter->mem , ++iter , --iter , iter1!=iter2 , iter1==iter2** 

12\. By routinely using **iterators** and **!=**, we don’t have to worry about the precise type of container we’re processing, rather than iterator and subscripts.

13\.  Iterator definitions in string and vector :
```
vector<int>::iterator it; // it can read and write vector<int> elements
string::iterator it2;  // it2 can read and write characters in a string
vector<int>::const_iterator it3; // it3 can read but not write elements
string::const_iterator it4;  // it4 can read but not write characters
```
14\. the library defines several other kinds of containers.** All of the library containers have iterators, but only a few of them support the subscript operator.** Technically speaking, **a string is not a container type**, but string supports many of the container operations. As we’ve seen string, like vector has a subscript operator. Like vectors, strings also have iterators.

15\. It is usually best to use a **const type (such as const\_iterator)** when we need to read but do not need to write to an object. To let us ask specifically for the const\_iterator type, the new standard introduced two new functions named **cbegin** and **cend**:   
```
	auto it3 = v.cbegin(); // it3 has type vector<int>::const_iterator
```
16\. The arrow operator combines dereference and member access into a single operation. That is,  **it->mem is a synonym for (\* it).mem**  

17\. It is important to realize that **loops that use iterators should not add elements to the container** to which the iterators refer. Any operation, such as push_back, that changes the size of a vector potentially invalidates all iterators into that vector.

18\. If arrays are initialized from string literals, it is important to remember that string literals end with a null character.
```
char a2[] = {'C', '+', '+', '\0'}; // list initialization, explicit null

char a3[] = "C++";  // null terminator added automatically

const char a4[6] = "Daniel";  // error: no space for the null!
```
The dimensions of a2 and a3 are both 4. The definition of a4 is in error. Although the literal contains only six explicit characters, the array size must be at least seven—six to hold the literal and one for the null.

19\.  Defining **arrays that hold pointers** is fairly straightforward, **defining a pointer or reference to an array** is a bit more complicated:
```
int *ptrs[10];  //  ptrs is an array of ten pointers to int

int &refs[10] = /* ? */;  //  error: no arrays of references

int (*Parray)[10] = &arr; //  Parray points to an array of ten ints

int (&arrRef)[10] = arr;  //  arrRef refers to an array of ten ints

int *(&arry)[10] = ptrs;  //  arry is a reference to an array of ten pointers
```
20\. size\_t is a machine-specific unsigned type that is guaranteed to be large enough to hold the size of any object in memory. The size\_t type is defined in the cstddef header.

21\.  ``int *e = &arr[10]; // pointer just past the last element in arr``

22\. **Arrays have functions begin and end** ,which are **defined in namespace std**.
```
	using std::begin; using std::end;

	int ia[] = {0,1,2,3,4,5,6,7,8,9}; // ia is an array of ten ints

	int *beg = begin(ia); // pointer to the first element in ia

	int *last = end(ia);  // pointer one past the last element in ia
```
23\. We cannot initialize a built-in array from another array. Nor can we initialize an array from a vector. However, we can use an array to initialize a vector. To do so, we specify the address of the first element and one past the last element that we wish to copy:

```	
	int int_arr[] = {0, 1, 2, 3, 4, 5};

	// ivec has six elements; each is a copy of the corresponding element in int_arr

	vector<int> ivec(begin(int_arr), end(int_arr));
```
As a result, ivec will have six elements.


## Chapter 4.  Expressions
1\. Roughly speaking, when we **use an object as an rvalue**, we use **the object’s value (its contents)**. When we **use an object as an lvalue**, we **use the object’s identity (its location in memory)**.

2\. The precedence of postfix increment is higher than that of the dereference operator, so **\*pbeg++ is equivalent to \*(pbeg++)**. 
>The subexpression pbeg++ increments pbeg and yields a copy of the previous value of pbeg as its result.

3\. **The Member Access Operators** : **the dot operator** fetches a member from an object of class type; **arrow** is defined so that ptr -> mem is a synonym for (* ptr ). mem :
```
	string s1 = "a string", *p = &s1;

	auto n = s1.size(); // run the size member of the string s1

	n = (*p).size();  // run size on the object to which p points

	n = p->size();  // equivalent to (*p).size()
```
Because dereference has a lower precedence than dot, we must parenthesize the dereference subexpression.

4\.  Because there are no guarantees for how the sign bit is handled, we strongly **recommend using unsigned types with the bitwise operator**.

5\. 	  ``unsigned char bits = 0233  ; // 0233 is an octal literal.``

6\. The ***sizeof*** Operator : the operator takes one of two forms:
```
sizeof (type)

sizeof expr

Sales_data data, *p;

sizeof(Sales_data); // size required to hold an object of type Sales\_data

sizeof data; // size of data's type, i.e., sizeof(Sales_data)

sizeof p;  // size of a pointer

sizeof *p;  // size of the type to which p points, i.e., sizeof(Sales_data)

sizeof data.revenue; // size of the type of Sales_data's revenue member

sizeof Sales_data::revenue; // alternative way to get the size of revenue
```
7\.  When the signedness differs and the type of the unsigned operand is the same as or larger than that of the signed operand, the signed operand is converted to unsigned. For example, given an unsigned int and an int, the int is converted to unsigned int.


## Chapter 5. Statements
1\.  A continue statement terminates the current iteration of the nearest enclosing loop and immediately begins the next iteration. A continue can appear only inside a for, while, or do while loop.

2\.  A goto statement provides an unconditional jump from the goto to a another statement in the same function.


>**Programs should not use gotos**.  gotos make programs hard to understand and hard to modify.

The syntactic form of a goto statement is
```
goto label;
```
where label is an identifier that identifies a statement. A labeled statement is any statement that is preceded by an identifier followed by a colon:
```
end: return;  // labeled statement; may be the target of a goto
```
3\.  Throw expressions, which the detecting part uses to indicate that it encountered something it can’t handle. We say that a throw raises an exception.
```
// first check that the data are for the same item
if (item1.isbn() != item2.isbn())
throw runtime_error("Data must refer to same ISBN");

// if we're still here, the ISBNs are the same
cout << item1 + item2 << endl;
```
Throwing an exception terminates the current function and transfers control to a handler that will know how to handle this error.

The type **runtime_error** is **one of the standard library exception types** and is **defined in the stdexcept header**.
```
	#include<stdexcept>
	using std::runtime_error;
```
4\.  The try Block:  The general form of a try block is
```
	try {
	program-statements
	} catch (exception-declaration) {
	handler-statements
	} catch (exception-declaration) {
	handler-statements
	} // . . .
```
Following the try block is a list of one or more catch clauses. A catch consists of three parts: the keyword catch, the declaration of a (possibly unnamed) object within parentheses (referred to as an exception declaration), and a block.When a catch is selected to handle an exception, the associated block is executed. Once the catch finishes, execution continues with the statement immediately following the last catch clause of the try block.

## Chapter 6. Functions
1\.  A function definition typically consists of a return type , a name, a list of zero or more parameters, and a body. The parameters are specified in a comma-separated list enclosed in parentheses. The actions that the function performs are specified in a statement block, referred to as the function body .

2\. `` if(!strcmp(str1,str2)) {...}  ``
	**strcmp() return value**:  
	<0  the first character has a lower value in ptr1 than in ptr2  
	0  the contents of both strings are equal  
 \>0  the first character has a greater value in ptr1 than in ptr2  
  
3\.  Use the C++ stringstream class to convert the third command line argument from text to an integer format.  
```  
	#include <sstream>  
	...  
	int divideWith = 0; // convert our input string to number - C++ style  
	stringstream s;  
	s << argv[2];  
	s >> divideWith;  
  ```
4\.  A parameter list typically consists of a comma-separated list of parameters, each of   which looks like a declaration with a single declarator. Even when the types of two  parameters are the same, the type must be repeated:  
```  
	int f3(int v1, v2) { /* ... */ }  // error  
	int f4(int v1, int v2) { /* ... */ } // ok  
```  
5\.  However,** the return type may not be an array type or a function type**. However, **a function may return a pointer to an array or a function**.  
  
6\.  Objects that exist only while a block is executing are known as automatic objects. After execution exits a block, the values of the automatic objects created in that block are undefined. Or in other words, the objects are destoryed.  Parameters are automatic objects.  
  
7\.  **Local static object** is initialized before the first time execution passes through the object’s definition. **Local statics are not destroyed when a function ends; they are destroyed when the program terminates**.  
```  
	size_t count_calls()  
	{  
		static size_t ctr = 0;  // value will persist across calls  
		return ++ctr;  
	}  
	int main()  
	{  
		for (size_t i = 0; i != 10; ++i)  
		cout << count_calls() << endl;  
		return 0;  
}  
```
This program will print the numbers from 1 through 10 inclusive.  Before control flows through the definition of ctr for the first time, ctr is created  and given an initial value of 0. Each call increments ctr and returns its new value. Whenever count_calls is executed, the variable ctr already exists and has whatever value was in that variable the last time the function exited. Thus, on the second invocation, the value of ctr is 1, on the third it is 2, and so on.  
  
If a local static has no explicit initializer, it is value initialized, meaning that local statics of built-in type are initialized to zero.  
  
8\.  Because a function declaration has no body, there is no need for parameter names. Hence, parameter names are often omitted in a declaration. But they can be useful to remind users what the function does.  
 ```
// parameter names chosen to indicate that the iterators denote a range of values to print  
	void print(vector<int>::const_iterator beg,  
	vector<int>::const_iterator end);  
 ``` 
These three elements—the return type, function name, and parameter types—describe  the function’s interface.  

9\.  We can pass either a **const** or a **nonconst** object to a parameter that has a top-level const:  
	``	void fcn(const int i) { /* fcn can read but not write to i */ }  ``
	
**We can call fcn passing it either a const int or a plain int. The fact that top-level consts are ignored on a parameter has one possibly surprising implication**:  
```
void fcn(const int i) { /* fcn can read but not write to i */ }  
void fcn(int i)  // error: redefines fcn(int)  
 ```
  
10\.  In C++, we can define several different functions that have the same name. However, we can do so only if their parameter lists are sufficiently different. Because top-level consts are ignored, we can pass exactly the same types to either version of fcn. The second version of fcn is an error. Despite appearances, its parameter list doesn’t differ from the list in the first version of fcn.  
  
11\.  We can initialize an object with a low-level const from a nonconst object but not vice versa, and a plain reference must be initialized from an object of the same type.  
  ```
	int i = 42;  
	const int *cp = &i; // ok: but cp can't change i (§ 2.4.2 (p. 62))  
	const int &r = i;  // ok: but r can't change i (§ 2.4.1 (p. 61))  
	const int &r2 = 42; // ok: (§ 2.4.1 (p. 61))  
	int *p = cp;  // error: types of p and cp don't match (§ 2.4.2 (p. 62))  
	int &r3 = r;  // error: types of r3 and r don't match (§ 2.4.1 (p. 61))  
	int &r4 = 42; // error: can't initialize a plain reference from a literal (§ 2.3.1)  
```  
12\.  Array Parameters:  
```
// despite appearances, these three declarations of print are equivalent  
// each function has a single parameter of type const int*  
	void print(const int*);  
	void print(const int[]);  // shows the intent that the function takes an array  
	void print(const int[10]); // dimension for documentation purposes (at best)  
  
	int i = 0, j[2] = {0, 1};  
	print(&i); // ok: &i is int*  
	print(j);  // ok: j is converted to an int* that points to j[0]  
```
If we pass an array to print, that argument is automatically converted to a pointer to the first element in the array; the size of the array is irrelevant.  
  
13\.  Using a Marker to Specify the Extent of an Array:  
C-style strings are stored in character arrays in which the last character of the string is followed by a null character. Functions that deal with C-style strings stop processing the array when they see a null character:  
```
	void print(const char *cp)  
	{  
		if (cp)  // if cp is not a null pointer  
		while (*cp)  // so long as the character it points to is not a null character  
		cout << *cp++; // print the character and advance the pointer  
	}  
```  
Using the Standard Library Conventions:  
**pass pointers to the first and one past the last element in the array.  **

```
void print(const int *beg, const int *end)  
	{  
		// print every element starting at beg up to but not including end  
		while (beg != end)  
		cout << *beg++ << endl; // print the current element  
		// and advance the pointer  
	}  
  
	int j[2] = {0, 1};  
	// j is converted to a pointer to the first element in j  
	// the second argument is a pointer to one past the end of j  
	print(begin(j), end(j)); // begin and end functions, see note 22 ;  
  ```
14\.  Array Reference Parameters  
```
// ok: parameter is a reference to an array; the dimension is part of the type  
	void print(int (&arr)[10])  
	{  
		for (auto elem : arr)  
		cout << elem << endl;  
	}  
```  
**The parentheses around &arr are necessary**.  
```
	f(int &arr[10])  // error: declares arr as an array of references  
	f(int (&arr)[10]) // ok: arr is a reference to an array of ten ints  
```  
15\.  **int main(int argc, char \*argv[]) { ... }**  
The second parameter,** argv, is an array of pointers to C-style character strings**. The first parameter, argc, passes the number of strings in that array. Because the second parameter is anint main(int argc, char **argv) { ... } array, we might alternatively define main as  
```
	int main(int argc, char **argv) { ... }  // indicating that argv points to a char*. 　　　
```
16\.  **Functions with Varying Parameters( unknown number of parameters)**  
The new standard provides two primary ways to write a function that takes a varying number of arguments: If all the arguments have the same type, we can pass a library type named initializer\_list. Like a vector, initializer\_list is a template type.When we define an initializer_list, we must specify the type of the elements.  
```
	initializer_list<string> ls; // initializer_list of strings  
	initializer_list<int> li;  // initializer_list of ints  
```  
If the argument types vary, we can write a special kind of function, known as a variadic template.  
  
17\.  **Returning a Pointer to an Array**:  
 ``` 
	typedef int arrT[10];  // arrT is a synonym for the type array of ten ints  
	using arrtT = int[10]; // equivalent declaration of arrT; see § 2.5.1 (p. 68)  
	arrT* func(int i);  // func returns a pointer to an array of five ints  
```
Here arrT is a synonym for an array of ten ints. Because we cannot return an array, we define the return type as a pointer to this type. Thus, func is a function that takes a single int argument and returns a pointer to an array of ten ints.  
  
18\.  To declare func without using a type alias, we must remember that the dimension of an array follows the name being defined:  
```
	int arr[10];  // arr is an array of ten ints  
	int *p1[10];  // p1 is an array of ten pointers  
	int (*p2)[10] = &arr; // p2 points to an array of ten ints  
  ```
19\.  if we want to **define a function that returns a pointer to an array**,** the dimension must follow the function’s name**. However, a function includes a parameter list, which also follows the name. The parameter list precedes the dimension.  
 ``
	Type (*function(parameter_list))[dimension]  
``  
Type is the type of the elements and dimension isthe size of the array. The parentheses around (* function ( parameter_list )) are necessary for the same reason that they were required when we defined p2. Without them, we would be defining a function that returns an array of pointers.  
  
As a concrete example, the following declares func without using a type alias:  
 ``` 
int (*func(int i))[10];  
 ``` 
To understand this declaration, it can be helpful to think about it as follows:  
 
• func(int) says that we can call func with an int argument.  
 
• (*func(int)) says we can dereference the result of that call.  
  
• (*func(int))\[10\] says that dereferencing the result of a call to func yields an array of size ten.  
  
• int (*func(int))\[10\] says the element type in that array is int.  
  
20\.  Under the new standard, another way to simplify the declaration of func is by using a trailing return type. Trailing returns can be defined for any function, but are most useful for functions with complicated return types, such as pointers (or references) to arrays. A trailing return type follows the parameter list and is preceded by ->. To signal that the return follows the parameter list, we use auto where the return type ordinarily appears:  
``` 
// fcn takes an int argument and returns a pointer to an array of ten ints  
	auto func(int i) -> int(*)[10];  
```  
Because the return type comes after the parameter list, it is easier to see that func returns a pointer and that that pointer points to an array of ten ints  

21\.  **Overloaded Functions**:  
Functions that have the **same name** but **different parameter lists** and that **appear in the same scope** are **overloaded**.  
**It is an error for two functions to differ only in terms of their return types**. If the parameter lists of two functions match but the return types differ, then the second declaration is an error:  
```
	Record lookup(const Account&);  
	bool lookup(const Account&); // error: only the return type is different  
```
22\.  Two parameter lists can be identical, even if they don’t look the same:  

```// each pair declares the same function  
	Record lookup(const Account &acct);  
	Record lookup(const Account&); // parameter names are ignored  
  
	typedef Phone Telno;  
	Record lookup(const Phone&);  
	Record lookup(const Telno&); // Telno and Phone are the same type  
```  
In the first pair, the first declaration names its parameter. Parameter names are only a documentation aid. They do not change the parameter list.  
In the second pair, it looks like the types are different, but Telno is not a new type; it is a synonym for Phone.  
  
23\.  Top-level const (§ 2.4.3, p. 63) has no effect on the objects that can be passed to the function. A parameter that has a top-level const is indistinguishable from one without a top-level const:  
```
	Record lookup(Phone);  
	Record lookup(const Phone); // redeclares Record lookup(Phone)  
  
	Record lookup(Phone*);  
	Record lookup(Phone* const); // redeclares Record lookup(Phone*)  
```
In these declarations, the second declaration declares the same function as the first.  
On the other hand, we can overload based on whether the parameter is a reference (or pointer) to the const or nonconst version of a given type; such consts are low-level:  
```
// functions taking const and nonconst references or pointers have different parameters  
// declarations for four independent, overloaded functions  
	Record lookup(Account&); // function that takes a reference to Account  
	Record lookup(const Account&); // new function that takes a const reference  
  
	Record lookup(Account*); // new function, takes a pointer to Account  
	Record lookup(const Account*); // new function, takes a pointer to const  
```
In these cases, the compiler can use the constness of the argument to distinguish which function to call. Because there is no conversion from const, we can pass a const object (or a pointer to const) only to the version with a const parameter.  
  
24\. Note that it is impossible to supply an argument for parameter z without also supplying arguments for parameters x and y. This is because C++ does not support a function call syntax such as printValues(,,3). This has two major consequences:  
  
1) All default parameters must be the rightmost parameters. The following is not allowed:  
void printValue(int x=10, int y); // not allowed  
  
2) If more than one default parameter exists, the leftmost default parameter should be the one most likely to be explicitly set by the user.  
  
> 函数赋默认值必须从右向左，一个形参有默认值的时候，它右边的所有形参都要有默认值，因为传参的时候不允许出现（\*\*,,,\*\*）的形式  
  
25\.  **inline Function**s Avoid Function Call Overhead: In general, the inline mechanism is meant to optimize small, straight-line functions that are called frequently.  
**A function specified as inline (usually) is expanded “in line” at each call**. If shorterString were defined as inline, then this call:  
```  
	cout << shorterString(s1, s2) << endl;  
```  
(probably) would be expanded during compilation into something like:  
```  
//use the whole function body instead of function call  
	cout << (s1.size() < s2.size() ? s1 : s2) << endl;  
 ``` 
26\.  When we define a pointer in a constexpr declaration, the constexpr specifier applies to the pointer, not the type to which the pointer points:  
```  
	const int *p = nullptr;  // p is a pointer to a const int  
	constexpr int *q = nullptr; // q is a const pointer to int  
```  
27\.  A **constexpr** function is **permitted to return a value that is not a constant**:  
```
// scale(arg) is a constant expression if arg is a constant expression  
	constexpr size_t scale(size_t cnt) { return new_sz() * cnt; }  
```  
28\.  **Pointers to Functions**:  
 A function’s type is determined by its return type and the types of its parameters. The function’s name is not part of its type. For example:  
```  
// compares lengths of two strings  
	bool lengthCompare(const string &, const string &);  
```  
has type bool(const string&, const string&). To declare a pointer that can point at this function, we declare a pointer in place of the function name:  
```
// pf points to a function returning bool that takes two const string references  
	bool (*pf)(const string &, const string &);  // uninitialized  
```  
29\.  The parentheses around *pf are necessary. If we omit the parentheses, then we declare pf as a function that returns a pointer to bool:  
```
// declares a function named pf that returns a bool*  
	bool *pf(const string &, const string &);  
```  
30\.  When we use the name of a function as a value, the function is automatically converted to a pointer. For example, we can assign the address of lengthCompare to pf as follows:  
```
	pf = lengthCompare;  // pf now points to the function named lengthCompare  
	pf = &lengthCompare; // equivalent assignment: address-of operator is optional  
```  
31\.  Moreover, we can use a pointer to a function to call the function to which the pointer points. We can do so directly—there is no need to dereference the pointer:  
```
	bool b1 = pf("hello", "goodbye");  // calls lengthCompare  
	bool b2 = (*pf)("hello", "goodbye"); // equivalent call  
	bool b3 = lengthCompare("hello", "goodbye"); // equivalent call  
```
