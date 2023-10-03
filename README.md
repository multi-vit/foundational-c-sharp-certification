# Foundational C# Certification

## Learning

### Verbatim String Literals

Use the `@` symbol to allow the use of characters that would normally need to be escaped, such as `\` and new lines/tabs.

### String interpolation

Use `$` symbol to use string interpolation and include expressions in your code:

```
Console.WriteLine($"5 + 2 = {5 + 2}");
string firstName = "Andy";
Console.WriteLine($"Hello {firstName}");
```

### Stateful vs stateless

- Some methods don't rely on the current state of the application to work properly - these are *stateless methods*, also known as *static methods*'