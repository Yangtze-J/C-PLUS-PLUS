# Class in C++ Primer

## A tiny example
A revised version of Sales_data:	
```
struct Sales_data {
    // new members: operations on Sales_data objects
    std::string isbn() const { return bookNo; }
    Sales_data& combine(const Sales_data&);
    double avg_price() const;
    // data members are unchanged from § 2.6.1 (p. 72)
    std::string bookNo;
    unsigned units_sold = 0;
    double revenue = 0.0;
};
// nonmember Sales_data interface functions
Sales_data add(const Sales_data&, const Sales_data&);
std::ostream &print(std::ostream&, const Sales_data&);
std::istream &read(std::istream&, Sales_data&);
```
And a program:
```
Sales_data total;         // variable to hold the running sum
if (read(cin, total))  {  // read the first transaction
    Sales_data trans;     // variable to hold data for the next transaction
    while(read(cin, trans)) {      //  read the remaining transactions
        if (total.isbn() == trans.isbn())   // check the isbns
            total.combine(trans);  // update the running total
        else {
            print(cout, total) << endl;  // print the results
            total = trans;               // process the next book
        }
    }
    print(cout, total) << endl;          // print the last transaction
} else {                                 // there was no input
    cerr << "No data?!" << endl;         // notify the user
}
```

>**Note:**
>Functions defined in the class are implicitly inline.

### Introducing  this
Let’s look again at a call to the isbn member function:
 ``
	total.isbn()
``
When isbn refers to members of Sales_data (e.g., bookNo), it is referring implicitly to the members of the object on which the function was called. In this call, when isbn returns bookNo, it is implicitly returning total.bookNo
Member functions access the object on which they were called through an extra, implicit parameter named **this**. When we call a member function, this is initialized with the address of the object on which the function was invoked. For example, when we call
```
	total.isbn()
```
the compiler passes the address of total to the implicit this parameter in isbn. It is as if the compiler rewrites this call as
```
// pseudo-code illustration of how a call to a member function is translated
	Sales_data::isbn(&total)
```
which calls the isbn member of Sales_data passing the address of total.
Inside the body of a member function, we can use this. It would be legal, although unnecessary, to define isbn as
```
	std::string isbn() const { return this->bookNo; }
```
Because this is intended to always refer to “this” object, this is a const pointer (§ 2.4.2, p. 62). We cannot change the address that this holds.

### Introducing const Member Functions
**this** is implicit and does not appear in the parameter list. There is no place to indicate that **this** should be a pointer to const. The language resolves this problem by letting us put const after the parameter list of a member function. 
A const following the parameter list indicates that this is a pointer to const. Member functions that use const in this way are **const member functions**.
We can think of the body of isbn as if it were written as
```
// pseudo-code illustration of how the implicit this pointer is used
// this code is illegal: we may not explicitly define the this pointer ourselves
// note that this is a pointer to const because isbn is a const member
	std::string Sales_data::isbn(const Sales_data *const this)
		{ return this->isbn; }
``` 
The fact that this is a pointer to const means that **const member functions cannot change the object on which they are called.** Thus, isbn may read but not write to the data members of the objects on which it is called.

### Class Scope and Member Functions
Recall that **a class is itself a scope**(§ 2.6.1, p. 72).
It is worth noting that isbn can use bookNo even though bookNo is defined after isbn.  
___________________
The **compiler processes classes** in **two steps**— the member declarations are compiled first, after which the member function bodies, if any, are processed.
___________________

#### Defining a Member Function outside the Class
When define a member function outside the class body, the member’s definition must match its declaration.

The name of a member defined outside the class must include the name of the class of which it is a member:
```
	double Sales_data::avg_price() const {
	    if (units_sold)
	        return revenue/units_sold;
	    else
	        return 0;
}
```
#### Defining a Function to Return “This” Object
```
	Sales_data& Sales_data::combine(const Sales_data &rhs)
	{
	    units_sold += rhs.units_sold; // add the members of rhs into
	    revenue += rhs.revenue;       // the members of ''this'' object
	    return *this; // return the object on which the function was called
	}
	....
	total.combine(trans); // update the running total
```
Here the return statement dereferences this to obtain the object on which the function is executing. That is, for the call above, we return a reference to total.

### Defining Nonmember Class-Related Functions
> Ordinarily, nonmember functions that are part of the interface of a class should be declared in the same header as the class itself.
```
// input transactions contain ISBN, number of copies sold, and sales price
	istream &read(istream &is, Sales_data &item)
	{
	    double price = 0;
	    is >> item.bookNo >> item.units_sold >> price;
	    item.revenue = price * item.units_sold;
	    return is;
	}
	ostream &print(ostream &os, const Sales_data &item)
	{
	    os << item.isbn() << " " << item.units_sold << " "
	       << item.revenue << " " << item.avg_price();
	    return os;
	}
```
First, both read and write take a reference to their respective IO class types.  **The IO classes are types that cannot be copied, so we may only pass them by reference.**

The second thing to note is that print does not print a newline. Ordinarily, **functions that do output should do minimal formatting**.

### Constructors
The job of a constructor is to initialize the data members of a class object. A constructor is run whenever an object of a class type is created.

1\.  **Constructors** have **no return type**.
2\. Constructors have a (possibly empty) parameter list and a (possibly empty) function body.
3\. **A class** can have **multiple constructors**.
4\.Like any other **overloaded function** (§ 6.4, p. 230), the **constructors** must **differ ** from each other in the number or types of their parameters.
_________________________________
When we create a const object of a class type, the object does not assume its “constness” until after the constructor completes the object’s initialization. Thus, constructors can write to const objects during their construction.>
_________________________________

If our class **does not explicitly define** any constructors, **the compiler will implicitly define** the **default constructor**.

The compiler-generated constructor is known as the synthesized default constructor. For most classes, this synthesized constructor initializes each data member of the class as follows: 
	• If there is an in-class initializer (§ 2.6.1, p. 73), use it to initialize the member. 
	• Otherwise, default-initialize (§ 2.2.1, p. 43) the member.

#### What = default Means
Constructor:  An empty parameter list (i.e., the default constructor)
```
	Sales_data() = default;
```
Under the new standard, if we want the default behavior, we can ask the compiler to generate the constructor for us by writing **= default** after the parameter list.
