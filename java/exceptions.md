# Exceptions in Java

Exception Handling in Java is one of the effective means to handle the runtime errors so that the regular flow of the application can be preserved. Java Exception Handling is a mechanism to handle runtime errors such as ClassNotFoundException, IOException, SQLException, RemoteException, etc.

> Font: [Exceptions in Java | Geeks for Geeks](https://www.geeksforgeeks.org/exceptions-in-java/)

## Errors
Represent irrecoverable conditions such as Java virtual machine (JVM) running out of memory, memory leaks, stack overflow errors, library incompatibility, infinite recursion, etc. Errors are usually beyond the control of the programmer, and we should not try to handle errors.

## Exceptions
Represent recoverable conditions such as invalid user input, network connection failure, database connection failure, etc. Exceptions are usually within the control of the programmer, and we should try to handle exceptions. In other words, exception indicates conditions that a reasonable application might try to catch.

### Types of Exceptions
Talking about exceptions, there are two types of exceptions in Java:
- User-defined exceptions
- Built-in exceptions

#### User-defined exceptions
User-defined exceptions are exceptions that are created by the programmer. These exceptions are created by extending the Exception class or the RuntimeException class. The Exception class is used to create checked exceptions and the RuntimeException class is used to create unchecked exceptions.

##### The Need for Custom Exceptions
Java exceptions cover almost all general exceptions that are bound to happen in programming.
However, we sometimes need to supplement these standard exceptions with our own. These are the main reasons for introducing custom exceptions:

- Business logic exceptions – exceptions that are specific to the business logic and workflow. 
- These help the application users or the developers understand what the exact problem is.
To catch and provide specific treatment to a subset of existing Java exceptions

#### Built-in exceptions
Built-in exceptions are the exceptions that are available in Java libraries. These exceptions are suitable to explain certain error situations.

Iside built-in exceptions, we can find:

- **Checked Exceptions**: Checked exceptions are called compile-time exceptions because these exceptions are checked at compile-time by the compiler.

- **Unchecked Exceptions**: The unchecked exceptions are just opposite to the checked exceptions. The compiler will not check these exceptions at compile time. In simple words, if a program throws an unchecked exception, and even if we didn’t handle or declare it, the program would not give a compilation error.

##### Checked Exceptions
These are the exceptions that are checked at compile time. If some code within a method throws a checked exception, then the method must either handle the exception (with a try catch block) or it must specify the exception using the throws keyword.

##### Unchecked Exceptions
These are the exceptions that are not checked at compile time. In C++, all exceptions are unchecked, so it is not forced by the compiler to either handle or specify the exception. It is up to the programmers to be civilized, and specify or catch the exceptions. In Java, exceptions under Error and RuntimeException classes are unchecked exceptions, everything else under throwable is checked.


