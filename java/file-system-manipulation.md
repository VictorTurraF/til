# File System Manipulation

With java and properly packages you can perform all file system operations, some of them are:
- Create, Rename, Move and Delete files
- Create, Rename, Move and Delete folders
- Read and Write files
- List files and folders
- Get and set file attributes

Below you can see some examples of how to perform these operations.

The examples are based [in the guide](https://www.marcobehler.com/guides/java-files).

## Paths

The `java.nio.file.Path` contais all tools to handle path.

### Creating a Path

You can use `Path.of()` to create a path.

```java
Path path = Path.of("path/to/file.txt");
```

Also accepts other formats like `URI`.

```java
Path path = Path.of("c:\\dev\\licenses\\windows\\readme.txt");

path = Path.of("c:/dev/licenses/windows/readme.txt");

path = Path.of("c:" , "dev", "licenses", "windows", "readme.txt");

path = Path.of("c:" , "dev", "licenses", "windows").resolve("readme.txt"); // resolve == getChild()

path = Path.of(new URI("file:///c:/dev/licenses/windows/readme.txt"));

// Java < 11 equivalent: Paths.get()
path = Paths.get("c:/dev/licenses/windows/readme.txt");
```

> It does not matter if you are using forward slashes e.g. on Windows, as the Path API is smart enough to construct the right path, independently of the OS and any forward-backward slash issues.

### Resolving Paths

You can use a `resolve()` method to get a child file or other path (including a folder or a file).

```java

path = Path.of("c:" , "dev", "licenses", "windows").resolve("readme.txt"); // resolve == getChild()
```

Other example:

```java
Path dir = Path.of("assets", "tasks");
Path file = dir.resolve("task1.txt");

// or directly

Path file = Path.of("assets", "tasks").resolve("task1.txt");
```

### Getting the absolute path

You can use `toAbsolutePath()` to get the absolute path. It means, the path from the root of the file system.


```java
Path absolute = Path.of("./assets/tasks").toAbsolutePath();
/* /home/victorturra/Documentos/learning/java-files-api/./assets/tasks */
```
> The path is not normalized, so it can contain `.` and `..`.

### Getting the normalized path

You can use `normalize()` to get the normalized path. It means, the path without `.` and `..`.

```java
Path normalized = Path.of("./assets/tasks").toAbsolutePath().normalize();
/* /home/victorturra/Documentos/learning/java-files-api/assets/tasks */
```

### Getting the parent path

You can use `getParent()` to get the parent path.

```java
Path parent = Path.of("./assets/tasks").toAbsolutePath().normalize().getParent();
/* /home/victorturra/Documentos/learning/java-files-api/assets */
```

### Relativizing paths

You can use `relativize()` to get the relative path between two paths.

```java
Path normalized = Path.of("./assets/tasks").toAbsolutePath().normalize();
Path relativized = Path.of("/home/victorturra/Documentos/learning").relativize(normalized);
/* java-files-api/assets/tasks */
```
> The example above is hipothetical, the intention is to get the relative path between the two paths.

## Files

The `java.nio.file.Files` contais all tools to handle files.

### Creating a file

You can use `createFile()` to create a file.

```java
Path path = Path.of("path/to/file.txt");
Path createdFile = Files.createFile(path);
```

### Creating a folder

You can use `createDirectory()` to create a folder.

```java
Path path = Path.of("path/to/folder");
Path createdFolder = Files.createDirectory(path);
```

### Creating a folder and all parents

You can use `createDirectories()` to create a folder and all parents.

```java
Path path = Path.of("path/to/folder");
Files.createDirectories(path);
```

### Deleting a file

You can use `delete()` to delete a file.

```java
Path path = Path.of("path/to/file.txt");
Files.delete(path);
```

### Deleting a folder

You can use `delete()` to delete a folder.

```java
Path path = Path.of("path/to/folder");
Files.delete(path); 
```
> Can throw an exception if the folder is not empty.

### Deleting a not empty folder

You can use `delete()` to delete a not empty folder.

```java
Path path = Path.of("path/to/folder");

try (Stream<Path> walk = Files.walk(path)) {
    walk.sorted(Comparator.reverseOrder()).forEach(pathToBeDeleted -> {
        try {
            Files.delete(pathToBeDeleted);
        } catch (IOException e) {
            // something could not be deleted
            e.printStackTrace();
        }
    });
}
```

### Moving a file

You can use `move()` to move a file.

```java
Path source = Path.of("path/to/file.txt");
Path target = Path.of("path/to/file2.txt");
Files.move(source, target);
```
> Important :warning: : Both paths should point to a file, not a folder.

### Listing files in a folder

You can use `list()` to list files in a folder.

```java
Path path = Path.of("path/to/folder");

try (var files = Files.list(path)) {
    files.forEach(System.out::println);
}
```

### List files in directory using a directory stream with a globe pattern

You can use `newDirectoryStream()` to list files in a folder using a glob pattern.

```java
Path path = Path.of("path/to/folder");

try (var files = Files.newDirectoryStream(newDir, "*.txt")) {
    files.forEach(System.out::println);
}
```

### List files recursively

You can use `walk()` to list files recursively.

```java
Path path = Path.of("path/to/folder");

try (var files = Files.walk(Path.of("assets"))) {
    files.forEach(System.out::println);
}
```

### Checking if a file or folder exists

You can use `exists()` to check if a file ou folder exists.

```java
Path path = Path.of("path/to/file.txt");

Files.exists(path) // true or false
```

### Create temporary file

You can use `createTempFile()` to create a temporary file.

```java
Path path = Files.createTempFile("file", ".txt");
```

### Create temporary folder

You can use `createTempDirectory()` to create a temporary folder.

```java
Path path = Files.createTempDirectory("folder");
```

### Getting the file owner

You can use `getOwner()` to get the file owner.

```java
Path path = Path.of("path/to/file.txt");

UserPrincipal owner = Files.getOwner(path);
```

### Getting the file size

You can use `size()` to get the file size.

```java
Path path = Path.of("path/to/file.txt");

long size = Files.size(path);
```

### Getting the file creation time

You can use `getLastModifiedTime()` to get the file creation time.

```java
Path path = Path.of("path/to/file.txt");

FileTime creationTime = Files.getLastModifiedTime(path);
```

### Comparing files

You can use `mismatch()` to compare files.

```java

Path path1 = Path.of("path/to/file1.txt");
Path path2 = Path.of("path/to/file2.txt");

long mismatch = Files.mismatch(path1, path2);
```
> The mismatch method returns the index of the first mismatched byte, or -1 if the files are identical.

### Reading string from a file

You can use `readString()` to read a string from a file.

```java
Path utfFile = Path.of("path/to/file.txt");

String content = Files.readString(utfFile, StandardCharsets.UTF_8);
```

### Reading all bytes of a file

You can use `readAllBytes()` to read a file.

```java
Stirng content = new String(Files.readAllBytes(utfFile), StandardCharsets.UTF_8);
```

### Writing string to a file

You can use `writeString()` to write a string to a file.

```java
Path utfFile = Path.of("path/to/file.txt");

Files.writeString(utfFile, "Hello World", StandardCharsets.UTF_8);
```

### Writing bytes to a file

You can use `write()` to write bytes to a file.

```java
Path utfFile = Path.of("path/to/file.txt");

Files.write(utfFile, "this is my string ää öö üü".getBytes(StandardCharsets.UTF_8));
```

### Options when writing to a file

You can use `writeString()` to write bytes to a file with options.

```java
Path utfFile = Path.of("path/to/file.txt");
Files.writeString(
    utfFile, 
    "this is my string ää öö üü", 
    StandardCharsets.UTF_8, 
    StandardOpenOption.CREATE, 
    StandardOpenOption.TRUNCATE_EXISTING, 
    StandardOpenOption.WRITE
);

// Writing bytes
Path oneMoreUtf8File = Path.of("path/to/file2.txt");
Files.write(
    oneMoreUtf8File, 
    "this is my string ää öö üü".getBytes(StandardCharsets.UTF_8),
    StandardOpenOption.CREATE, 
    StandardOpenOption.TRUNCATE_EXISTING, 
    StandardOpenOption.WRITE
);
```

## FileWriter and FileReader

We can use `FileWriter` and `FileReader` to write and read a file.

Java `FileWriter` class of `java.io` package is used to write data in character form to file. Java FileWriter class is used to write character-oriented data to a file. It is a character-oriented class that is used for file handling in java.

`FileReader` is a class in the `java.io` package which can be used to read a stream of characters from the files. This class uses either specified charset or the platform’s default charset for decoding from bytes to characters.

> Fonts: [File Writter](https://www.geeksforgeeks.org/filewriter-class-in-java/?ref=rp) and [File Reader](https://www.geeksforgeeks.org/java-io-filereader-class/?ref=rp)