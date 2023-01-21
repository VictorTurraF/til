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

## Examples

Creating a custom unchecked exception

Lets suppose we're working on a checking account app, and we want to create a custom exception to be thrown when a user tries to withdraw more money than he has in his account.


```java
public class InsufficientFoundsException extends RuntimeException {

    public InsufficientFoundsException( Currency balance, Currency amountInCents ) {
        super("\nYou have not enough founds. \nYour balance is " + balance + " and you tried to withdraw " + amountInCents);
    }

}
```

Your class should extend RuntimeException, and you can add a custom message to the exception.

Now, lets suppose we have a method that withdraws money from the account, and we want to throw the exception if the user tries to withdraw more money than he has in his account.

```java
public class CheckingAccount {

    /* ... class definitions ... */

    public void withdraw(Integer amountInCents) {
        if (this.balance.getAmountInCents() < amountInCents)
            throw new InsufficientFoundsException(this.balance, new Currency(amountInCents));

        this.balance.subtractCents(amountInCents);
    }

}
```

Now, if we try to withdraw more money than we have in our account, we'll get the exception we created.

```java
public class Main {
    public static void main(String[] args) {
        CheckingAccount checkingAccount = new CheckingAccount();

        checkingAccount.deposit(10000);

        checkingAccount.withdraw(10004);

        System.out.println("Your balance is: " + checkingAccount.balance);
    }
}
```

Output:

```
Exception in thread "main" InsufficientFoundsException: 
You have not enough founds. 
Your balance is R$ 100,00 and you tried to withdraw R$ 100,04
	at CheckingAccount.withdraw(CheckingAccount.java:14)
	at Main.main(Main.java:7)

Process finished with exit code 1
```

As you can see in the output, we have the custom message we created in the exception. In this case this is a unchecked exception, so we don't need neighter to declare it in the method signature or surround it with a try/catch block.

This is a hipothetical example, in a real usage of a checking account, we would probably want to create a checked exception, so we could handle it in a try/catch block, and not let the program crash. What about to create a custom checked exception? Lets see how to do it.

First lest change our exception to extend `Exception` instead of `RuntimeException`.

```java

public class InsufficientFoundsException extends Exception {
    /* ... here stays the same code ... */
}
```

If you try to compile this code, you'll get an error, because we're not handling the exception in the method signature. You can see that the compiler is forcing us to handle the exception. It shows the name of the exception we have created and the line in the code.

```
/home/victorturra/Documentos/learning/java-exceptions/src/CheckingAccount.java:14:13
java: unreported exception InsufficientFoundsException; must be caught or declared to be thrown
```

I'm using Intellij IDEA, the IDE shows the error in the line where the exception is thrown.

![Captura de tela de 2023-01-21 15-16-04](https://user-images.githubusercontent.com/59932737/213881405-3e84f70c-ac5e-4f82-b96c-d116aa3268a5.png)

Lets change the method signature to handle the exception. We need to add the `throws` keyword, and the name of the exception we want to handle.

```java
public void withdraw(Integer amountInCents) throws InsufficientFoundsException {
    /* ... here stays the same code ... */
}
```

Still, if you try to compile the code, you'll get an error, because we're not handling the exception in the method main of our application.

![image](https://user-images.githubusercontent.com/59932737/213881460-52c0567c-d554-4cd9-9525-d7380af5ca53.png)


```java




