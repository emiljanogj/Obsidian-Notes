- Assume we have the following class definitions in C++

```cpp
class Execution {}; // Parent Class
class ExecuteCommand: public Execution {   // Child of Execution Class
public:
int y;
   virtual void exec(const char *program) 
   {
       system(program);
   }
};
class ExecuteGreet: public Execution {   // Child of Execution Class
public:
int x;
   virtual void greet(const char *str) 
   {
       cout << str << endl;
   }
};

int main() {
   Execution *baseClass2 = new ExecuteCommand;
   ExecuteGreet *gPointer;
   
   gPointer = static_cast<ExecuteGreet*>(baseClass2); // Unsafe Casting to sibling class "ExecuteCommand"
   gPointer->greet("/usr/bin/bash"); // String passed to exec() function which will turn into a command to execute a bash terminal
                                   
   delete baseClass1;
   delete baseClass2;
   return 0;
}
```

Note the following line, which converts the `baseClass2` of type `ExecuteCommand` to `gPointer` of type `ExecuteGreet`:
```cpp
gPointer = static_cast<ExecuteGreet*>(baseClass2); // Unsafe Casting to sibling class "ExecuteCommand"
```
This line simply move the pointer `gPointer` to the `baseClass2` address as shown below:
![[Pasted image 20230612173917.png]]
Note that the static casting above is allowed because of the class hierarchy(common parent) and because `static_cast` doesn't do runtime checks, which can be good for performance but bad for security. 
From the above picture, we see that when we execute:
```cpp
gPointer -> greet("/bin/bash")
```
instead of the expected output(simply printing "/bin/bash"), we get a shell since the `exec` function is executed instead.

#### Real-world Example

### Apache Xerces-C++

  
Apache Xerces-C++ had a type confusion vulnerability located in `xercesc/dom/imple/DOMCasts.hpp`. The vulnerability arises when an object is C-style cast to another object that isn't a sub-object (parent) of the original object (otherwise known as a down-cast).
```cpp
static inline DOMNodeImpl *castToNodeImpl(const DOMNode *p) {
    DOMElementImpl *pE = (DOMElementImpl *) p;
    return &(pE->fNode);
}
```