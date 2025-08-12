**প্রশ্ন:** Dart এ inheritance বলতে কী বোঝায় এবং এটি কীভাবে ব্যবহার করা হয়?

**উত্তর:** Inheritance হলো object-oriented programming এর একটি মৌলিক ধারণা যেখানে একটি ক্লাস (child বা subclass) অন্য একটি ক্লাস (parent বা superclass) এর বৈশিষ্ট্য (properties) এবং আচরণ (methods) উত্তরাধিকার সূত্রে পায়। এটি কোড পুনঃব্যবহার (code reusability) এবং সম্পর্ক স্থাপন (establishing relationships) এর জন্য ব্যবহৃত হয়।

Dart এ `extends` কিওয়ার্ড ব্যবহার করে inheritance প্রয়োগ করা হয়।

```
dart
class Animal {
  void eat() {
    print("The animal eats.");
  }
}

class Dog extends Animal {
  void bark() {
    print("The dog barks.");
  }
}

void main() {
  var dog = Dog();
  dog.eat(); // Inherited from Animal
  dog.bark();
}
```
এখানে `Dog` ক্লাস `Animal` ক্লাসকে extend করে, তাই `Dog` অবজেক্ট `eat()` মেথডটি ব্যবহার করতে পারে।

**প্রশ্ন:** Dart এ interface বলতে কী বোঝায়? Dart কি explicit interface সমর্থন করে?

**উত্তর:** Dart এ explicit interface কিওয়ার্ড নেই, তবে implicitly যেকোনো ক্লাসকে interface হিসেবে ব্যবহার করা যেতে পারে। এর মানে হলো, আপনি একটি ক্লাসের সমস্ত পাবলিক মেথড এবং প্রোপার্টি ব্যবহার করে অন্য একটি ক্লাস implement করতে পারেন। `implements` কিওয়ার্ড ব্যবহার করে এটি করা হয়। Interface একটি contract সংজ্ঞায়িত করে যা implementing ক্লাসকে অবশ্যই মেনে চলতে হবে।

```
dart
class Printable {
  void printInfo();
}

class User implements Printable {
  String name;

  User(this.name);

  @override
  void printInfo() {
    print("User name: $name");
  }
}

void main() {
  var user = User("Alice");
  user.printInfo();
}
```
এখানে `User` ক্লাস `Printable` কে implement করে, তাই এটিকে অবশ্যই `printInfo()` মেথডটি প্রদান করতে হবে।

**প্রশ্ন:** Abstract class কী এবং কেন এটি ব্যবহার করা হয়?

**উত্তর:** Abstract class হলো এমন একটি ক্লাস যা সরাসরি ইনস্ট্যানশিয়েট (instantiate) করা যায় না। এটি মূলত অন্যান্য ক্লাসের জন্য একটি ব্লুপ্রিন্ট (blueprint) হিসেবে কাজ করে। Abstract class এ abstract methods থাকতে পারে, যার কোনো implementation থাকে না এবং concrete (নন-abstract) methods ও থাকতে পারে। Abstract methods অবশ্যই abstract class কে extend করা subclass গুলিতে implement করতে হবে। `abstract` কিওয়ার্ড ব্যবহার করে abstract class সংজ্ঞায়িত করা হয়।

```
dart
abstract class Shape {
  void draw(); // Abstract method
  void display() {
    print("This is a shape.");
  }
}

class Circle extends Shape {
  @override
  void draw() {
    print("Drawing a circle.");
  }
}

void main() {
  // var shape = Shape(); // Error: Cannot instantiate abstract class
  var circle = Circle();
  circle.draw();
  circle.display();
}
```
এখানে `Shape` একটি abstract class এবং `draw()` একটি abstract method। `Circle` ক্লাস `Shape` কে extend করে `draw()` মেথডটি implement করেছে।

**প্রশ্ন:** Mixin কী এবং এটি কীভাবে inheritance থেকে আলাদা?

**উত্তর:** Mixin হলো এক ধরনের ক্লাস যা অন্য ক্লাসের ক্ষমতা প্রসারিত করতে ব্যবহৃত হয়। Inheritance যেখানে "is a" সম্পর্ক স্থাপন করে (যেমন Dog is an Animal), Mixin সেখানে "has a" বা "can do" সম্পর্ক বোঝায় (যেমন A class can do something defined in the mixin)। Mixin সরাসরি ইনস্ট্যানশিয়েট করা যায় না এবং এটি একাধিক inheritance এর সমস্যা সমাধানের একটি উপায়। `with` কিওয়ার্ড ব্যবহার করে mixin যুক্ত করা হয়।
```
dart
mixin Walker {
  void walk() {
    print("Walking...");
  }
}

mixin Swimmer {
  void swim() {
    print("Swimming...");
  }
}

class Human with Walker, Swimmer {
  // Human can walk and swim
}

void main() {
  var human = Human();
  human.walk();
  human.swim();
}
```
এখানে `Human` ক্লাস `Walker` এবং `Swimmer` mixin দুটি ব্যবহার করে উভয়টির ক্ষমতা অর্জন করেছে।

**প্রশ্ন:** Asynchronous programming কী এবং Dart এ এটি কীভাবে হ্যান্ডেল করা হয়?

**উত্তর:** Asynchronous programming হলো এমন প্রোগ্রামিং যেখানে একটি অপারেশন শুরু হওয়ার পর প্রোগ্রাম অন্য কাজ করতে থাকে এবং অপারেশনটি সম্পন্ন হলে তার ফলাফল হ্যান্ডেল করে। এটি অ্যাপ্লিকেশনকে নন-ব্লকিং (non-blocking) করে তোলে, যা UI ফ্রিজ হওয়া থেকে রক্ষা করে এবং পারফরম্যান্স উন্নত করে। Dart Future, Stream, async, এবং await এর মাধ্যমে asynchronous programming হ্যান্ডেল করে।

**প্রশ্ন:** Future কী এবং এটি asynchronous programming এ কীভাবে কাজ করে?

**উত্তর:** Future একটি অবজেক্ট যা represent করে একটি asynchronous অপারেশন যা ভবিষ্যতে একটি ভ্যালু produce করবে অথবা একটি error throw করবে। এটি মূলত একটি প্রমিজ (promise) যা একটি অপারেশনের ফলাফল সম্পর্কে বলে। Future এর দুটি অবস্থা থাকে: uncompleted এবং completed। Completed state আবার সফলভাবে সম্পন্ন (with a value) বা ব্যর্থ (with an error) হতে পারে।

```
dart
Future<String> fetchUserData() {
  return Future.delayed(Duration(seconds: 2), () {
    return "User Data";
  });
}

void main() {
  print("Fetching data...");
  fetchUserData().then((data) {
    print("Data received: $data");
  }).catchError((error) {
    print("Error: $error");
  });
  print("Continuing other tasks...");
}
```
এখানে `fetchUserData` একটি Future রিটার্ন করে যা 2 সেকেন্ড পর সম্পন্ন হবে। `.then()` ব্যবহার করে আমরা Future সম্পন্ন হওয়ার পর প্রাপ্ত ডেটা হ্যান্ডেল করি।

**প্রশ্ন:** Stream কী এবং Future এর থেকে এটি কীভাবে আলাদা?

**উত্তর:** Stream হলো asynchronous data event গুলোর একটি সিকোয়েন্স। Future শুধুমাত্র একটি single asynchronous ভ্যালু produce করে, যেখানে Stream সময়ের সাথে সাথে এক বা একাধিক ভ্যালু emit করতে পারে। এটি মূলত ডেটা স্ট্রিমের (data stream) মতো কাজ করে, যেখানে আপনি ডেটাতে subscribe করে নতুন ডেটা উপলব্ধ হলে তা প্রক্রিয়া করতে পারেন। Real-time data যেমন user input, database changes, বা network requests হ্যান্ডেল করার জন্য Stream খুব উপযোগী।
```
dart
Stream<int> countStream(int max) async* {
  for (int i = 1; i <= max; i++) {
    await Future.delayed(Duration(seconds: 1));
    yield i; // Emit a value
  }
}

void main() {
  print("Starting stream...");
  countStream(5).listen((number) {
    print("Received: $number");
  }, onDone: () {
    print("Stream finished.");
  }, onError: (error) {
    print("Stream error: $error");
  });
  print("Continuing other tasks...");
}
```
এখানে `countStream` একটি Stream রিটার্ন করে যা প্রতি সেকেন্ডে একটি করে সংখ্যা emit করে। `.listen()` ব্যবহার করে আমরা Stream এ subscribe করি এবং emitted value গুলো হ্যান্ডেল করি।

**প্রশ্ন:** async এবং await কিওয়ার্ডগুলি asynchronous programming এ কীভাবে সাহায্য করে?

**উত্তর:** `async` এবং `await` কিওয়ার্ডগুলি asynchronous কোড লিখতে এবং পড়তে সহজ করে তোলে। `async` কিওয়ার্ড একটি ফাংশনকে asynchronous বানায়, যার ফলে সেই ফাংশনটি Future রিটার্ন করে। `await` কিওয়ার্ডটি asynchronous ফাংশনের ভিতরে ব্যবহার করা হয় একটি Future এর জন্য অপেক্ষা করার জন্য, যেন মনে হয় কোড synchronously execute হচ্ছে। এটি `.then()` বা `.catchError()` ব্যবহারের চেয়ে কোডকে অনেক বেশি রিডেবল করে তোলে।

```
dart
Future<String> fetchUserData() {
  return Future.delayed(Duration(seconds: 2), () {
    return "User Data";
  });
}

Future<String> fetchOrderData() {
  return Future.delayed(Duration(seconds: 3), () {
    return "Order Data";
  });
}

Future<void> getUserAndOrderData() async {
  print("Fetching user data...");
  String userData = await fetchUserData(); // Wait for user data
  print("User data received: $userData");

  print("Fetching order data...");
  String orderData = await fetchOrderData(); // Wait for order data
  print("Order data received: $orderData");
}

void main() {
  getUserAndOrderData();
  print("Continuing other tasks...");
}
```
এখানে `getUserAndOrderData` একটি `async` ফাংশন। `await` ব্যবহার করে আমরা `fetchUserData` এবং `fetchOrderData` Future গুলির জন্য অপেক্ষা করি।

**প্রশ্ন:** Dart এ error handling কীভাবে করা হয় asynchronous কোডে?

**উত্তর:** Asynchronous কোডে error handling করার জন্য `try`, `catch`, এবং `finally` ব্লকগুলি ব্যবহার করা হয়, ঠিক synchronous কোডের মতো। যখন একটি Future সম্পন্ন হওয়ার সময় একটি error throw করে, তখন আপনি `catchError` ব্যবহার করতে পারেন `.then()` এর সাথে অথবা `await` এর সাথে `try...catch` ব্যবহার করতে পারেন।
```
dart
Future<String> fetchDataWithError() {
  return Future.delayed(Duration(seconds: 2), () {
    throw Exception("Failed to fetch data");
    return "Some Data"; // This line won't be reached
  });
}

Future<void> processData() async {
  try {
    String data = await fetchDataWithError();
    print("Data: $data");
  } catch (e) {
    print("Caught error: $e");
  } finally {
    print("Processing finished.");
  }
}

void main() {
  processData();
}
```
এখানে `fetchDataWithError` একটি error throw করে। `processData` ফাংশনে `try...catch` ব্লক ব্যবহার করে error টি ধরা হয়েছে। `finally` ব্লকটি error ঘটুক বা না ঘটুক, সর্বদা execute হবে।

**প্রশ্ন:** FutureBuilder এবং StreamBuilder উইজেটগুলি কী এবং কখন সেগুলি ব্যবহার করা হয়?

**উত্তর:** `FutureBuilder` এবং `StreamBuilder` হলো Flutter এর উইজেট যা asynchronous ডেটার উপর ভিত্তি করে UI আপডেট করতে ব্যবহৃত হয়।

*   **FutureBuilder:** এটি একটি Future এর ফলাফলের উপর ভিত্তি করে একটি উইজেট তৈরি করে। যখন Future সম্পন্ন হয় (সফলভাবে বা error সহ), FutureBuilder স্বয়ংক্রিয়ভাবে তার UI আপডেট করে। এটি ডেটা লোড হওয়ার জন্য অপেক্ষা করার সময় একটি লোডিং ইন্ডিকেটর দেখানো বা ডেটা উপলব্ধ হলে ডেটা প্রদর্শন করার জন্য উপযোগী।
```
dart
    FutureBuilder<String>(
      future: fetchUserData(), // Your Future
      builder: (BuildContext context, AsyncSnapshot<String> snapshot) {
        if (snapshot.connectionState == ConnectionState.waiting) {
          return CircularProgressIndicator(); // Show loading
        } else if (snapshot.hasError) {
          return Text('Error: ${snapshot.error}'); // Show error
        } else {
          return Text('Data: ${snapshot.data}'); // Show data
        }
      },
    )
    
```
*   **StreamBuilder:** এটি একটি Stream থেকে আসা ডেটার উপর ভিত্তি করে একটি উইজেট তৈরি করে। যখন Stream নতুন ডেটা emit করে, StreamBuilder স্বয়ংক্রিয়ভাবে তার UI আপডেট করে। এটি রিয়েল-টাইম ডেটা যেমন চ্যাট মেসেজ বা স্টক প্রাইস আপডেট প্রদর্শন করার জন্য উপযোগী।