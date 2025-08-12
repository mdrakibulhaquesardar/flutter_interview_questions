# Dart Language - প্রশ্নোত্তর সেট ১

## Dart Language Fundamentals

### প্রশ্ন ১: Dart কী এবং এটা Flutter-এর সাথে কীভাবে relate করে?

**উত্তর (ডিটেইল):**

- **Dart** হলো Google দ্বারা তৈরি একটি modern, object-oriented programming language যা client-side development-এর জন্য optimize করা। এটা strong typing, garbage collection, এবং multiple paradigms support করে।

- **Flutter-এর সাথে সম্পর্ক:**
  - Flutter-এর primary language হলো Dart
  - Dart-এর features Flutter-এর performance এবং developer experience improve করে
  - Hot Reload, strong typing, null safety সব Dart থেকে আসে

**উদাহরণ:**

```dart
// Basic Dart syntax
void main() {
  // Variables
  var name = 'John Doe'; // Type inference
  String email = 'john@example.com'; // Explicit typing
  final age = 25; // Immutable
  const pi = 3.14159; // Compile-time constant
  
  // Functions
  String greet(String person) {
    return 'Hello, $person!';
  }
  
  // Classes
  class Person {
    final String name;
    final int age;
    
    const Person(this.name, this.age);
    
    String get description => '$name is $age years old';
    
    void celebrateBirthday() {
      print('Happy birthday, $name!');
    }
  }
  
  // Usage
  final person = Person(name, age);
  print(person.description);
  person.celebrateBirthday();
  
  // Collections
  final List<String> hobbies = ['reading', 'coding', 'gaming'];
  final Map<String, dynamic> profile = {
    'name': name,
    'age': age,
    'hobbies': hobbies,
  };
  
  // Null safety
  String? nullableName; // Can be null
  String nonNullableName = 'John'; // Cannot be null
  
  // Null-aware operators
  String displayName = nullableName ?? 'Anonymous';
  String? upperName = nullableName?.toUpperCase();
  
  print('Display: $displayName');
  print('Upper: $upperName');
}
```

**Dart-এর Key Features:**
- Strong typing with type inference
- Null safety
- Garbage collection
- Async/await support
- Mixins and extensions
- Sound type system

**Interview Tips:**
- Dart হলো Flutter-এর backbone
- Null safety Dart 2.12+ থেকে default
- Type inference vs explicit typing বুঝুন
- Sound type system-এর benefits জানুন

---

### প্রশ্ন ২: Null Safety কী এবং এটা কীভাবে implement করবেন?

**উত্তর (ডিটেইল):**

- **Null Safety** হলো Dart-এর একটি feature যা null reference errors prevent করে। এটা compile-time-এ null-related bugs catch করে।

**Null Safety Rules:**
- Variables default-এ non-nullable
- Nullable variables-কে `?` দিয়ে mark করতে হয়
- Nullable variables-কে use করার আগে null check করতে হয়

**উদাহরণ:**

```dart
// Null Safety Examples
void nullSafetyDemo() {
  // Non-nullable variables (default)
  String name = 'John'; // Cannot be null
  int age = 25; // Cannot be null
  
  // Nullable variables
  String? nullableName; // Can be null
  int? nullableAge; // Can be null
  
  // Initialization
  nullableName = 'John'; // OK
  nullableName = null; // OK
  
  // name = null; // Error: Cannot assign null to non-nullable type
  
  // Null-aware operators
  String displayName = nullableName ?? 'Anonymous';
  String upperName = nullableName?.toUpperCase() ?? 'N/A';
  
  // Null-aware assignment
  nullableName ??= 'Default Name'; // Assign only if null
  
  // Null-aware access
  int nameLength = nullableName?.length ?? 0;
  
  // Late initialization
  late String lateName;
  // lateName is accessed before assignment will throw error
  
  // Null check patterns
  if (nullableName != null) {
    print('Name length: ${nullableName.length}');
  }
  
  // Null-aware cascade
  nullableName
    ?..toUpperCase()
    ?..trim();
  
  // Null-aware spread
  final List<String> names = [
    'John',
    if (nullableName != null) nullableName,
    'Jane',
  ];
  
  print('Names: $names');
}

// Class with null safety
class User {
  final String name; // Non-nullable
  final String? email; // Nullable
  final int age; // Non-nullable
  
  const User({
    required this.name, // Required parameter
    this.email, // Optional parameter
    required this.age,
  });
  
  // Method with null safety
  String getDisplayInfo() {
    final emailInfo = email != null ? ' ($email)' : '';
    return '$name$emailInfo - Age: $age';
  }
  
  // Null-safe method
  void sendEmail(String message) {
    if (email != null) {
      print('Sending email to $email: $message');
    } else {
      print('Cannot send email: no email address');
    }
  }
}

// Usage
void userExample() {
  final user1 = User(
    name: 'John Doe',
    email: 'john@example.com',
    age: 30,
  );
  
  final user2 = User(
    name: 'Jane Smith',
    age: 25, // email is null
  );
  
  print(user1.getDisplayInfo());
  print(user2.getDisplayInfo());
  
  user1.sendEmail('Hello!');
  user2.sendEmail('Hello!'); // Will show error message
}
```

**Null Safety Best Practices:**
- Use non-nullable types when possible
- Use `late` for variables that will be initialized later
- Use null-aware operators for concise code
- Always handle null cases explicitly

**Interview Tips:**
- Null safety compile-time errors prevent করে
- `late` keyword runtime errors throw করে
- Null-aware operators code concise করে
- Required parameters non-nullable হওয়া উচিত

---

### প্রশ্ন ৩: Dart-এ Variables এবং Data Types কীভাবে declare করবেন?

**উত্তর (ডিটেইল):**

- **Variables** হলো data store করার জন্য ব্যবহৃত containers। Dart-এ multiple ways-এ variables declare করা যায়।

**Variable Declaration Types:**
- `var`: Type inference
- `final`: Immutable variable
- `const`: Compile-time constant
- `late`: Late initialization
- Explicit types: `String`, `int`, etc.

**উদাহরণ:**

```dart
// Variable Declaration Examples
void variableDemo() {
  // 1. var - Type inference
  var name = 'John Doe'; // String
  var age = 25; // int
  var height = 5.9; // double
  var isStudent = true; // bool
  
  // Type is inferred and cannot be changed
  // name = 123; // Error: Cannot assign int to String
  
  // 2. final - Immutable variable
  final String fullName = 'John Doe';
  final int birthYear = 1998;
  final double pi = 3.14159;
  
  // Cannot reassign
  // fullName = 'Jane Doe'; // Error: Cannot assign to final variable
  
  // 3. const - Compile-time constant
  const int maxAge = 120;
  const String appName = 'MyApp';
  const List<String> supportedLanguages = ['en', 'bn', 'hi'];
  
  // Cannot be changed at runtime
  // maxAge = 150; // Error: Cannot assign to const variable
  
  // 4. late - Late initialization
  late String userName;
  late int userAge;
  
  // Will be initialized later
  userName = 'John';
  userAge = 25;
  
  // 5. Explicit types
  String firstName = 'John';
  int currentAge = 25;
  double currentHeight = 5.9;
  bool isEmployed = true;
  
  // 6. Dynamic type (avoid when possible)
  dynamic dynamicValue = 'Hello';
  dynamicValue = 42; // OK
  dynamicValue = true; // OK
  
  // 7. Object type
  Object objectValue = 'Hello';
  objectValue = 42; // OK (Object can hold any type)
  
  // 8. var with explicit type
  var explicitString = String;
  var explicitInt = int;
  
  // 9. Multiple declarations
  var a = 1, b = 2, c = 3;
  final d = 4, e = 5, f = 6;
  
  // 10. Type annotations with var
  var list = <String>['a', 'b', 'c'];
  var map = <String, int>{'a': 1, 'b': 2};
  var set = <int>{1, 2, 3};
}

// Class with different variable types
class VariableExample {
  // Instance variables
  final String name; // Immutable
  int age; // Mutable
  late String email; // Late initialized
  static const String className = 'VariableExample'; // Static constant
  
  // Constructor
  VariableExample(this.name, this.age);
  
  // Method to demonstrate variable usage
  void demonstrateVariables() {
    // Local variables
    var localVar = 'Local variable';
    final localFinal = 'Local final';
    const localConst = 'Local constant';
    
    // Using instance variables
    print('Name: $name');
    print('Age: $age');
    
    // Modifying mutable variable
    age++;
    print('New age: $age');
    
    // Using late variable
    email = '$name@example.com';
    print('Email: $email');
    
    // Using static constant
    print('Class name: $className');
    
    // Local variable scope
    print('Local var: $localVar');
    print('Local final: $localFinal');
    print('Local const: $localConst');
  }
}

// Function with different parameter types
void functionWithVariables({
  required String requiredParam,
  String? optionalParam,
  required int requiredInt,
  int optionalInt = 0,
}) {
  // Function-local variables
  var localVar = 'Function local';
  final localFinal = 'Function final';
  
  print('Required: $requiredParam');
  print('Optional: ${optionalParam ?? 'Not provided'}');
  print('Required int: $requiredInt');
  print('Optional int: $optionalInt');
  print('Local var: $localVar');
  print('Local final: $localFinal');
}

// Usage
void main() {
  variableDemo();
  
  final example = VariableExample('John', 25);
  example.demonstrateVariables();
  
  functionWithVariables(
    requiredParam: 'Hello',
    requiredInt: 42,
    optionalParam: 'Optional',
  );
}
```

**Variable Best Practices:**
- Use `final` for variables that won't change
- Use `const` for compile-time constants
- Use explicit types for clarity
- Avoid `dynamic` when possible
- Use `late` sparingly

**Interview Tips:**
- `var` type inference করে
- `final` runtime-এ assign করা যায়
- `const` compile-time-এ assign করা হয়
- `late` access করার আগে initialize করতে হয়

---

### প্রশ্ন ৪: Dart-এ Functions এবং Methods কীভাবে define করবেন?

**উত্তর (ডিটেইল):**

- **Functions** হলো reusable code blocks যা specific tasks perform করে। Dart-এ functions multiple ways-এ define করা যায়।

**Function Types:**
- Regular functions
- Anonymous functions (lambdas)
- Arrow functions
- Higher-order functions
- Generators

**উদাহরণ:**

```dart
// Function Examples
void functionDemo() {
  // 1. Regular function
  String greet(String name) {
    return 'Hello, $name!';
  }
  
  // 2. Function with multiple parameters
  String introduce(String name, int age, {String? city}) {
    final cityInfo = city != null ? ' from $city' : '';
    return 'I am $name, $age years old$cityInfo.';
  }
  
  // 3. Function with default parameters
  String createEmail(String username, {String domain = 'gmail.com'}) {
    return '$username@$domain';
  }
  
  // 4. Function with named parameters
  String buildProfile({
    required String name,
    required int age,
    String? email,
    String? phone,
  }) {
    final emailInfo = email != null ? '\nEmail: $email' : '';
    final phoneInfo = phone != null ? '\nPhone: $phone' : '';
    
    return 'Profile:\nName: $name\nAge: $age$emailInfo$phoneInfo';
  }
  
  // 5. Function with optional positional parameters
  String formatAddress(String street, String city, [String? state, String? country]) {
    final stateInfo = state != null ? ', $state' : '';
    final countryInfo = country != null ? ', $country' : '';
    
    return '$street, $city$stateInfo$countryInfo';
  }
  
  // 6. Arrow function (single expression)
  String shortGreet(String name) => 'Hi, $name!';
  
  // 7. Function that returns a function
  Function multiply(int factor) {
    return (int value) => value * factor;
  }
  
  // 8. Generic function
  T findFirst<T>(List<T> list, bool Function(T) predicate) {
    for (final item in list) {
      if (predicate(item)) {
        return item;
      }
    }
    throw Exception('No item found');
  }
  
  // Usage examples
  print(greet('John'));
  print(introduce('John', 25, city: 'Dhaka'));
  print(createEmail('john.doe'));
  print(createEmail('john.doe', domain: 'company.com'));
  
  print(buildProfile(
    name: 'John Doe',
    age: 30,
    email: 'john@example.com',
  ));
  
  print(formatAddress('123 Main St', 'Dhaka', 'Dhaka', 'Bangladesh'));
  print(shortGreet('John'));
  
  final doubleIt = multiply(2);
  print('Double of 5: ${doubleIt(5)}');
  
  final numbers = [1, 2, 3, 4, 5];
  final firstEven = findFirst(numbers, (n) => n % 2 == 0);
  print('First even number: $firstEven');
}

// Class with methods
class FunctionExample {
  final String name;
  final int age;
  
  FunctionExample(this.name, this.age);
  
  // Instance method
  String getDescription() {
    return '$name is $age years old';
  }
  
  // Method with parameters
  String greet(String greeting) {
    return '$greeting, $name!';
  }
  
  // Method with optional parameters
  String introduce({String? title}) {
    final titleInfo = title != null ? '$title ' : '';
    return '${titleInfo}${name}, $age years old';
  }
  
  // Static method
  static String getClassName() {
    return 'FunctionExample';
  }
  
  // Factory constructor with method
  factory FunctionExample.fromMap(Map<String, dynamic> map) {
    return FunctionExample(
      map['name'] as String,
      map['age'] as int,
    );
  }
  
  // Method that returns a function
  Function getGreetingFunction() {
    return (String time) => 'Good $time, $name!';
  }
}

// Higher-order functions
void higherOrderFunctionDemo() {
  // Function that takes a function as parameter
  void processList(List<int> numbers, int Function(int) processor) {
    for (final number in numbers) {
      final result = processor(number);
      print('$number -> $result');
    }
  }
  
  // Function that returns a function
  Function createProcessor(String operation) {
    switch (operation) {
      case 'double':
        return (int x) => x * 2;
      case 'square':
        return (int x) => x * x;
      case 'increment':
        return (int x) => x + 1;
      default:
        return (int x) => x;
    }
  }
  
  // Usage
  final numbers = [1, 2, 3, 4, 5];
  
  print('Doubling numbers:');
  processList(numbers, createProcessor('double'));
  
  print('\nSquaring numbers:');
  processList(numbers, createProcessor('square'));
  
  print('\nIncrementing numbers:');
  processList(numbers, createProcessor('increment'));
}

// Anonymous functions and closures
void anonymousFunctionDemo() {
  final numbers = [1, 2, 3, 4, 5];
  
  // Anonymous function
  final evenNumbers = numbers.where((number) => number % 2 == 0);
  print('Even numbers: $evenNumbers');
  
  // Closure
  Function createCounter() {
    int count = 0;
    return () => ++count;
  }
  
  final counter = createCounter();
  print('Count: ${counter()}');
  print('Count: ${counter()}');
  print('Count: ${counter()}');
}

// Usage
void main() {
  functionDemo();
  
  final example = FunctionExample('John', 25);
  print(example.getDescription());
  print(example.greet('Hello'));
  print(example.introduce(title: 'Mr.'));
  
  print('Class name: ${FunctionExample.getClassName()}');
  
  final greetingFunc = example.getGreetingFunction();
  print(greetingFunc('morning'));
  
  higherOrderFunctionDemo();
  anonymousFunctionDemo();
}
```

**Function Best Practices:**
- Use descriptive names
- Keep functions small and focused
- Use named parameters for clarity
- Use default parameters when appropriate
- Document complex functions

**Interview Tips:**
- Functions first-class objects
- Higher-order functions powerful feature
- Closures state maintain করে
- Generics type safety provide করে

---

### প্রশ্ন ৫: Dart-এ Classes এবং Objects কীভাবে create করবেন?

**উত্তর (ডিটেইল):**

- **Classes** হলো blueprints for objects যা data এবং behavior encapsulate করে। **Objects** হলো class-এর instances।

**Class Features:**
- Constructors
- Properties
- Methods
- Inheritance
- Interfaces
- Mixins
- Extensions

**উদাহরণ:**

```dart
// Class Examples
void classDemo() {
  // 1. Basic class
  class Person {
    // Properties
    final String name;
    final int age;
    String? email;
    
    // Constructor
    Person(this.name, this.age);
    
    // Named constructor
    Person.guest() : name = 'Guest', age = 18;
    
    // Factory constructor
    factory Person.fromMap(Map<String, dynamic> map) {
      return Person(
        map['name'] as String,
        map['age'] as int,
      );
    }
    
    // Getter
    String get description => '$name is $age years old';
    
    // Method
    void introduce() {
      print('Hi, I am $name, $age years old.');
    }
    
    // Setter
    set setEmail(String value) {
      email = value;
    }
    
    // Override toString
    @override
    String toString() {
      return 'Person(name: $name, age: $age, email: $email)';
    }
    
    // Override equality
    @override
    bool operator ==(Object other) {
      if (identical(this, other)) return true;
      return other is Person &&
          other.name == name &&
          other.age == age;
    }
    
    @override
    int get hashCode => name.hashCode ^ age.hashCode;
  }
  
  // 2. Class with inheritance
  class Student extends Person {
    final String studentId;
    final List<String> courses;
    
    Student(super.name, super.age, this.studentId, this.courses);
    
    @override
    void introduce() {
      super.introduce();
      print('I am a student with ID: $studentId');
    }
    
    void enrollCourse(String course) {
      courses.add(course);
      print('Enrolled in: $course');
    }
    
    @override
    String get description => '${super.description} (Student ID: $studentId)';
  }
  
  // 3. Abstract class
  abstract class Animal {
    final String name;
    
    Animal(this.name);
    
    // Abstract method
    void makeSound();
    
    // Concrete method
    void sleep() {
      print('$name is sleeping');
    }
  }
  
  // 4. Class implementing interface
  class Dog implements Animal {
    @override
    final String name;
    
    Dog(this.name);
    
    @override
    void makeSound() {
      print('$name says: Woof!');
    }
    
    @override
    void sleep() {
      print('$name is sleeping like a dog');
    }
  }
  
  // 5. Class with mixins
  class Flying {
    void fly() {
      print('Flying high!');
    }
  }
  
  class Swimming {
    void swim() {
      print('Swimming deep!');
    }
  }
  
  class Duck extends Animal with Flying, Swimming {
    Duck(super.name);
    
    @override
    void makeSound() {
      print('$name says: Quack!');
    }
  }
  
  // 6. Class with extensions
  extension PersonExtension on Person {
    String get formalGreeting => 'Good day, $name.';
    
    bool get isAdult => age >= 18;
    
    void celebrateBirthday() {
      print('Happy birthday, $name!');
    }
  }
  
  // Usage examples
  final person = Person('John', 25);
  print(person.description);
  person.introduce();
  
  final guest = Person.guest();
  print(guest.description);
  
  final student = Student('Jane', 20, 'ST001', ['Math', 'Physics']);
  student.introduce();
  student.enrollCourse('Chemistry');
  
  final dog = Dog('Buddy');
  dog.makeSound();
  dog.sleep();
  
  final duck = Duck('Donald');
  duck.makeSound();
  duck.fly();
  duck.swim();
  
  // Using extensions
  print(person.formalGreeting);
  print('Is adult: ${person.isAdult}');
  person.celebrateBirthday();
  
  // Creating objects from map
  final personMap = {'name': 'Alice', 'age': 30};
  final alice = Person.fromMap(personMap);
  print(alice);
}

// Singleton pattern
class Singleton {
  static Singleton? _instance;
  
  Singleton._internal();
  
  static Singleton get instance {
    _instance ??= Singleton._internal();
    return _instance!;
  }
  
  void doSomething() {
    print('Singleton is doing something');
  }
}

// Generic class
class Box<T> {
  final T value;
  
  Box(this.value);
  
  T getValue() => value;
  
  void setValue(T newValue) {
    // Note: This won't work with final fields
    // This is just for demonstration
    print('Setting value to: $newValue');
  }
}

// Usage
void main() {
  classDemo();
  
  // Singleton usage
  final singleton1 = Singleton.instance;
  final singleton2 = Singleton.instance;
  print('Same instance: ${identical(singleton1, singleton2)}');
  
  // Generic class usage
  final stringBox = Box<String>('Hello');
  final intBox = Box<int>(42);
  
  print('String box: ${stringBox.getValue()}');
  print('Int box: ${intBox.getValue()}');
}
```

**Class Best Practices:**
- Use meaningful names
- Keep classes focused and single-purpose
- Use composition over inheritance
- Implement proper equality and hashCode
- Use const constructors when possible

**Interview Tips:**
- Classes reference types
- Inheritance vs composition বুঝুন
- Mixins multiple inheritance simulate করে
- Extensions existing classes extend করে
