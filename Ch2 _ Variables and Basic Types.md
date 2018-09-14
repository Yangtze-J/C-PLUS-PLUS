## Chapter 2. Variables and Basic Types

### 2.3  Compound types
#### Reference
1\.  There is no way to rebind a reference to refer to a different object, so **references must be initialized**.
```
	int a = 1, c = 2;
	int &b = a; //ok, b is a reference to a
	&b = c; //wrong, after initialization, b can never have '&' on its left side, so no rebind.
```
2\.  A reference is not an object. Instead, a reference is another name for an already existing object.

#### Pointers
1\. The * must be repeated for each pointer variable:
```
	int *ip1, *ip2; // both ip1 and ip2 are pointers to int
	double dp, *dp2; // dp2 is a pointer to double; dp is a double
```
2\.  Because ***references are not objects, they don’t have addresses.*** Hence, we may not define a pointer to a reference.

3\. We can use the dereference operator (the * operator) to access the object. Dereferencing a pointer yields the object to which the pointer points. We can assign to that object by assigning to the result of the dereference:
```
	*p = 0; // * yields the object; we assign a new value to ival through p
	cout << *p; // prints 0
```
***When we assign to \*p, we are assigning to the object to which p points.***

4\. There are several ways to obtain a **null pointer**:
```
	int *p1 = nullptr; // equivalent to int *p1 = 0;
	int *p2 = 0; // directly initializes p2 from the literal constant 0
	// must #include cstdlib
	int *p3 = NULL; // equivalent to int *p3 = 0
```
***nullptr*** is a literal that has a special type that can be converted to any other pointer type.
***NULL is a preprocessor variable*** which the cstdlib header defines as 0. 
>The preprocessor is a program that runs before the compiler. Preprocessor variables are managed by the preprocessor, and are not part of the ***std*** namespace. 

It is illegal to assign an int variable to a pointer.
```
	int zero = 0;
	pi = zero; // error: cannot assign an int to a pointer
```
5\.  **void\*  pointers**
The type void* is a special pointer type that can hold the address of any object. Like any other pointer, a void* pointer holds an address, but the type of the object at that address is unknown:
```
	double obj = 3.14, *pd = &obj;
	// ok: void* can hold the address value of any data pointer type
	void *pv = &obj; // obj can be an object of any type
```
We can assign it to another void* pointer.  **We cannot use a void\* to operate on the object it addresses**—we don’t know that object’s type
>Generally, we use a void* pointer to deal with memory as memory, rather than using the pointer to access the object stored in that memory. 

6\.  we can put white-space between the type modifier( * and & ) and the name being declared:
```
	int* p1, p2; // p1 is a pointer to int; p2 is an int
```
It's better to **place the \* (or the &) with the variable name**.


7\.  **Pointers to pointers**
We indicate each pointer level by its own \*. That is, we write \*\*  for a pointer to a pointer, \*** for a pointer to a pointer to a pointer, and so on:
```
	int ival = 1024;int *pi = &ival; 	// pi points to an int
	int **ppi = &pi; 					// ppi points to a pointer to an int
	// Here pi is a pointer to an int and ppi is a pointer to a pointer to an int. 
	// ppi -> pi ->ival
```
8\.  **References to Pointers**
A reference is not an object. Hence, we may not have a pointer to a reference. However, because a pointer is an object, we can define a reference to a pointer:
```
	int i = 42;
	int *p; 		// p is a pointer to int
	int *&r = p; 	// r is a reference to the pointer p
	r = &i; 		// r refers to a pointer; assigning &i to r makes p point to i
	*r = 0; 		// dereferencing r yields i, the object to which p points; changes i to 0
```
**The easiest way to understand the type of r is to read the definition right to left. The symbol closest to the name of the variable (in this case the & in &r) is the one that has the most immediate effect on the variable’s type.** 
#### 2.4  const Qualifier
1\.  We can make a variable unchangeable by defining the variable’s type as **const**.

2\. When we use an object to initialize another object, it doesn’t matter whether either or both of the objects are consts:
```
	int i = 42;
	const int ci = i; 	// ok: the value in i is copied into ci
	int j = ci; 		// ok: the value in ci is copied into j
```
3\. **References to const**
```
	const int ci = 1024;
	const int &r1 = ci; // ok: both reference and underlying object are const
```
 References to const objects must be const themselves.
 
 4\. Pointers and const
 **pointers to non-const  cannot point to const objects;**
```
	const double pi = 3.14; 	// pi is const; its value may not be changed
	double *ptr = &pi; 			// error: ptr is a plain pointer
```
  **pointers to const can point to non-const objects.**
```
	const double *cptr = &pi; 
	double dval = 3.14; 		// dval is a double; its value can be changed
	cptr = &dval;				// ok: but can't change dval through cptr
```
5\. A const pointer must be initialized, and once initialized, its value (i.e., the address that it holds) may not be changed. We indicate that the pointer is const by putting the const after the \*. 
```
	int errNumb = 0;
	int *const curErr = &errNumb; 	 // curErr will always point to errNumb
	const double pi = 3.14159; 
	const double *const pip = &pi;	 // pip is a const pointer to a const object
```
6\. **Top- level const** 
For pointers, We use the term top-level const to indicate that the pointer itself is a const. When a pointer can point to a const object, we refer to that const as a low-level const.
>More generally, **top-level const** indicates that an object itself is const. 
>**Low-level const** appears in the base type of compound types such as pointers or references. 

The distinction between top-level and low-level matters when we copy an object. When we copy an object, top-level consts are ignored:
```
	const int ci = 42; 			// we cannot change ci; const is top-level
	const int *p2 = &ci; 		// we can change p2; const is low-level
	const int *const p3 = p2; 	// right-most const is top-level, left-most is not
	
	// the fact that p3 itself is a const is ignored.
	p2 = p3;    // ok: pointed-to type matches; top-level const in p3 is ignored
```  
On the other hand, low-level const is never ignored. When we copy an object, both objects must have the same low-level const qualification or there must be a conversion between the types of the two objects. In general, we can convert a non-const to const but not the other way round:
```
	int i = 0;
	int *p = p3; 	// error: p3 has a low-level const but p doesn't
	p2 = p3; 		// ok: p2 has the same low-level const qualification as p3
	p2 = &i; 		// ok: we can convert int* to const int*
	int &r = ci; 	// error: can't bind an ordinary int& to a const int object
	const int &r2 = i; // ok: can bind const int& to plain int
```
7\.  **constexpr** : Variables declared as constexpr are implicitly const and must be initialized by constant
expressions.
**Pointers and constexpr**
when we define a pointer in a constexpr declaration, the constexpr specifier applies to the pointer, not the type to which the pointer points:
```
	const int *p = nullptr; // p is a pointer to a const int
	constexpr int *q = nullptr; // q is a const pointer to int
```
#### 2.5 Dealing with types
1\. type alias
```
	typedef  a  b    // use b to represent a
	using   a = b;   // use a to replace b
```
2\.  **The decltype Type Specifier**
The compiler analyzes the expression to determine its type but does not evaluate the expression.
```
	decltype(f()) sum = x; // sum has whatever type f returns
```
The compiler does not call f, but it uses the type that such a call would return as the type for sum. 
**When the expression to which we apply decltype is a variable,** decltype returns the type of that variable, including top-level const and references:
```
	const int ci = 0, &cj = ci;
	decltype(ci) x = 0; 	    // x has type const int
	decltype(cj) y = x;			// y has type const int& and is bound to x
	decltype(cj) z; 			// error: z is a reference and must be initialized
```
Some expressions will cause decltype to yield a reference type
```
	int i = 42, *p = &i, &r = i;
	decltype(r + 0) b; 	 	// ok: addition yields an int; b is an (uninitialized) int
	decltype(*p) c; 		// error: c is int& and must be initialized
``` 
The type deduced by decltype(\*p) is int&, not plain int.
```
	// decltype of a parenthesized variable is always a reference
	decltype((i)) d; 		// error: d is int& and must be initialized
	decltype(i) e;			// ok: e is an (uninitialized) int
```
>Remember that **decltype((variable)) (note, double parentheses) is always a reference type**, 
>but decltype(variable) is a reference type only if variable is a reference.
