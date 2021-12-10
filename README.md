# task3.home_page.
<h3>_**usages of const keyword:**_<h3>
"The const keyword allows you to specify whether or not
a variable is modifiable. You can use const to prevent modifications to variables and const pointers
and const references prevent changing the data pointed to (or referenced)."
Const gives you the ability to document your program more clearly and actually enforce that documentation.
By enforcing your documentation, the const keyword provides guarantees to your users that allow you to make performance optimizations without the threat of damaging their data.
For instance, const references allow you to specify that the data referred to won't be changed;
this means that you can use const references as a simple and immediate way of improving performance for any function that currently takes objects by value without having to worry that your function might modify the data. Even if it does,
the compiler will prevent the code from compiling and alert you to the problem.
On the other hand, if you didn't use const references, you'd have no easy way to ensure that your data wasn't modified.<hr>
  
_**const variables**_ ex: const int number; <br>
  <hr>.
_**Const pionter**_
there are two ways of declaring a const pointer: one that prevents you from changing what is pointed to, and one that prevents you from changing the data pointed to.

The syntax for declaring a pointer to constant data is natural enough:<br>

const int *p_int;<br><br>
You can think of this as reading that *p_int is a "const int". So the pointer may be changeable, but you definitely can't touch what p_int points to. The key here is that the const appears before the *.

On the other hand, if you just want the address stored in the pointer itself to be const, then you have to put const after the *:<br><br>
int x;<br>
int * const p_int = &x;<hr><br>
_**Const functions**_<br>
In C++, however, there's the issue of classes with methods. If you have a const object, you don't want to call methods that can change the object, so you need a way of letting the compiler know which methods can be safely called. These methods are called "const functions", and are the only functions that can be called on a const object. Note, by the way, that only member methods make sense as const methods. Remember that in C++, every method of an object receives an implicit this pointer to the object; const methods effectively receive a const this pointer.

The way to declare that a function is safe for const objects is simply to mark it as const; the syntax for const functions is a little bit peculiar
because there's only one place where you can really put the const: at the end of the function:<br>
<return-value> <class>::<member-function>(<args>) const<br>
{<br>
 ...
}<br>
For instance,
int Loan::calcInterest() const<br>
{<br>
return loan_value * interest_rate;<br>
}<br><br>
Note that just because a function is declared const that doesn't prohibit non-const functions from using it; the rule is this:
Const functions can always be called
Non-const functions can only be called by non-const objects<hr>
_**Const Overloading**_<br>
In large part because const functions cannot return non-const references to an objects' data, there are many times where it might seem appropriate to have both const and non-const versions of a function.
For instance, if you are returning a reference to some member data (usually not a good thing to do, but there are exceptions), then you may want to have a non-const version of the function that returns a non-const reference:<br>
int& myClass::getData()<br>
{<br>
return data;<br>
}<br>
On the other hand, you don't want to prevent someone using a const version of your object,

myClass constDataHolder;
from getting the data. You just want to prevent that person from changing it by returning a const reference. But you probably don't want the name of the function to change just because you change whether the object is const or not--among other things, this would mean an awful lot of code might have to change just because you change how you declare a variable--going from a non-const to a const version of a variable would be a real headache.

Fortunately, C++ allows you to overload based on the const-ness of a method. So you can have both const and non-const methods, and the correct version will be chosen. If you wish to return a non-const reference in some cases, you merely need to declare a second, const version of the method that returns a const method:<br>
// called for const objects only since a non-const version also exists<br>
const int& myData::getData() const<br>
{<br>
return data;<br>
}<hr><br>
_**Const iterators**_
As we've already seen, in order to enforce const-ness, C++ requires that const functions return only const pointers and references.
Since iterators can also be used to modify the underlying collection,
when an STL collection is declared const, then any iterators used over the collection must be const iterators.
They're just like normal iterators, except that they cannot be used to modify the underlying data.
(Since iterators are a generalization of the idea of pointers, this makes sense.)

Const iterators in the STL are simple enough: just append "const_" to the type of iterator you desire.
For instance, we could iterator over a vector as follows:<br>
std::vector<int> vec;<br>
vec.push_back( 3 );<br>
vec.push_back( 4 );<br>
vec.push_back( 8 );<br>

for ( std::vector<int>::const_iterator itr = vec.begin(), end = vec.end();<br>
itr != end;<br>
++itr )<br>
{<br>
// just print out the values...<br>
std::cout<< *itr <<std::endl;<br>
}<hr><br>

Const casts look like regular typecasts in C++, except that they can only be used for casting away constness (or volatile-ness)
but not converting between types or casting down a class hierarchy.<hr>

_**usages of & operator:**_<br>
The & operator yields a pointer to its operand. The operand must be a lvalue, a function designator, or a qualified name.
It cannot be a bit field, nor can it have the storage class register.
If the operand is a lvalue or function, the resulting type is a pointer to the expression type.
For example, if the expression has type int, the result is a pointer to an object having type int.<hr>

_**Use & to declare a reference to a type.**_<br>
If you use & in the left-hand side of a variable declaration, it means that you expect to have a reference to the declared type.
It can be used in any type of declarations (local variables, class members, method parameters).
<br>std::string mrSamberg("Andy");<br>
std::string& theBoss = mrSamberg<hr><br>

_**Use & to get the address of a variable**_<br>
The meaning of & changes if you use it in the right-hand side of an expression. In fact, if you use it on the left-hand side, it must be used in a variable declaration,
on the right-hand side, it can be used in assignments too.

When using it on the right-hand side of a variable, it's also known as the "address-of operator".
Not surprisingly if you put it in front of a variable, it'll return its address in the memory instead of the variable's value itself. It is useful for pointer declarations.
<br>std::string mrSamberg("Andy");<br>
std::string* theBoss;<br>
theBoss = &mrSamberg;<br><hr>
  _**Use & as a bitwise operator**_<br>
It is the bitwise AND. Its an infix operator taking two numbers as inputs and doing an AND on each of the bit pairs of the inputs.
Here is an example. 14 is represented as 1110 as a binary number and 42 can be written as 101010.
So 1110 (14) will be zero filed from the left.<hr>
_**Use && in a logical expression**_<br>
&& in a (logical) expression is just the C-style way to say and.<br>
_**Use && for declaring rvalue references**_
<br>auto mrSamberg = std::string{"Andy"};<br><br>
mrSamberg represents an lvalue. It points to a specific place in the memory which identifies an object.
On the other hand, what you can find on the right side <br>std::string{"Andy"} <br>is actually an rvalue.
It's an expression that can't have a value assigned to, that's already the value itself.
It can be only on the right-hand side of an assignment operator.<hr>

_**Use & or && for function overloading**_<br>
Since C++11 you can use both the single and double ampersands as part of the function signature,
but not part of the parameter list. If I'm not clear enough, let me give the examples:
<br>void doSomething() &;<br>
void doSomething() &&;<br>
auto doSomethingElse() & -> int;<br>
auto doSomethingElse() && -> int;<br>
