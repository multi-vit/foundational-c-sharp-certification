# Foundational C# Certification

These learnings are specific to me and won't cover all of the "ground up" basics, given I already have experience with Java and JavaScript.
Instead, they will be a selection of things that are either completely new, slightly different or particularly interesting.

## Learning

### Variable name rules

- Variable names can contain alphanumeric characters and the underscore character
- Special characters like the hash `#`, the dash `-`, and the dollar sign `$` are not allowed
- Variable names must begin with an alphabetical letter or an underscore, not a number
- Variable names must **NOT** be a C# keyword. For example: `float float;` or `string string;`
- Convention is to use camelCase

### Verbatim String Literals

Use the `@` symbol to allow the use of characters that would normally need to be escaped, such as `\` and new lines/tabs.
```
Console.Write(@"I can use \ any time I like, without having to escape it in the normal way (\\)
I can also write on new lines without using \n   and leave whitespace without using \t");
```

### String interpolation

Use `$` symbol to use string interpolation and include expressions in your code:

```
Console.WriteLine($"5 + 2 = {5 + 2}");
string firstName = "Andy";
Console.WriteLine($"Hello {firstName}");
```

You can add modifiers to the variable name to have it formatted in a certain way. For example, `:c` will format the variable as currency:
```
decimal price = 123.45m;
int discount = 50;
Console.WriteLine($"Price: {price:C} (Save {discount:C})");
```
The currency displayed depends on the country and display language of the machine the code is running on,
for example if "English (United States)" is set, then the price and discount will be shown in dollars.

Modifiers encountered so far:
- `:C` = currency
- `:N` = number - display 2 decimal points by defualt, add a number afterwards to show a different amount e.g. `:N4` N.B. this rounds!
- `:P` = percentage - add a number afterwards to control the number of values displayed after the percent e.g. `:P3` N.B this rounds!

You can also do composite formatting similar to Java:
```
string first = "Hello";
string second = "World";
string result = string.Format("{0} {1}!", first, second);
Console.WriteLine(result);
```

### Stateful vs stateless

- Some methods don't rely on the current state of the application to work properly - these are **stateless methods**, also known as **static methods**
- When calling a **stateless method**, you don't need to create a new instance of its class first
- Other methods, however, must have access to the state of the application to work properly - these are **stateful methods**, also known as *instance methods*
- Stateful methods keep track of their state in **fields**, which are variables defined on the class. Each new instance of the class gets its own copy of those fields in which to store state.
- When calling a **stateful method**, you need to create an instance of the class, and access the method on the object (using the `new` operator)

### Parameter vs Argument

- Parameter refers to the variable that's being used inside the method
- Argument is the value that's passed when the method is called

### Whitespace

- The C# compiler ignores whitespace
- So the following examples will compile and produce the state output:
    ```
    // Example 1:
    Console
    .
    WriteLine
    (
    "Hello Example 1!"
    )
    ;

    // Example 2:
    string firstWord="Hello";string lastWord="Example 2";Console.WriteLine(firstWord+" "+lastWord+"!");
    ```
- Use general good programming practices to order your code appropriately
- The main difference I've seen so far is that conditional code blocks have their statements and {} on a completely new line:
    ```
    if (true)
    {
        // Do some stuff
    }
    else
    {
        // Do some other stuff
    }
    ```
- You can also remove the curly braces if the code block contains a single line:
    ```
    bool flag = true;
    if (flag)
    {
        Console.WriteLine(flag);
    }
    // Can be written instead as:
    bool flag = true;
    if (flag)
        Console.WriteLine(flag);
    // This could also work on a single line but is against convention
    ```
- Only remove the curly braces of a code block when it makes the code more readable. It's always acceptable to include curly braces.

### Switch statements

- You can assign the same behaviour to different cases if needed:
    ```
    int employeeLevel = 100;
    string employeeName = "John Smith";

    string title = "";

    switch (employeeLevel)
    {
        case 100:
        case 200:
            title = "Senior Associate";
            break;
        case 300:
            title = "Manager";
            break;
        case 400:
            title = "Senior Manager";
            break;
        default:
            title = "Associate";
            break;
    }

    Console.WriteLine($"{employeeName}, {title}");
    ```

### Nullable types

- You can make a type nullable by appending a `?` to it:
    ```
    string? readResult;
    Console.WriteLine("Enter a string:");
    do
    {
        readResult = Console.ReadLine();
    } while (readResult == null);
    ```

### Extra return values

- In C#, as well as the usual `return` statement, you can also return values using the `out` keyword.
- I've seen this in int.TryParse() so far:
    ```
    string value = "bad";
    int result = 0;
    // int.TryParse() returns a boolean as to whether it was successful
    // As well as setting a variable to the result of the conversion!
    if (int.TryParse(value, out result))
    {
        Console.WriteLine($"Measurement: {result}");
    }
    else
    {
        Console.WriteLine("Unable to report the measurement.");
    }

    if (result > 0)
        Console.WriteLine($"Measurement (w/ offset): {50 + result}");
    ```

### Pass by value/reference

- Most methods require you pass arguments by value (this is the default behaviour)
- However, sometimes you need to pass arguments by reference, using the `ref` keyword:
    ```
    // Change the size of an array called pallets, using the ref keyword, to 6
    Array.Resize(ref pallets, 6);
    ```

### Constants

Learnt in the context of avoiding *Magic values* - values which are hardcoded instead of being stored in variables.

Use the `const` keyword to define and initialise a variable whose value can never be changed:
const string message = "I cannot be changed";

### Methods

- Method names should be Pascal case and generally shouldn't start with digits
- You don't have to have a method defined before you call it
- It's common to define all methods at the end of a program. For example:
```
int[] a = {1,2,3,4,5};

Console.WriteLine("Contents of Array:");
PrintArray();

void PrintArray()
{
    foreach (int x in a)
    {
        Console.Write($"{x} ");
    }
    Console.WriteLine();
}
```
- When an argument is passed to a method, value type variables have their values copied into the method.
Each variable has its own copy of the value, so the original variable isn't modified
- With reference types, the address of the value is passed into the method.
The variable given to the method references the value at that address, so operations on that variable affect the value that is referenced by the other

### Strings

It is important to remember that `string` is a reference type, but it is *immutable*.
That means once it has been assigned a value, it can't be altered.
In C#, when methods and operators are used to modify a string, the result that is returned is actually a new string object.