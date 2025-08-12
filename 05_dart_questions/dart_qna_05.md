## ডার্ট প্রোগ্রামিং ভাষার মৌলিক বিষয়াবলী: সাক্ষাৎকার প্রশ্ন ও উত্তর (প্রথম অংশ)

#### প্রশ্ন ১: ডার্টে `var`, `final`, এবং `const` এর মধ্যে পার্থক্য কি? কখন কোনটি ব্যবহার করা উচিত?

**উত্তর:** ডার্টে ডেটা ডিক্লেয়ার করার জন্য `var`, `final` এবং `const` এই তিনটি কীওয়ার্ড ব্যবহার করা হয়। এদের মধ্যে প্রধান পার্থক্য হলো ভ্যারিয়েবলের মান পরিবর্তনযোগ্যতা (mutability) এবং কম্পাইল-টাইম বনাম রান-টাইম কনস্ট্যান্ট।

*   **`var`:** এটি একটি জেনেরিক কীওয়ার্ড যা ভ্যারিয়েবলের টাইপ স্বয়ংক্রিয়ভাবে অনুমান করে। `var` দিয়ে ডিক্লেয়ার করা ভ্যারিয়েবলের মান রান-টাইমে পরিবর্তন করা যায়।
    
```
dart
    var name = 'Alice';
    name = 'Bob'; // Valid
    
```
*   **`final`:** `final` কীওয়ার্ড ব্যবহার করে ডিক্লেয়ার করা ভ্যারিয়েবলের মান শুধুমাত্র একবার অ্যাসাইন করা যায় এবং পরে আর পরিবর্তন করা যায় না। এর মান রান-টাইমে নির্ধারণ করা যেতে পারে।
    
```
dart
    final currentTime = DateTime.now();
    // currentTime = DateTime.now(); // Invalid
    
```
*   **`const`:** `const` কীওয়ার্ড ব্যবহার করে ডিক্লেয়ার করা ভ্যারিয়েবল কম্পাইল-টাইম কনস্ট্যান্ট হতে হবে। এর মান অবশ্যই কম্পাইল হওয়ার আগেই নির্ধারিত থাকতে হবে এবং এটি কখনোই পরিবর্তন করা যায় না। `const` ইমিউটেবল।
```
dart
    const PI = 3.14159;
    // PI = 3.0; // Invalid
    const List<int> constantList = [1, 2, 3];
    // constantList[0] = 10; // Invalid
    
```
**কখন কোনটি ব্যবহার করা উচিত:**
*   যখন একটি ভ্যারিয়েবলের মান পরিবর্তন করার প্রয়োজন হয়, তখন `var` ব্যবহার করুন।
*   যখন একটি ভ্যারিয়েবলের মান শুধুমাত্র একবার সেট করা হবে এবং পরে পরিবর্তন হবে না, তখন `final` ব্যবহার করুন। এটি রান-টাইম ডেটার জন্য উপযুক্ত।
*   যখন একটি ভ্যারিয়েবলের মান কম্পাইল হওয়ার আগেই স্থির থাকে এবং কখনোই পরিবর্তন হবে না, তখন `const` ব্যবহার করুন। এটি স্ট্যাটিক ডেটা এবং পারফরম্যান্স অপ্টিমাইজেশানের জন্য ভালো।

#### প্রশ্ন ২: ডার্টের ডেটা টাইপগুলো কি কি? প্রাইমিটিভ এবং কালেকশন ডেটা টাইপের উদাহরণ দিন।

**উত্তর:** ডার্ট একটি স্ট্যাটিক্যালি টাইপড ভাষা, তবে এটি টাইপ ইনফারেন্স (type inference) সমর্থন করে। ডার্টের কিছু মৌলিক ডেটা টাইপ নিচে দেওয়া হলো:

**প্রাইমিটিভ ডেটা টাইপ (Primitive Data Types):**
এগুলো মৌলিক বিল্ট-ইন টাইপ যা একক ভ্যালু ধারণ করে।
*   **`int`:** পূর্ণসংখ্যা (যেমন: 10, -5)।
*   **`double`:** ফ্লোটিং-পয়েন্ট সংখ্যা (যেমন: 3.14, -2.5)। `num` টাইপ `int` এবং `double` উভয়কেই অন্তর্ভুক্ত করে।
*   **`String`:** টেক্সট সিকোয়েন্স (যেমন: 'Hello', "World")। স্ট্রিং ডাবল বা সিঙ্গেল কোট ব্যবহার করে লেখা যায়। মাল্টি-লাইন স্ট্রিংয়ের জন্য ট্রিপল কোট (`'''` বা `"""`) ব্যবহার করা যায়।
*   **`bool`:** বুলিয়ান ভ্যালু, শুধুমাত্র `true` বা `false` হতে পারে।

**কালেকশন ডেটা টাইপ (Collection Data Types):**
এগুলো একাধিক ভ্যালু ধারণ করে।
*   **`List`:** অর্ডারড কালেকশন যা ডুপ্লিকেট ভ্যালু ধারণ করতে পারে। স্কয়ার ব্র্যাকেট (`[]`) ব্যবহার করে তৈরি করা হয়।
```
dart
    List<int> numbers = [1, 2, 3, 3];
    
```
*   **`Set`:** ইউনিক ভ্যালুর আনঅর্ডারড কালেকশন। কার্লি ব্র্যাকেট (`{}`) ব্যবহার করে তৈরি করা হয়, তবে লিস্ট থেকে আলাদা করার জন্য টাইপ উল্লেখ করা ভালো বা `Set()` কনস্ট্রাক্টর ব্যবহার করা হয়।
```
dart
    Set<String> names = {'Alice', 'Bob', 'Alice'}; // Becomes {'Alice', 'Bob'}
    
```
*   **`Map`:** কী-ভ্যালু পেয়ারের কালেকশন। প্রতিটি কী ইউনিক হতে হবে। কার্লি ব্র্যাকেট (`{}`) ব্যবহার করে তৈরি করা হয়।
```
dart
    Map<String, int> ages = {'Alice': 30, 'Bob': 25};
    
```
*   **`Runes`:** স্ট্রিং এর ইউনিকোড কোড পয়েন্টের সিকোয়েন্স।
*   **`Symbols`:** রান-টাইমে ডিক্লেয়ার করা একটি অপারেশনের জন্য ব্যবহৃত হয়।

#### প্রশ্ন ৩: ডার্টে কন্ট্রোল ফ্লো স্টেটমেন্টগুলি (if, else, switch, loops) কিভাবে কাজ করে উদাহরণ সহ ব্যাখ্যা করুন।

**উত্তর:** ডার্টে প্রোগ্রাম ফ্লো নিয়ন্ত্রণ করার জন্য বিভিন্ন স্টেটমেন্ট রয়েছে:

*   **`if` এবং `else`:** একটি শর্ত সত্য হলে নির্দিষ্ট কোড ব্লক এক্সিকিউট করতে ব্যবহৃত হয়। `else` ঐচ্ছিক এবং শর্ত মিথ্যা হলে এক্সিকিউট হয়।
```
dart
    int score = 75;
    if (score >= 60) {
      print('Passed');
    } else {
      print('Failed');
    }
    
```
*   **`else if`:** একাধিক শর্ত পরীক্ষা করার জন্য `if` এবং `else` এর সাথে ব্যবহার করা হয়।
```
dart
    int grade = 85;
    if (grade >= 90) {
      print('A');
    } else if (grade >= 80) {
      print('B');
    } else {
      print('C');
    }
    
```
*   **`switch`:** একটি ভ্যারিয়েবলের মান বিভিন্ন কেসের সাথে তুলনা করার জন্য ব্যবহার করা হয়।
```
dart
    String day = 'Monday';
    switch (day) {
      case 'Monday':
        print('Start of the week');
        break; // break is essential to exit the switch after a match
      case 'Friday':
        print('End of the week');
        break;
      default:
        print('Mid-week');
    }
    
```
*   **`for` লুপ:** নির্দিষ্ট সংখ্যক বার একটি কোড ব্লক রিপিট করার জন্য ব্যবহৃত হয়।
```
dart
    for (int i = 0; i < 5; i++) {
      print('Iteration $i');
    }
    
```
*   **`while` লুপ:** একটি শর্ত সত্য থাকা পর্যন্ত একটি কোড ব্লক রিপিট করার জন্য ব্যবহৃত হয়।
```
dart
    int count = 0;
    while (count < 3) {
      print('Count: $count');
      count++;
    }
    
```
*   **`do-while` লুপ:** `while` লুপের মতো, তবে কোড ব্লকটি অন্তত একবার এক্সিকিউট হবে তারপর শর্ত পরীক্ষা করা হবে।
```
dart
    int i = 0;
    do {
      print('Value: $i');
      i++;
    } while (i < 0); // Condition is false, but runs once
    
```
*   **`for-in` লুপ:** কালেকশনের আইটেমগুলির উপর ইটারেট করার জন্য ব্যবহৃত হয়।
```
dart
    List<String> fruits = ['apple', 'banana', 'orange'];
    for (String fruit in fruits) {
      print(fruit);
    }
    
```
#### প্রশ্ন ৪: ডার্টে ফাংশন কিভাবে ডিক্লেয়ার এবং ব্যবহার করা হয়? প্যারামিটার এবং রিটার্ন টাইপ সম্পর্কে বলুন।

**উত্তর:** ডার্টে ফাংশন হলো একটি কোড ব্লক যা নির্দিষ্ট কাজ সম্পাদন করে।

**ফাংশন ডিক্লেয়ারেশন:**
একটি ফাংশন ডিক্লেয়ার করার জন্য প্রথমে ঐচ্ছিকভাবে রিটার্ন টাইপ, তারপর ফাংশনের নাম, এবং প্যারামিটার বন্ধনী `()` ব্যবহার করা হয়।
```
dart
returnType functionName(parameter1, parameter2, ...) {
  // Function body
  return value; // Optional return statement
}
```
যদি ফাংশন কিছু রিটার্ন না করে, তাহলে রিটার্ন টাইপ হিসেবে `void` ব্যবহার করা হয়। যদি রিটার্ন টাইপ উল্লেখ না করা হয়, তাহলে ডার্ট স্বয়ংক্রিয়ভাবে এটি অনুমান করে (সাধারণত `dynamic`)।

**প্যারামিটার:**
ফাংশন প্যারামিটার গ্রহণ করতে পারে। প্যারামিটারগুলি ফাংশন বন্ধনীর মধ্যে ডিক্লেয়ার করা হয়।

*   **পজিশনাল প্যারামিটার (Positional Parameters):** এগুলো বন্ধনীর মধ্যে ক্রমানুসারে ডিক্লেয়ার করা হয় এবং কল করার সময় একই ক্রমে ভ্যালু পাস করতে হয়। এগুলো ডিফল্টভাবে রিকোয়ার্ড।
```
dart
    void greet(String name, int age) {
      print('Hello $name, you are $age years old.');
    }
    greet('Alice', 30);
    
```
*   **নামড প্যারামিটার (Named Parameters):** এগুলো কার্লি ব্র্যাকেট `{}` এর মধ্যে ডিক্লেয়ার করা হয় এবং কল করার সময় নাম উল্লেখ করে ভ্যালু পাস করতে হয়। এগুলো ডিফল্টভাবে ঐচ্ছিক (`optional`)।
    
```
dart
    void displayUserInfo({String name, int age}) {
      print('Name: $name, Age: $age');
    }
    displayUserInfo(name: 'Bob', age: 25);
    displayUserInfo(age: 25, name: 'Bob'); // Order doesn't matter
    displayUserInfo(name: 'Charlie'); // age will be null
    
```
নামড প্যারামিটারকে রিকোয়ার্ড করার জন্য `required` কীওয়ার্ড ব্যবহার করা হয় (নাল সেফটি সহ):
```
dart
    void displayUserInfo({required String name, required int age}) {
      print('Name: $name, Age: $age');
    }
    
```
*   **অপশনাল পজিশনাল প্যারামিটার (Optional Positional Parameters):** এগুলো স্কয়ার ব্র্যাকেট `[]` এর মধ্যে ডিক্লেয়ার করা হয়। এগুলো ঐচ্ছিক এবং কল করার সময় এদের জন্য ভ্যালু পাস করা যেতে পারে বা নাও করা যেতে পারে।
```
dart
    void printMessage(String message, [String? sender]) {
      if (sender != null) {
        print('$message from $sender');
      } else {
        print(message);
      }
    }
    printMessage('Hello');
    printMessage('Hi', 'Bob');
    
```
**রিটার্ন টাইপ:**
ফাংশন একটি ভ্যালু রিটার্ন করতে পারে। রিটার্ন টাইপ ফাংশন ডিক্লেয়ারেশনের শুরুতে উল্লেখ করা হয়। `return` কীওয়ার্ড ব্যবহার করে ভ্যালু রিটার্ন করা হয়।
```
dart
int add(int a, int b) {
  return a + b;
}
int sum = add(5, 3); // sum is 8
```
#### প্রশ্ন ৫: ডার্টে ক্লাসেস এবং অবজেক্টসের ধারণা ব্যাখ্যা করুন। কিভাবে একটি ক্লাস ডিক্লেয়ার করা হয় এবং একটি অবজেক্ট তৈরি করা হয়?

**উত্তর:** ডার্ট একটি অবজেক্ট-ওরিয়েন্টেড প্রোগ্রামিং (OOP) ভাষা, এবং `class` এবং `object` OOP এর দুটি মৌলিক ধারণা।

*   **ক্লাস (Class):** ক্লাস হলো অবজেক্ট তৈরির জন্য একটি ব্লুপ্রিন্ট বা টেমপ্লেট। এটি ডেটা (অ্যাট্রিবিউট বা প্রপার্টি) এবং ঐ ডেটার উপর কাজ করার জন্য মেথড (ফাংশন) ডিফাইন করে। ক্লাস ডেটা এবং মেথডকে একসাথে ধারণ করে (encapsulation)।

    **ক্লাস ডিক্লেয়ারেশন:** `class` কীওয়ার্ড ব্যবহার করে একটি ক্লাস ডিক্লেয়ার করা হয়, তারপরে ক্লাসের নাম এবং কার্লি ব্র্যাকেটের মধ্যে ক্লাস মেম্বার (ভেরিয়েবল এবং মেথড) ডিফাইন করা হয়।
    
```
dart
    class Car {
      // Attributes (Instance variables)
      String brand;
      String model;
      int year;

      // Constructor
      Car(this.brand, this.model, this.year);

      // Method
      void displayInfo() {
        print('Car: $year $brand $model');
      }
    }
    
```
*   **অবজেক্ট (Object):** অবজেক্ট হলো একটি ক্লাসের ইনস্ট্যান্স (instance)। যখন একটি ক্লাস থেকে একটি অবজেক্ট তৈরি করা হয়, তখন মেমোরিতে ঐ ক্লাসের ডেটা এবং মেথডগুলির জন্য জায়গা তৈরি হয়। প্রতিটি অবজেক্টের নিজস্ব স্টেট (প্রপার্টি ভ্যালু) থাকে।

    **অবজেক্ট তৈরি করা:** একটি ক্লাসের নাম এবং প্যারামিটারসহ কনস্ট্রাক্টর কল করে `new` কীওয়ার্ড (যা ঐচ্ছিক) ব্যবহার করে একটি অবজেক্ট তৈরি করা হয়।
```
dart
    // Creating an object (instance) of the Car class
    Car myCar = Car('Toyota', 'Corolla', 2022);

    // Accessing object properties and methods
    print(myCar.brand); // Output: Toyota
    myCar.displayInfo(); // Output: Car: 2022 Toyota Corolla
    
```
এখানে `myCar` হলো `Car` ক্লাসের একটি অবজেক্ট। `myCar` এর নিজস্ব `brand`, `model`, এবং `year` প্রপার্টি রয়েছে এবং এটি `displayInfo` মেথড ব্যবহার করতে পারে।

#### প্রশ্ন ৬: ডার্টে কনস্ট্রাক্টর কি এবং বিভিন্ন ধরণের কনস্ট্রাক্টর উদাহরণ সহ ব্যাখ্যা করুন।

**উত্তর:** কনস্ট্রাক্টর হলো ক্লাসের একটি বিশেষ মেথড যা একটি অবজেক্ট তৈরি করার সময় কল করা হয়। এর প্রধান কাজ হলো অবজেক্টের ইনস্ট্যান্স ভ্যারিয়েবলগুলিকে ইনিশিয়ালাইজ করা। ডার্টে কয়েক ধরণের কনস্ট্রাক্টর রয়েছে:

*   **ডিফল্ট কনস্ট্রাক্টর (Default Constructor):** যদি আপনি কোনো কনস্ট্রাক্টর ডিক্লেয়ার না করেন, তাহলে ডার্ট স্বয়ংক্রিয়ভাবে একটি পাবলিক নো-আর্গুমেন্ট ডিফল্ট কনস্ট্রাক্টর সরবরাহ করে।
    
```
dart
    class Person {
      // No constructor declared, Dart provides a default one
      String name = 'Unknown';
    }
    Person p = Person(); // Using the default constructor
    
```
*   **প্যারামিটারাইজড কনস্ট্রাক্টর (Parameterized Constructor):** এটি প্যারামিটার গ্রহণ করে যা ইনস্ট্যান্স ভ্যারিয়েবল ইনিশিয়ালাইজ করতে ব্যবহৃত হয়।
```
dart
    class Dog {
      String name;
      String breed;

      // Parameterized constructor
      Dog(this.name, this.breed); // Shorthand syntax for assigning parameters to instance variables

      void bark() {
        print('$name says Woof!');
      }
    }
    Dog myDog = Dog('Buddy', 'Labrador');
    myDog.bark(); // Output: Buddy says Woof!
    
```
*   **নামড কনস্ট্রাক্টর (Named Constructor):** একটি ক্লাসের একাধিক কনস্ট্রাক্টর থাকতে পারে, যা নাম ব্যবহার করে আলাদা করা হয়। এটি অবজেক্ট তৈরির জন্য বিভিন্ন উপায় সরবরাহ করে।
```
dart
    class Point {
      double x;
      double y;

      // Default constructor
      Point(this.x, this.y);

      // Named constructor for origin point
      Point.origin() : this(0.0, 0.0);

      // Named constructor from JSON map
      Point.fromJson(Map<String, double> json)
          : x = json['x']!,
            y = json['y']!;
    }
    Point p1 = Point(1.0, 2.0);
    Point origin = Point.origin(); // Using the named constructor
    Point p2 = Point.fromJson({'x': 5.0, 'y': 10.0});
    
```
*   **ফ্যাক্টরি কনস্ট্রাক্টর (Factory Constructor):** `factory` কীওয়ার্ড ব্যবহার করে ডিক্লেয়ার করা হয়। এটি একটি নতুন ইনস্ট্যান্স তৈরি করতে পারে বা বিদ্যমান ইনস্ট্যান্স রিটার্ন করতে পারে। এটি কনস্ট্রাক্টরের মতো বাধ্যতামূলকভাবে একটি নতুন ইনস্ট্যান্স তৈরি করে না। এটি Singleton প্যাটার্ন বা সাবক্লাস রিটার্ন করার জন্য উপযোগী।
    
```
dart
    class Logger {
      static final Map<String, Logger> _cache = <String, Logger>{};

      final String name;

      factory Logger(String name) {
        if (_cache.containsKey(name)) {
          return _cache[name]!;
        } else {
          final logger = Logger._internal(name);
          _cache[name] = logger;
          return logger;
        }
      }

      // Private constructor
      Logger._internal(this.name);

      void log(String message) {
        print('[$name] $message');
      }
    }
    Logger logger1 = Logger('App');
    Logger logger2 = Logger('App'); // Returns the same instance as logger1
    print(identical(logger1, logger2)); // Output: true
    
```
#### প্রশ্ন ৭: ডার্টে ইনহেরিটেন্স (Inheritance) কিভাবে কাজ করে? একটি উদাহরণ দিন।

**উত্তর:** ইনহেরিটেন্স হলো অবজেক্ট-ওরিয়েন্টেড প্রোগ্রামিংয়ের একটি মৌলিক নীতি যা একটি নতুন ক্লাসকে বিদ্যমান ক্লাস থেকে প্রপার্টি এবং মেথড উত্তরাধিকারসূত্রে পেতে দেয়। যে ক্লাস প্রপার্টি এবং মেথড প্রদান করে তাকে **সুপারক্লাস (superclass)**, **প্যারেন্ট ক্লাস (parent class)**, বা **বেস ক্লাস (base class)** বলা হয়। যে নতুন ক্লাসটি উত্তরাধিকারসূত্রে পায় তাকে **সাবক্লাস (subclass)**, **চাইল্ড ক্লাস (child class)**, বা **ডিরাইভড ক্লাস (derived class)** বলা হয়।

ডার্টে ইনহেরিটেন্স বাস্তবায়ন করতে `extends` কীওয়ার্ড ব্যবহার করা হয়।

**উদাহরণ:**
ধরা যাক আমাদের একটি `Animal` ক্লাস আছে এবং আমরা `Dog` এবং `Cat` এর মতো বিশেষ প্রাণীদের জন্য ক্লাস তৈরি করতে চাই। `Dog` এবং `Cat` ক্লাসগুলি `Animal` ক্লাসের সাধারণ বৈশিষ্ট্যগুলি (যেমন: নাম) এবং আচরণগুলি (যেমন: খাওয়া) উত্তরাধিকারসূত্রে পেতে পারে।
```
dart
// Superclass (Parent Class)
class Animal {
  String name;

  Animal(this.name);

  void eat() {
    print('$name is eating.');
  }
}

// Subclass (Child Class) inheriting from Animal
class Dog extends Animal {
  String breed;

  Dog(String name, this.breed) : super(name); // Calling the superclass constructor

  void bark() {
    print('$name says Woof!');
  }

  // Overriding the eat method from the superclass
  @override
  void eat() {
    print('$name is eating dog food.');
  }
}

// Another Subclass inheriting from Animal
class Cat extends Animal {
  int lives;

  Cat(String name, this.lives) : super(name);

  void meow() {
    print('$name says Meow!');
  }
}

void main() {
  Dog myDog = Dog('Buddy', 'Labrador');
  myDog.eat(); // Output: Buddy is eating dog food. (Overridden method)
  myDog.bark(); // Output: Buddy says Woof!

  Cat myCat = Cat('Whiskers', 9);
  myCat.eat(); // Output: Whiskers is eating. (Inherited method)
  myCat.meow(); // Output: Whiskers says Meow!
}
```
এই উদাহরণে, `Dog` এবং `Cat` ক্লাসগুলি `Animal` ক্লাস থেকে `name` প্রপার্টি এবং `eat()` মেথড উত্তরাধিকারসূত্রে পেয়েছে। `Dog` ক্লাস `eat()` মেথডটিকে ওভাররাইড করেছে তার নিজস্ব বাস্তবায়ন প্রদান করার জন্য। `super(name)` ব্যবহার করে সাবক্লাস কনস্ট্রাক্টর থেকে সুপারক্লাস কনস্ট্রাক্টরকে কল করা হয়।

#### প্রশ্ন ৮: ডার্টে অ্যাবস্ট্রাক্ট ক্লাস (Abstract Class) কি? উদাহরণ সহ ব্যাখ্যা করুন।

**উত্তর:** ডার্টে অ্যাবস্ট্রাক্ট ক্লাস হলো একটি ক্লাস যা সরাসরি ইনস্ট্যান্সিয়েট (অবজেক্ট তৈরি) করা যায় না। এটি অন্য ক্লাসের জন্য একটি বেস ক্লাস হিসেবে কাজ করে এবং এটিতে অ্যাবস্ট্রাক্ট মেথড থাকতে পারে (বা নাও থাকতে পারে)। অ্যাবস্ট্রাক্ট মেথড হলো সেই মেথড যার শুধুমাত্র সিগনেচার (নাম, রিটার্ন টাইপ, প্যারামিটার) ডিফাইন করা থাকে কিন্তু কোনো বডি থাকে না। সাবক্লাসগুলিকে অবশ্যই এই অ্যাবস্ট্রাক্ট মেথডগুলি বাস্তবায়ন (implement) করতে হবে।

অ্যাবস্ট্রাক্ট ক্লাস ডিক্লেয়ার করার জন্য `abstract` কীওয়ার্ড ব্যবহার করা হয়।

**বৈশিষ্ট্য:**
*   `abstract` কীওয়ার্ড ব্যবহার করে ডিক্লেয়ার করা হয়।
*   সরাসরি ইনস্ট্যান্সিয়েট করা যায় না।
*   এটিতে অ্যাবস্ট্রাক্ট মেথড থাকতে পারে (বডি ছাড়া)।
*   এটিতে নন-অ্যাবস্ট্রাক্ট মেথড এবং ভ্যারিয়েবলও থাকতে পারে।
*   অন্যান্য ক্লাস অ্যাবস্ট্রাক্ট ক্লাস থেকে উত্তরাধিকারসূত্রে পেতে (`extends`) পারে এবং অ্যাবস্ট্রাক্ট মেথডগুলি বাস্তবায়ন করতে বাধ্য থাকে।

**উদাহরণ:**
ধরা যাক আমাদের একটি অ্যাবস্ট্রাক্ট ক্লাস `Shape` আছে যার একটি সাধারণ মেথড `calculateArea()` রয়েছে যা প্রতিটি শেপের জন্য আলাদাভাবে বাস্তবায়ন করা প্রয়োজন।

```
dart
// Abstract class
abstract class Shape {
  // Abstract method (no body)
  double calculateArea();

  // Non-abstract method
  void display() {
    print('This is a shape.');
  }
}

// Concrete class inheriting from Shape
class Circle extends Shape {
  double radius;

  Circle(this.radius);

  // Implementing the abstract method
  @override
  double calculateArea() {
    return 3.14 * radius * radius;
  }
}

// Concrete class inheriting from Shape
class Rectangle extends Shape {
  double width;
  double height;

  Rectangle(this.width, this.height);

  // Implementing the abstract method
  @override
  double calculateArea() {
    return width * height;
  }
}

void main() {
  // Cannot create an instance of an abstract class
  // Shape s = Shape(); // Error

  Circle c = Circle(5.0);
  print('Area of Circle: ${c.calculateArea()}'); // Output: Area of Circle: 78.5
  c.display(); // Output: This is a shape.

  Rectangle r = Rectangle(4.0, 6.0);
  print('Area of Rectangle: ${r.calculateArea()}'); // Output: Area of Rectangle: 24.0
  r.display(); // Output: This is a shape.
}
```
এই উদাহরণে, `Shape` হলো একটি অ্যাবস্ট্রাক্ট ক্লাস যা `calculateArea()` নামে একটি অ্যাবস্ট্রাক্ট মেথড ডিফাইন করে। `Circle` এবং `Rectangle` ক্লাসগুলি `Shape` থেকে উত্তরাধিকারসূত্রে পায় এবং অবশ্যই `calculateArea()` মেথড তাদের নিজস্ব উপায়ে বাস্তবায়ন করে। আমরা সরাসরি `Shape` ক্লাসের কোনো অবজেক্ট তৈরি করতে পারিনি।

#### প্রশ্ন ৯: ডার্টে ইন্টারফেসের ধারণা ব্যাখ্যা করুন। অ্যাবস্ট্রাক্ট ক্লাস এবং ইন্টারফেসের মধ্যে পার্থক্য কি?

**উত্তর:** ডার্টে ইন্টারফেস হলো একটি কনট্রাক্ট যা একটি ক্লাস ইমপ্লিমেন্ট করতে পারে। একটি ইন্টারফেস ডিফাইন করে যে একটি ক্লাসে কি কি মেথড এবং প্রপার্টি থাকতে হবে, কিন্তু তাদের বাস্তবায়ন (implementation) সরবরাহ করে না। ডার্টে কোনো ডেডিকেটেড `interface` কীওয়ার্ড নেই। পরিবর্তে, যেকোনো ক্লাসকে একটি ইন্টারফেস হিসেবে ব্যবহার করা যায়। যখন একটি ক্লাস অন্য ক্লাসকে ইন্টারফেস হিসেবে ব্যবহার করে, তখন তাকে ঐ ক্লাসের সমস্ত পাবলিক মেম্বার ইমপ্লিমেন্ট করতে হয়।

**ইন্টারফেস ব্যবহার:**
ডার্টে `implements` কীওয়ার্ড ব্যবহার করে ইন্টারফেস বাস্তবায়ন করা হয়।

**উদাহরণ:**
ধরা যাক আমাদের একটি ক্লাস `Printable` আছে যা একটি ইন্টারফেস হিসেবে ব্যবহৃত হবে।
```
dart
// This class acts as an interface
class Printable {
  void printDocument() {
    // This is like a contract, the implementing class must provide the body
  }

  String get format; // Interface can also have abstract getters/setters
}

// Class implementing the Printable interface
class Report implements Printable {
  @override
  void printDocument() {
    print('Printing report...');
  }

  @override
  String get format => 'PDF'; // Implementing the getter
}

void main() {
  Report r = Report();
  r.printDocument(); // Output: Printing report...
  print('Format: ${r.format}'); // Output: Format: PDF
}
```
এই উদাহরণে, `Report` ক্লাস `Printable` ইন্টারফেস ইমপ্লিমেন্ট করেছে, তাই এটিকে অবশ্যই `printDocument()` মেথড এবং `format` গেটার বাস্তবায়ন করতে হয়েছে।

**অ্যাবস্ট্রাক্ট ক্লাস এবং ইন্টারফেসের মধ্যে পার্থক্য:**

| বৈশিষ্ট্য          | অ্যাবস্ট্রাক্ট ক্লাস (Abstract Class)                                 | ইন্টারফেস (Interface)                                                      |
| :----------------- | :------------------------------------------------------------------- | :------------------------------------------------------------------------- |
| **ইনস্ট্যান্সিয়েশন** | সরাসরি ইনস্ট্যান্সিয়েট করা যায় না।                                    | সরাসরি ইনস্ট্যান্সিয়েট করা যায় না।                                            |
| **বাস্তবায়ন (Implementation)** | এটিতে অ্যাবস্ট্রাক্ট এবং নন-অ্যাবস্ট্রাক্ট মেথড থাকতে পারে। কিছু মেথডের বডি থাকতে পারে। | এটিতে শুধুমাত্র অ্যাবস্ট্রাক্ট মেথড থাকে (বডি ছাড়া)। বাস্তবায়নকারী ক্লাসকে বডি প্রদান করতে হয়। |
| **উত্তরাধিকার/বাস্তবায়ন** | একটি ক্লাস শুধুমাত্র একটি অ্যাবস্ট্রাক্ট ক্লাস থেকে `extends` করতে পারে। | একটি ক্লাস একাধিক ইন্টারফেস `implements` করতে পারে।                        |
| **উদ্দেশ্য**       | একটি বেস ক্লাস হিসেবে কাজ করে যা সাধারণ আচরণ এবং বৈশিষ্ট্য প্রদান করে, তবে কিছু অংশ সাবক্লাস দ্বারা বাস্তবায়নের জন্য রেখে দেয়। | একটি কনট্রাক্ট ডিফাইন করে যা বাস্তবায়নকারী ক্লাসকে নির্দিষ্ট মেথড এবং প্রপার্টি থাকার নিশ্চয়তা দেয়। |
| **গঠন**           | `abstract class ClassName { ... }`                                    | ডার্টে কোনো ডেডিকেটেড `interface` কীওয়ার্ড নেই। যেকোনো ক্লাসকে `implements` কীওয়ার্ড ব্যবহার করে ইন্টারফেস হিসেবে ব্যবহার করা হয়। |

সংক্ষেপে, অ্যাবস্ট্রাক্ট ক্লাস একটি "ইজ-এ" (is-a) সম্পর্ক তৈরি করে (যেমন: একটি `Dog` একটি `Animal`) এবং কোডের কিছু অংশ শেয়ার করতে সাহায্য করে, যখন ইন্টারফেস একটি "হ্যাস-এ" (has-a) বা "ক্যান-ডু" (can-do) সম্পর্ক তৈরি করে (যেমন: একটি `Report` `Printable`)।

#### প্রশ্ন ১০: ডার্টে মিক্সিন (Mixin) কি? মিক্সিনের ব্যবহার এবং সুবিধা ব্যাখ্যা করুন।

**উত্তর:** ডার্টে মিক্সিন হলো একটি উপায় যা একটি ক্লাসকে অন্য ক্লাস hierarchy তে হস্তক্ষেপ না করে একাধিক ক্লাসের কোড পুনরায় ব্যবহার করতে দেয়। অন্য ভাষায়, মিক্সিন হলো এমন কোড যা একাধিক ক্লাসে ব্যবহার করা যেতে পারে, এমনকি যদি ঐ ক্লাসগুলি একে অপরের সাথে সম্পর্কিত না হয় (যেমন, একই প্যারেন্ট ক্লাস নেই)।

মিক্সিন ডিক্লেয়ার করার জন্য `mixin` কীওয়ার্ড ব্যবহার করা হয়। একটি ক্লাস বা অন্য মিক্সিন `on` কীওয়ার্ড ব্যবহার করে নির্দিষ্ট টাইপের মিক্সিন ব্যবহার করতে পারে, যা ঐ মিক্সিনের জন্য কিছু পূর্বশর্ত সেট করে। একটি ক্লাসে মিক্সিন যুক্ত করতে `with` কীওয়ার্ড ব্যবহার করা হয়।

**উদাহরণ:**
ধরা যাক আমাদের দুটি আলাদা ক্লাস `Car` এবং `Bike` আছে, কিন্তু উভয়ই একটি `Engine` এর সাথে সম্পর্কিত কিছু ফাংশনালিটি শেয়ার করতে পারে, যেমন `start()` এবং `stop()`। আমরা এই ফাংশনালিটি একটি মিক্সিনে রাখতে পারি।

```
dart
// Mixin
mixin Engine {
  void start() {
    print('Engine started.');
  }

  void stop() {
    print('Engine stopped.');
  }
}

// Class using the Mixin
class Car with Engine {
  void drive() {
    print('Car is driving.');
  }
}

// Another Class using the Mixin
class Bike with Engine {
  void ride() {
    print('Bike is riding.');
  }
}

void main() {
  Car myCar = Car();
  myCar.start(); // Accessing method from Mixin
  myCar.drive();
  myCar.stop();  // Accessing method from Mixin

  Bike myBike = Bike();
  myBike.start(); // Accessing method from Mixin
  myBike.ride();
  myBike.stop();  // Accessing method from Mixin
}
```
এই উদাহরণে, `Engine` হলো একটি মিক্সিন যা `start()` এবং `stop()` মেথড ধারণ করে। `Car` এবং `Bike` ক্লাসগুলি `with Engine` ব্যবহার করে এই মিক্সিন ব্যবহার করছে, ফলে তারা ঐ মেথডগুলি অ্যাক্সেস করতে পারছে।

**মিক্সিনের সুবিধা:**
*   **কোড পুনঃব্যবহার:** একাধিক असंबंधित ক্লাসের মধ্যে কোড শেয়ার করার এটি একটি শক্তিশালী উপায়।
*   **মাল্টিপল ইনহেরিটেন্সের বিকল্প:** ডার্ট সরাসরি মাল্টিপল ইনহেরিটেন্স সমর্থন করে না (একটি ক্লাস একাধিক প্যারেন্ট থেকে extends করতে পারে না)। মিক্সিন মাল্টিপল ইনহেরিটেন্সের মতো সুবিধা প্রদান করে একটি ক্লাসে একাধিক মিক্সিনের ফাংশনালিটি যুক্ত করার মাধ্যমে।
*   **নমনীয়তা:** এটি ক্লাসের hierarchy তে পরিবর্তন না করে নতুন বৈশিষ্ট্য যুক্ত করতে দেয়।

মিক্সিন সাধারণত ক্ষুদ্র এবং ফোকাসড ফাংশনালিটির জন্য ব্যবহৃত হয় যা একাধিক ক্লাস শেয়ার করে।