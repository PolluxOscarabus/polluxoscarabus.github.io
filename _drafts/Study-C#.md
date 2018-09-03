---
layout: post
title: Study of C# programming language
---

## References
[C# documentation](https://docs.microsoft.com/en-us/dotnet/csharp/csharp)
[Clean unmanaged resources](https://docs.microsoft.com/en-us/dotnet/standard/garbage-collection/unmanaged)

---
# Draft section

# C# Concepts

## Types
* every type derives from the *object* class type
  * this unified type hierarchy is called the *Common Type System*
  * values of reference types are treated as *objects* by viewing the value as *object*
  * values of vlue types are treated as *objects* by performing *boxing* and *unboxing*
    * during *boxing*, an *object* instance is allocated to hold the value that is then copied into it 
    * during *unboxing*, the *object* instance is cast to a value type 
* two kinds of types : value types and reference types
  * values types can be *simple*, *enum*, *struct*, and *nullable* types
    * *structs* and *enums* are the only two distinct categories of value types
      * Built-in numeric types are structs that have properties and methods
    * *structs* don't support user-specified inheritance because they can only inherit from *ValueType* type 
      * classes can't inherit from a struct
      * within a struct, fields cannot be initialized unless being *static* or *const*
      * structs can't declare default constructors without parameters nor finalizers
    * *nullable* types are values of other types that can additionally hold the *null* value
      * the nullabe equivalent of a type *T* would be declared as *T?*
    * value types derive from *System.ValueType*
      * value types are sealed, a type can't derive from them
    * the memory containing the value of the variable is allocated inline in the context the variable
      * no separate heap allocation or garbage collection overhead are involved
  * references types can be *class*, *interface*, *array*, and *delegate* types
    * the memory is allocated on the managed heap
      * types on the managed heap require overhead both during allocation and deallocation
    * *classes* support single inheritance and polymorphism, and can also implement multiple interfaces
    * *interfaces* may inherit from multiple base interfaces
    * *delegates* represents references to methods with a particular parameter list and return type
      * can be assigned to variables and passed as parameter
* *classes*,*interfaces*,*delegates*,*enums*, and *structs* are user-defined types
* *classes*,*interfaces*,*structs*, and *delegates* all support generics
* *Literal* values are assigned a type from the compiler by inference or from the letter suffix specifying a particular type
* kinds of *type conversions*
  * *implicit conversions* are type safe and no data are lost during the conversion
    * conversions from smaller to larger integral types
    * conversions from derived classes to base classes since derived classes contain every members of its base
    * conversion from a numeric value to its nullable equivalent can be done implicitly
    ```
    int a = 12;
    int? b = a;
    ```
  * *explicit conversions* requiring a cast operator are used when information can be lost during the conversion
    * numeric conversion to a type with less precision while acknowledging the potential loss
    * conversion from a base class to a derived class change only the type of the reference
      * an exception is thrown if the conversion can't be done
  * *user-defined conversions* are defined by methods to enable explicit and implicit conversion between custom types without base-derived class relationship
  * *conversions using helper classes* are made using standard library classes used for conversion of non-compatible types
    * for example, using the *System.BitConverter* class or the *Parse* methods of the built-in numeric types
* An invalid cast can be undetected by the compiler, it will fail at runtime while throwing an InvalidCastException
* the *is* and *as* operators can be used to test the success of a cast without throwing an exception
  * the *as* operator returns the cast value if the cast is successful and null otherwise
  * the *is* operator returns a Boolean value
* *Boxing* is the process of converting a value type to the type *object* or any interface this type implements
  * the value type wrapped inside a *System.Object* is stored in the heap 
    * the object reference is created on the stack and reference a value on the heap of the type boxed
  * *Boxing* is implicit while *Unboxing* is explicit
  ```
  int a = 3;
  object o = a; // boxing
  a = (int)o;   // unboxing
  ```
  * *Boxing* can be used to allow a function or collection (list) to receive all type of arguments boxed into a *System.Object*
  * *Boxing* is a computationally expensive process
    * the use of Generic collections or methods should be preferred
  * *Unboxing* to a instance variable of a different type than the boxed value type would throw an *InvalidCastException*
    * implicit conversion are not done during the casting
    ```
    int a = 23;
    object o = a; // boxing
    long b = (long)o; // thrown an InvalidCastException
    ```
* the *dynamic* type is a static type bypassing static type checking during compile time
  * errors are caught at runtime
  * operations on dynamic type value return dynamic value if no implicit conversion are possible
  ```
  dynamic a = 1;
  var b = a + 1; // b is a dynamic type value
  int c = a + 1; // c is an int, a static type value, a is implicitly converted to an int
  a = c;         // a is still a dynamic type value, c is implicitly converted to an dynamic type
  ```
  * *Overload resolution* occurs at runtime when one of the argument or the variable receiving the returned value is a dynamic type
    * *Exceptions* are thrown at runtime if an invalid type is passed or returned
* *Custom Dynamic Object* can be created by inheriting from the *DynamicObject* class 
* *System.BitConverter*  class can be used to convert an array of bytes to numeric types
  ```
  byte[] bytes = {0,0,1,0};
  int i = BitConverter.ToInt32(bytes,0); // returns 256 from the binary 0000 0000 0001 0000
  bytes = BitConverter.GetBytes(i);
  Console.WriteLine(bytes); // display 00-00-01-00
  ```
* *string* types can be converted to numeric types using the *System.Convert* class or the *TryParse* method of the numeric types
  * *TryParse* methods are more efficient in most cases while *System.Convert* is used for general object implementing *IConvertible* 
  * *Parse* throws an exception and *TryParse* returns a bool
    * the ouput argument taken by *TryParse* will be modified regardless of the success of the conversion
    ```
    int a = 12;
    bool parsed = Int32.TryParse("abc", out a);
    Console.WriteLine(a); // prints 0
    ```
    * *Parse* can receive a member of the *System.Globalization.NumberStyles* enum to indicate the base of the number to convert to
    ```
    // convert hexadecimal and prints 4351
    int a = Int32.Parse("10FF", System.Globalization.NumberStyles.HexNumber); 
    ```
  * *Parse* and *TryParse* ignore leading and trailing whitespace
* *Generic types* are declared with one or more *type parameters* serving as placeholder for the actual type
  * type arguments must be provided during the definition of an instance of a generic class
    ```
    public class MyClass<Tone, Ttwo>
    {
      public Tone one;
      public Ttwo two;
    }
  
    MyClass<string, int> myclass = new MyClass<string, int>{ one = "bob", two = 12}
    ```
  * Generic methods don't need to receive type arguments as they can often be inferred from the arguments of the method

## Namespaces
* the *using* directive allows access to the types defined in a namespace without specifying their fully qualified names
* an *alias* for a namespace can be created with the *using* directive
  ```
  using Name = MyNamespace.MySubNamespace;
  ```
* the *::* keyword is used to reference a namespace alias
```
using alias = N1;
...
alias::N2.MyClass a = new alias::N2.MyClass();
```
* Namespaces declares outside of an explicitly declared namespace are members of the *global* namespace
  ```
  namespace N1
  {
    class MyClass
    {
      public static void Main(string[] args)
      {
        global::N2.MyClass a = new global::N2.MyClass(); // could also use simply N2.MyClass
      }
    }
  }
  namespace N2
  {
    class MyClass{}
  }
  ```
* several classes can have the same name as long as their fully qualified name is different

## Tuples
* The Nuget package *System.ValueTuple* must be installed to use the new type of tuple accessible since C#7.0
* tuples initialized using literal constants have fields named *ItemX* where *X* is the ordered number of the item of the tuple
  ```
  var unnamed = ("one", "two");
  Console.WriteLine(unnamed.Item1); // prints one
  ```
* a *named tuple* associates elements with a name
  ```
  var named = (first: "one", second: "two");
  ```
* multiple elements from a tuple (or more generally, any objects) can be retrieved using a single *deconstruct* operation
  ```
  var(a,b,c) = (1,2,3);
  Console.WriteLines($"{a},{b},{c}"); // prints 1,2,3
  ```
  * methods can return several objects using deconstruction operator
    ```
    public (int, int, int) MyMethod() { return a,b,c; }
    ```

## Interfaces
* *Interfaces* can contain methods, properties, events, or indexers
  * can't contain constants, fields, operators, instance constructors, finalizers, or types
* Every member of an *Interface* must be public, and can't be static
* the member of the class or struct implementing the corresponding interface member must be public and non-static
* Interface members implemented by a base class are inherited by its derived classes
* the members of an interface implemented explicitly in a class can't be set as public
  ```
  public int GetLength(){...}
  // or
  int IDimensions.GetLength(){...}
  ```
* Explicit implementation can be used to specify functionalities accessible only from the interface instance obtain from a cast
  ```
  A a = new A();
  Console.WriteLine(a.Lenght()) // prints the value obtain from the interface methods implemented as public 
  Console.WriteLine(((IRare)a).Lenght()); // prints the value of the interface members explicitly implemented 
  ...
  public float Length(){return 12;}
  float IRare.Length(){return 15;}
  ...
  interface IRare { float Length(); }
  interface IStandard { float Length(): }
  ```


## C# Projects Management

### Organization
* assemblies
  * packages created during compilation
  * contains the compiled programs
  * have the file extension .exe if application or .dll if libraries
    * libraries are code without a Main entry point
  * contain executable code in the form of Intermediate Language (IL) instructions and symbolic information as metadata
    * .NET Common Language Runtime uses it JIT compiler to convert it to processor-specific code
  * referencing an assembly is enough to make available its public types and members 
    * by being self-describing unit of functionality, *include* directive and *header* files are not required
* programs
  * are made of several source files
  * can declare one or several types in a single file 
  * can declare one or several namespaces in a single file 
* types
  * can be organized into namespaces
  * contains members
  * classes and interfaces are common examples of types
* members
  * fields, methods, properties, and events are common examples of members

### Debugging 
* Use 'Immediate Window' to run expression while program is stopped at a breakpoint
  * Example : running the command '? name == String.Empty' returns true
* Step through the program
  * Place cursor where the stepping process should start
  * Select 'Debug -> Step Into' or 'F11'
  * Use same button or key to step into each statement wanted
  
### Testing
* Test class library using a Unit test project
  * In the same solution, create a new project
  * Use template 'Visual C# -> .Net Core -> Unit Test Project'
  * In the 'Dependencies' section of the project, right click to add a reference
  * In the 'Reference Manager', access the 'Project' section and select the project to test
  * Modify the class of the Unit Test project marked with the attribute '[TestClass]'
  * Add methods calling members of the 'Assert' class
  * Select menu 'Test -> Run -> All Tests' to run Unit test methods
  * Check result of tests in the 'Test Explorer' window.

## Classes
* Define constructor with fewer parameters to provide default values when calling constructor with the greatest number of parameters
  ```
  public Book(string title, string author) : this(title, String.Empty, author){}
  public Book(string title, string isbn, string author) : base(title, author, PublicationType.Book)
  ```
* C# compiler automatically generates a private backing field associated with each public property of a class 
* Operators can be overloaded for expressions including user-defined class or struct types used as operands 
* class header includes attributes, modifiers, the class name, the base class, and the interfaces implemented
* the *new* operator allocated memory for the new instance of the class, and return a reference to the instance
  * the reference to the instance can be assigned to another variable of the same class type 
  ```
  Myclass a = new Myclass(){Name = "bob"};
  Console.WriteLine(a.Name); // prints "bob"
  MyClass b = a; // assign to b the value of the reference to the instance of the class
  b.Name = "john";
  Console.WriteLine(b.Name); // prints "john"
  Console.WriteLine(a.Name); // prints "john"
  ```
* A derived class can be implicitly converted to any of its base classes
  ```
  Base a = new Base();
  Base b = new Derived();
  ```
* The memory occupied by an object is automatically reclaimed when the object is no longer referenced
* A *generic* class type is declared to take type parameters
  * type arguments must be provided during definition of an instance of a generic class
  ```
  public class MyClass<Tone, Ttwo>
  {
    public Tone one;
    public Ttwo two;
  }

  MyClass<string, int> myclass = new MyClass<string, int>{ one = "bob", two = 12}
  ```
* Values specified in the invokation of class' method are called arguments while values in a class definition are called arguments
* Methods of a class can receive four kinds of parameters
  * *value parameters* are used for passing input arguments that are not affected by modification of the value parameter
    * default value can be specified
  * *reference parameters* are used for passing arguments by reference
    * reference parameters represents the same storage location as the argument variables
    * reference parameters are declared with the *ref* modifier
    ```
    public static void Method(ref int x){...}
    ...
    int a = 2;
    Method(ref a);
    ```
  * *output parameters* are used for passing arguments by reference similarly to reference parameters
    * it's required to explicitly assign a value to the arguments provided to the method
    * output parameters is declared with the *out* modifier
    ```
    public static void Method(out int x){...}
    ...
    Method(out int a); //a doesn't have to be declared prior to the method call 
    ```
  * *parameter arrays* are used to pass an varying number of arguments to a method
    * parameter arrays are declared with the *params* modifier
    * only the last parameter of a method can be a parameter array
    * parameter array can only be single-dimensional array
      * the arguments passed to the method can either be declared as an array, or as multiple elements of the array
      ```
      Method(a,b,c);
      // is equivalent to
      int[] arg = {1,2,3};
      Method(arg);
      ```
* Static methods can only directly access static members
* Instance methods can access both static and instance methods
* *Properties* don't denote storage location but rather have accessors specifiying statements executed during read or write call
  * properties can be static or instantiated
  * accessors of a property can be virtual, the modifiers are then indicated to the property name but apply to its accessors
* An *Indexer* is a member enabling the object to be indexed
  * indexers are declared like a property and have accessors
  * indexers don't have a name but rather use *this*
  ```
  public int this[int index]
  {
    get
    {
      return items;
    }
    set
    {
      items[index] = value;
    }
  }
  ```
  * indexers can be overloaded if the number or the type of the parameters differ between each declaration

### Inheritance
* Members of a base class not inherited by derived classes
  * Static constructors
    * called automatically to initialize the class immediately before a first instance is created
    * used to set static properties and fields
    * don't take modifiers or parameters
  * Instance constructors
  * Finalizers
    * used to free unmanaged resources
    * only used with classes, not structs
    * don't take modifiers or parameters
    * should not be empty to avoid performance issues
    * programmer doesn't control when finalizer is called
    * can be used as safeguard to clean if IDisposable Dispose method fails
* Modifiers set accessibility of base class member to derived classes
  * *public* members are not limited in their access and are part of the public interface of derived classes
  * *protected* members are visible only to the class and its derived classes
    * nested classes can't access protected members unless they derive from the nesting class 
  * *private* members are visible only to the class itself and nested derived classes  
  * *internal* members are visible only to classes in the same assembly 
  * *protected internal* members are visible to the class and its derived classes within different assemblies
* All types of .NET type system inherit from Object type
  * 8 methods are implicitly inherited from Object type
    * ToString method as defined in Object class returns the fully qualified name of the object type
    * Equals(Object) method as defined in Object class compares the reference held by the two variables
      * if the Equals(Object) is overriden, the GetHashCode method must be as well since the values returned is consistent with the test for equality
* Derived classes can override virtual members of the base class marked with a *virtual* modifier
  * abstract members are virtual member with no implementations
    * abstract methods are only permitted in abstract classes
    * abstract classes can contained non-abstract methods
  * abstract members of a base class are required to be overriden by the derived classes
    * constructor of abstract base class is called by constructor of derived class
    * C# compiler automatically generates a call to the base class default constructor
* A class with a 'seal' modifier can't be used as base class
* An implicit conversion exists from a class type to any of its base class types
  * a variable of a class type can reference an instance of that class, or any of its derived classes
  * An instance object of a derived class can be pass as argument of a function expecting an instance of its base class
  ```
  A a = new A(1,2);
  A b = new B(1,2,3);
  ```
  * an instance of a derived class can be given when a base object is expected, but not the contrary
  ```
  Object a = new A(); // works
  A b = new Object(); // returns an error
  ```
* *Method overloading* can be used to define several methods with the same name as long as their signature is different
  * the compiler uses *overloading resolution* to determine the specific method to invoke
  * a specific method overloaded can be choosen by explicitly casting the argument to its expected parameter types and by supplying its type arguments
* *Events* are members providing notifications
  * events are declared using the *event* keyword
  ```
  public event EventHandler MyEvent;
  ```
  * the type of the event must be a deleguate
  * events behave like a field of deleguate type
    * the field would store a reference to a deleguate representing the event handlers added to the event
    * if no event handlers are present, the field is null
  * clients react to event throught *event-handlers*
    * *event-handlers* are attached to the event using the *+=* or *-=* operators 
  * event declarations can explicitly provide *add* and *remove* accessors to handle its underlying storage
* *Operator* members of a class can be unary, binary, or conversion operators
  * all operators must be declared as *static* and *public*
  * the default behavior of the equality operator *==* when applied to container is to compare the object they refer to
* *Finalizers* members implement the action required to finalize an instance of a class
  * *Finalizers* don't have parameters nor modifiers and can't be explicitly invoked
  * *Finalizers* are invoked automatically during garbage collection
    * the invokation of the garbage collector is not deterministic and thus the use of finalizer should be avoided if alternatives are possible

### Structs
* All struct inherit from ValueType
  * don't support user-specified inheritance
* Struct constructors are invoked with the *new* operator that return the struct value itself, instead of a reference to it

### Interfaces
* can employ *multiple inheritance*
* Classes or structs implementing an interface can be implicitly converted to that interface type
* using *explicit interface member implementations*, the members can only be accessed via the interface type
```
public class Myclass : IMyInterface
{
  void IMyInterface.InterMethod(){}
}
...
MyClass instance = new MyClass();
a.InterMethod(); // returns an error
IMyInterface inter = instance;
inter.InterMethod(); // OK
```

### Enums
* Contains a set of named constants
* The default *underlying type* of the members of enum is *int*
  * the range of possible values are determined by the underlying type, and is not limited by its members value
  * the *underlying type* can be explicitly specified
  * values can be converted to integral values and vice versa using type casts
  ```
  enum MyEnum : sbyte
  {
      Left = -1,
      Center = 0,
      Right = 1
  }
  
  enum MyEnum2
  {
      Left = 1,
      Center = 3,
      Right
  }
  ...
  Console.WriteLine(MyEnum.Right); // prints Right
  Console.WriteLine((MyEnum)0); // prints 0
  Console.WriteLine((MyEnum)1); // prints Left
  Console.WriteLine((MyEnum)2); // prints 2
  Console.WriteLine((MyEnum)3); // prints Center
  Console.WriteLine((MyEnum)4); // prints Right
  ```

### Delegates
* *Delegate type* represents references to methods with a particular parameter list and return type
  * methods can be treated as entities that can be assigned to variables and passed as parameters
* *Delegates* can reference static methods or instance methods 
* *Delegates* can be created using anonymous functions
  ```
  double[] a = {1.0, 2.0, 3.0}; // using an array initializer
  double[] doubles = MyFunction(a, (double x) => x * 2.0);
  ```

## Syntax elements
* 'using' statement is used to automatically manage resource cleanup
  * variables initialized in the 'using' statement must implement the 'IDisposable' interface to call Dispose method to clean the resource after its released
* Declaring 'using static' when indicating a namespace allow to imports the methods from one class only instead of all classes of a namespace
* two dimensional array are declared as int[,] where single-dimensional array of arrays would be as int[][] 
* assignment (*=*) and conditional (*?:*) operators are right-associative
* *checked* statements throw an exception if overflow happen, *unchecked* let it be
* *readonly* fields can only be assigned in the class declaration or in a constructor of the class
* Generic methods don't need to receive type arguments as they can often be inferred from the arguments of the method
* The *signature* of a method is composed of its name, its list of type arguments, and its list of parameters
* *Arrays* contain elements of a the same type
  * in an array of arrays, the element arrays don't have to all be of the same length
  ```
  int[][] a = new int[3][];
  a[0] = new int[5];
  a[1] = new int[6];
  a[2] = new int[7];
  ```
  * *Array initializers* can be used in new operators to set the values of an array during its declaration
  ```
  int[] a = new int[]{1,2,3};
  // or
  int[] a = {1,2,3};
  ```
* *bool* types don't convert to int, unlike in C or C++
* Type conversion without data loss are performed automatically by the compiler
  * a cast is required in the source code when the conversion might cause data loss

### Attributes
* *Attributes* are user-defined types of declarative information that can be attached to program entities
  * *Attributs* are retrieved at run-time
* All attributes classes derive from the *Attribute* base class provided by the standard library
* Name of *attributes* ending by "Attribute" can have that part ommited during their reference

# ToDo
### Read more
* Interpolation string
* Enumerator method
* Asynchronous task
* Action delegate
* Lambda expression
