# Java Packages

## What is a package?

A package is a namespace that mainly contains classes and interfaces. For instance, the standard class `ArrayList` is in the package `java.util`. For this class, `java.util.ArrayList` is called its fully qualified name because this syntax has no ambiguity. Classes in different packages can have the same name. For example, you have the two classes `java.util.Date` and `java.sql.Date`, which are different. If no package is declared in a class, its package is the default package.

> Font: [Java Roadmap - From Developers Roadmaps](https://roadmap.sh/java/)

## Examples

### Creating a package

If you are not using an IDE, you can create a package using the sintax bellow:

```bash
javac -d directory javafilename  
```

In practice, looks like this:

```bash
javac -d . Simple.java  
```

Creating a class in the package

```java
// ./MyPack/Messenger.java

package MyPack;

public class Messenger {
    public static void sayHello() {
        System.out.println("Hello from Utils class");
    }
}
```

Calling the class in the package

```java
// ./Main.java

import MyPack.Messenger;
//      ^package ^class

public class Main {
    public static void main(String[] args) {
        Messenger.sayHello();
    }
}
```

You can also import all the classes in a package using the sintax bellow:

```java
import MyPack.*;
```

You can also import the class directly inline

```java
public class Main {
    public static void main(String[] args) {
        MyPack.Messenger.sayHello();
    }
}
```