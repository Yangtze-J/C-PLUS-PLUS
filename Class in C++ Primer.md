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

