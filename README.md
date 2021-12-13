# task3.home_page.

"The const keyword allows you to specify whether or not
a variable is modifiable. You can use const to prevent modifications to variables and const pointers
and const references prevent changing the data pointed to (or referenced)."
Const gives you the ability to document your program more clearly and actually enforce that documentation.
By enforcing your documentation, the const keyword provides guarantees to your users that allow you to make performance optimizations without the threat of damaging their data.
For instance, const references allow you to specify that the data referred to won't be changed;
this means that you can use const references as a simple and immediate way of improving performance for any function that currently takes objects by value without having to worry that your function might modify the data. Even if it does,
the compiler will prevent the code from compiling and alert you to the problem.
On the other hand, if you didn't use const references, you'd have no easy way to ensure that your data wasn't modified. 
  
_**const variables**_ ex: <br>
int main() {<br>
  const int myNum = 15;<br>
  myNum = 10;<br>
  cout << myNum;<br>
  return 0;<br>
}<br>
  <hr>.
_**Const pionter**_
there are two ways of declaring a const pointer: one that prevents you from changing what is pointed to, and one that prevents you from changing the data pointed to.

The syntax for declaring a pointer to constant data is natural enough:<br>

const int *p_int;<br><br>
You can think of this as reading that *p_int is a "const int". So the pointer may be changeable, but you definitely can't touch what p_int points to. The key here is that the const appears before the *.

On the other hand, if you just want the address stored in the pointer itself to be const, then you have to put const after the *:<br><br>
  #include<iostream><br>
int main()<br>
{<br>
    char a ='A';<br>
    char *const ptr = &a;<br>
    printf( "Value pointed to by ptr: %c\n", *ptr);<br>
    printf( "Address ptr is pointing to: %d\n\n", ptr);<br>
}
  <hr><br>
_**Const functions**_<br>
In C++, however, there's the issue of classes with methods. If you have a const object, you don't want to call methods that can change the object, so you need a way of letting the compiler know which methods can be safely called. These methods are called "const functions", and are the only functions that can be called on a const object. Note, by the way, that only member methods make sense as const methods. Remember that in C++, every method of an object receives an implicit this pointer to the object; const methods effectively receive a const this pointer.

The way to declare that a function is safe for const objects is simply to mark it as const; the syntax for const functions is a little bit peculiar
because there's only one place where you can really put the const: at the end of the function:<br>
class Dog{<br>
  string name;<br>
  string breed;<br>
 
public:<br>
  Dog(string name, string breed){<br>
    this->name = name;<br>
    this->breed = breed;<br>
  }<br>
 
  string getName() const{<br>
    return this->name;<br>
  }<br>
 
};<br>
 
int main() {<br>
  const Dog dog("Scooby", "Breed-1");<br>
  cout << dog.getName() << endl;<br>
}<br><br>
Note that just because a function is declared const that doesn't prohibit non-const functions from using it; the rule is this:
Const functions can always be called
Non-const functions can only be called by non-const objects.<br>
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
#include <iostream><br>

template <typename T><br>
class Vector<br>
{<br>
public:<br>

    // Default constructors<br>
    Vector() : mSize{0}, mBuffer{nullptr}<br>
    {<br>
    }<br>
    // Constructors which take more than zero arguments <br>
    explicit Vector(size_t size) : mSize{size}, mBuffer{new T[size]}<br>
    {<br>
        for (int i = 0; i < mSize; i ++)<br>
        {
            mBuffer[i] = 0;<br>
        }
    }<br>
    explicit Vector(std::initializer_list<T> lst) : mSize{lst.size()}, mBuffer{new T[lst.size()]}<br>
    {
        std::copy(lst.begin(), lst.end(), this->mBuffer);<br>
    }
    // Copy constructor<br>
    Vector(const Vector& vec) : mSize{vec.mSize}, mBuffer{new T[vec.mSize]}<br>
    {
        std::copy(vec.mBuffer, vec.mSize, this->mBuffer);<br>
    }
    // Move constructor<br>
    Vector(const Vector&& vec) : mSize{vec.mSize}, mBuffer{vec.mBuffer}<br>
    {
        vec.mSize = 0;<br>
        vec.mBuffer = nullptr;<br>
    }
    // Copy assignment<br>
    Vector& operator=(const Vector& vec)<br>
    {<br>
        delete[] this->mBuffer;<br>
        this->mBuffer = new T[vec.mSize];<br>
        std::copy(vec.mBuffer, vec.mSize, this->mBuffer);<br>
        this->mSize = vec.mSize;v
        return *this; // This line of code is not required<br>
    }<br>
    // Move assignment<br>
    Vector& operator=(const Vector&& vec)<br>
    {<br>
        delete[] this->mBuffer;<br>
        this->mBuffer = vec.mBuffer;<br>
        this->mSize = vec.mSize;<br>
        vec.mSize = 0;<br>
        return *this; // This line of code is not required<br>
    }<br>
    // Destructor<br>
    ~Vector()<br>
    {<br>
        delete[] this->mBuffer;<br>
    }<br>
    // [] operator<br>
    T& operator[](int n)<br>
    {<br>
        std::cout << "Non-const operator [] called." << std::endl;<br>
        return this->mBuffer[n];<br>
    }
    // [] operator const overloaded<br>
    const T& operator[](int n) const<br>
    {
        std::cout << "Const operator [] called." << std::endl;<br>
        return this->mBuffer[n];<br>
    }
    // Public methods<br>
    size_t size() const<br>
    {
        return this->mSize;<br>
    }
    // For non-const typed instance, we return normal pointers<br>
    T* data()<br>
    {
        std::cout << "Non-const pointer returned." << std::endl;<br>
        return this->mBuffer;<br>
    }
    // const overloaded<br>
    // For const typed instance, we return const pointers to prevent modifying the instance<br>
    const T* data() const<br>
    {
        std::cout << "Const pointer returned." << std::endl;<br>
        return this->mBuffer;<br>
    }

private:

    size_t mSize;<br>
    T* mBuffer;<br>
};<br>

template <typename T><br>
void printConstVector(const Vector<T>& vec)<br>
{<br>
    for (int i = 0; i < vec.size(); i ++)<br>
    {
        std::cout << vec[i] << " ";<br>
    }
    std::cout << std::endl;<br>
}

template <typename T><br>
void printNonConstVector(Vector<T>& vec)<br>
{
    for (int i = 0; i < vec.size(); i ++)<br>
    {<br>
        std::cout << vec[i] << " ";<br>
    }<br>
    std::cout << std::endl;<br>
}<br>

int main()<br>
{<br>
    Vector<int> vec{1,2,3};<br>
    printConstVector(vec);<br>
    printNonConstVector(vec);<br>

    std::cout << "------------------------" << std::endl;<br>

    const Vector<int> constVec{1,2,3};<br>
    printConstVector(constVec);<br>

    std::cout << "------------------------" << std::endl;<br>

    int* pVec = vec.data();<br>

    std::cout << "------------------------" << std::endl;<br>

    pVec[0] = 4;<br>
    pVec[1] = 5;<br>
    pVec[2] = 6;<br>
    printNonConstVector(vec);v<br>

    std::cout << "------------------------" << std::endl;<br>

    const int* pConstVec = constVec.data();<br>
}
  <hr><br>
_**Const iterators**_<br>
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
}
  <hr><br>

Const casts look like regular typecasts in C++, except that they can only be used for casting away constness (or volatile-ness)
but not converting between types or casting down a class hierarchy.
  <hr>

_**usages of & operator:**_<br>
The & operator yields a pointer to its operand. The operand must be a lvalue, a function designator, or a qualified name.
It cannot be a bit field, nor can it have the storage class register.
If the operand is a lvalue or function, the resulting type is a pointer to the expression type.
For example, if the expression has type int, the result is a pointer to an object having type int.
  <hr>

_**Use & to declare a reference to a type.**_<br>
If you use & in the left-hand side of a variable declaration, it means that you expect to have a reference to the declared type.
It can be used in any type of declarations (local variables, class members, method parameters).
<br>std::string mrSamberg("Andy");<br>
std::string& theBoss = mrSamberg
  <hr><br>

_**Use & to get the address of a variable**_<br>
The meaning of & changes if you use it in the right-hand side of an expression. In fact, if you use it on the left-hand side, it must be used in a variable declaration,
on the right-hand side, it can be used in assignments too.

When using it on the right-hand side of a variable, it's also known as the "address-of operator".
Not surprisingly if you put it in front of a variable, it'll return its address in the memory instead of the variable's value itself. It is useful for pointer declarations.<br>
#include<iostream><br>
using namespace std;<br>
  
int main()<br>
{
  int x = 10;<br>
  
  // ref is a reference to x.<br>
  int& ref = x;<br>
  
  // Value of x is now changed to 20<br>
  ref = 20;<br>
  cout << "x = " << x << endl ;<br>
  
  // Value of x is now changed to 30<br>
  x = 30;<br>
  cout << "ref = " << ref << endl ;<br>
  
  return 0;<br>
}
  <br><hr>
  _**Use & as a bitwise operator**_<br>
It is the bitwise AND. Its an infix operator taking two numbers as inputs and doing an AND on each of the bit pairs of the inputs.
Here is an example. 14 is represented as 1110 as a binary number and 42 can be written as 101010.
So 1110 (14) will be zero filed from the left.<br>
  #include <iostream><br>
using namespace std;<br>

int main() {<br>
    // declare variables<br>
    int a = 12, b = 25;<br>

    cout << "a = " << a << endl;<br>
    cout << "b = " << b << endl;<br>
    cout << "a & b = " << (a & b) << endl;<br>

    return 0;<br>
}<br>
  <hr>
_**Use && in a logical expression**_<br>
&& in a (logical) expression is just the C-style way to say and<br>
  #include <iostream><br>
using namespace std;<br>

int main() {<br>
    int a = 5;<br>
    int b = 9;<br>
  
    // false && false = false<br>
    cout << ((a == 0) && (a > b)) << endl;<br>
  
    // false && true = false<br>
    cout << ((a == 0) && (a < b)) << endl;<br>

    // true && false = false<br>
    cout << ((a == 5) && (a > b)) << endl;<br>

    // true && true = true<br>
    cout << ((a == 5) && (a < b)) << endl;<br>

    return 0;<br>
}
  <br><hr>
_**Use && for declaring rvalue references**_
<br>auto mrSamberg = std::string{"Andy"};<br><br>
mrSamberg represents an lvalue. It points to a specific place in the memory which identifies an object.
On the other hand, what you can find on the right side <br>std::string{"Andy"} <br>is actually an rvalue.
It's an expression that can't have a value assigned to, that's already the value itself.
It can be only on the right-hand side of an assignment operator.
  <hr>

_**Use & or && for function overloading**_<br>
Since C++11 you can use both the single and double ampersands as part of the function signature,
but not part of the parameter list. If I'm not clear enough, let me give the examples:
<br>void doSomething() &;<br>
void doSomething() &&;<br>
auto doSomethingElse() & -> int;<br>
auto doSomethingElse() && -> int;<br>
