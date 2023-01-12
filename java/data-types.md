# Java Data Types

## Integer data types
| Data Type | Size (bytes)  | Range         |
|-----------|---------------|---------------|
| byte      | 1             | -128 to 127      |
| short     | 2             | -32768 to 32767  |
| int       | 4             | -2147483648 to 2147483647 |
| long      | 8             | -9223372036854775808 to 9223372036854775807|


### Examples
Exceding the type range:
```java
byte a = 130; 
// Trows: 
// java: incompatible types: possible lossy conversion from int to byte
```

Type casting:
```java
byte a = (byte) 130;
// Converting 130 from int to byte
// Outputs a = -126
```

The language takes the last bit to indicate the sign.
See the example:
```java
byte a = 100;
// In memory a is equals to 0000 1010

byte b = -100;
// In memory b is equals to 1000 1010
//                          ^ see this bit
```

## Floating Data Type
| Data Type | Size (bytes)  | Range         |
|-----------|---------------|---------------|
| float     | 4             | 3.40282347 * 10³⁸ (integers), 1.40239846 * 10⁻⁴⁵ (decimal precision)|
| double    | 8             | 1.7976931348623157 x 10³⁰⁸(integers), 4.9406564584124654 x 10⁻³²⁴ (decimal precision)|

## Textual Data Type
| Data Type | Size (bytes)  | Range         |
|-----------|---------------|---------------|
| char      | 2             | 0 to 65535    |

The char data type is a single 16-bit Unicode character. It has a minimum value of '\u0000' (or 0) and a maximum value of '\uffff' (or 65,535 inclusive).

### Examples
Assigning a unicode char:
```java

char a = '\u0041';

System.out.printf("Char example: %c\n", charExample); // Char example: A

```
> The char data type is used to store any character. For example: letters, digits, and special symbols. To specify a char you should use a single quote notation; for example: 'a', '1', or '$'.


More informations in [official java documentation](https://docs.oracle.com/javase/tutorial/java/nutsandbolts/datatypes.html).