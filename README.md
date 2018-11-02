#From Java8 to Java11 - development guide

## Java 9

### Java REPL (JShell)  
It stands for Java Shell. It's used to easily execute and test any Java construction like a class, interface, enum, etc.  

### Factory Methods for Inmutable Collections (List, Map, Set & Map.Entry)
_(I'll use Lists as an example in this file, but this is valid for Maps or Sets too)_  

Until Java8 we could use `Collections.unmodifiableList()` to achieve this, but this is really verbose. Now we can do the same with  
```
List inmutableList = List.of("bla", "ble", "bli");
```

### Private methods in Interfaces
To avoid redundant code and more re-usability we can use _private_ and _private static_ methods directly in interfaces now. They behaviour is the same as a normal class
```
public interface Card {

	private Long createcardID() {
		// calculate and return ID
	}

	private static void displayCardDetails() {
		// implement
	}

}
```

### Modules

### Try-with resources improvements

### HTTP Client Protocol API

### References
https://www.journaldev.com/13121/java-9-features-with-examples

## Java 10

### Local-variable Type Inference
It adds type inference to declarations of local variables with initializers. It can only be used in the following scenarios:  

* Limited only to local variable with initializer  
```
var numbers = List.of(1, 2, 3, 4);
```

* Indexes of foreach loops  
```
for(var number : numbers) {
	// do something
}
```

* Local declared in for loop  
```
for(var i = 0; i < numbers.size(); i++) {
	// do something
}
```

### API Improvements

#### List, Map & Set
##### Collection#copyOf(Collection)
Returns an **unmodifiable list, map or set** containing the entries provided. For a List, if the original list is subsequently modified, the returned List will not reflect this modifications.  
```
List<String> strings = new ArrayList<>();
strings.add("bla");
strings.add("ble");

List<String> copy = List.copyOf(strings);
strings.add("bli");

System.out.println(strings); // bla, ble, bli
System.out.println(copy); // bla, ble
```

##### Collectors#toUnmodifiable...()
Different methods to collect into a unmodifiable collection.
```
List<String> strings = new ArrayList<>();
strings.add("bla");
strings.add("ble");

List<String> unmodifiableStrings = strings.stream()
										  .collect(Collectors.toUnmodifiableList());
```

#### Optional#orElseThrow()
Is the same as _Optional#get()_ but the doc states that this is a preferred alternative.  
```
String name = "bla";
Optional<String> optionalName = Optional.ofNullable(name);
String s = optionalName.orElseThrow(); // bla
```
```
Optional<String> empty = Optional.empty();
String s = empty.orElseThrow(); // throws java.util.NoSuchElementException
```

### References
https://www.journaldev.com/20395/java-10-features#local-variable-type-inference-jep-286  
https://howtodoinjava.com/java10/java10-features/  

## Java 11

### Single-file Applications
Now it's possible to run single-file applications without the need to compile. It really simplifies the process to test new features.  

In the file we have to write the following shebang  
```
#!/usr/bin/java --source11
```

To run  
```
java HelloWorld.java
```

Parameters _before_ the name of the source file are passed as parameters to the java launcher. Parameters _after_ are passed as parameters to the program.  
```
java -classpath /home/bla/java HelloWorld.java Param1
```

### Type inference for lambdas
var was introduced in Java10. The ability to use it in lambdas has been introduced in Java11.  

```
list.stream()
	.map((var s) -> s.toLowerCase())
	.collect(Collectors.toList());
```
We already had type inference in lambdas, the difference is that with var we can use now the new annotations to Lambda parameters. **(TEST: If this does not work without var, but with the old type inference)**

```
lists.stream()
	.map((@Notnull var s) -> s.toLowerCase())
	.collect(Collectors.toList());
```

### Annotations with local variables for lambda parameters

### Async HTTP Client Protocol API
It was included in _Java9_ as an _incubator module_. In Java11 it was moved to be a part of _Java SE Standard_.  
The new classes are:  

* HttpClient  
* HttpRequest  
* HttpResponse  
* WebSocket  

This API can be used sync. or async. This last one makes use of:  

* CompletableFutures  
* CompletionStages  

### String API improvements
#### String#Repeat
Allows concatenating a String with itself a number x of times
```
String s = "bla ";
String result = s.repeat(2); // bla bla 
```

#### String#isBlank
Checks wether a String is empty or contains a whitespace
```
String s = "";
boolean result = s.isBlank(); // true
```

#### String#strip
Deletes whitespace on the start & end of a String. The difference with String#trim is that **this is unicode aware.** It relies on the same definition of whitespace as String#isBlank. This is a preparation for *raw strings* that will be implemented in Java12.
```
String s = " bla ";
String result = s.strip(); // bla (without spaces)
```

#### String#lines
To easily split a String into a Stream<String> of separate lines.
```
String s = "bla\nble";
List<String> lines = s.lines().collect(Collectors.toList()); // bla
															  // ble
```

### TLS 1.3

### Misc
JavaEE and CORBA modules were removed.  

### References
https://www.azul.com/90-new-features-and-apis-in-jdk-11/  
https://4comprehension.com/java-11-string-api-updates/  