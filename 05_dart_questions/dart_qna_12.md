## প্রশ্ন ১১: Dart-এ `factory` কনস্ট্রাক্টর কি এবং কখন এটি ব্যবহার করবেন?

**উত্তর:**

Dart-এ `factory` কনস্ট্রাক্টর হলো একটি বিশেষ ধরণের কনস্ট্রাক্টর যা একটি নতুন ইনস্ট্যান্স তৈরি করার পরিবর্তে বিদ্যমান ইনস্ট্যান্স রিটার্ন করতে পারে বা একটি সাবক্লাসের ইনস্ট্যান্স রিটার্ন করতে পারে। এটি সরাসরি ক্লাসের নতুন ইনস্ট্যান্স তৈরি করে না, বরং কনস্ট্রাক্টরের লজিক অনুযায়ী ইনস্ট্যান্স রিটার্ন করে।

`factory` কনস্ট্রাক্টর ব্যবহার করার প্রধান কারণগুলি হল:

1.  **বিদ্যমান ইনস্ট্যান্স রিটার্ন করা:** যখন আপনি চান একটি ক্লাসের শুধুমাত্র একটি ইনস্ট্যান্স থাকুক (সিঙ্গেলটন প্যাটার্ন) অথবা যখন ইনপুটের উপর ভিত্তি করে আগে তৈরি করা ইনস্ট্যান্স রিটার্ন করতে চান।
2.  **সাবক্লাসের ইনস্ট্যান্স রিটার্ন করা:** যখন আপনি চান প্যারেন্ট ক্লাস থেকে একটি কনস্ট্রাক্টর কল করে ইনপুটের উপর ভিত্তি করে ভিন্ন ভিন্ন সাবক্লাসের ইনস্ট্যান্স তৈরি করতে।
3.  **লজিক সহ ইনস্ট্যান্স তৈরি করা:** যখন ইনস্ট্যান্স তৈরির সময় কিছু জটিল লজিক চালানোর প্রয়োজন হয় যা সাধারণ কনস্ট্রাক্টরে সম্ভব নয়।

**উদাহরণ:**

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

  Logger._internal(this.name);

  void log(String message) {
    print('[$name] $message');
  }
}

void main() {
  var logger1 = Logger('UI');
  var logger2 = Logger('UI');
  var logger3 = Logger('Data');

  print(identical(logger1, logger2)); // আউটপুট: true (একই ইনস্ট্যান্স)
  print(identical(logger1, logger3)); // আউটপুট: false (ভিন্ন ইনস্ট্যান্স)
}
```
এই উদাহরণে, `Logger` ক্লাস একটি `factory` কনস্ট্রাক্টর ব্যবহার করে নিশ্চিত করে যে একই নামের জন্য শুধুমাত্র একটি `Logger` ইনস্ট্যান্স তৈরি হয়।

## প্রশ্ন ১২: Dart-এ `const` এবং `final` কী এবং তাদের মধ্যে পার্থক্য কী?

**উত্তর:**

Dart-এ `const` এবং `final` উভয়ই পরিবর্তনশীলকে অপরিবর্তনীয় (immutable) করতে ব্যবহৃত হয়, কিন্তু তাদের মধ্যে কিছু গুরুত্বপূর্ণ পার্থক্য রয়েছে:

*   **`final`:** একটি `final` পরিবর্তনশীল একবার মান নির্ধারিত হওয়ার পর আর পরিবর্তন করা যায় না। এই মান রানটাইমে নির্ধারণ করা যেতে পারে।
    
```
dart
    final int x = 10; // রানটাইমে মান সেট করা হয়েছে
    // x = 20; // Error: A final variable can only be set once.

    final DateTime now = DateTime.now(); // রানটাইমে নির্ধারণ করা হবে
    
```
*   **`const`:** একটি `const` পরিবর্তনশীল অবশ্যই কম্পাইল-টাইম কনস্ট্যান্ট হতে হবে। এর মান কম্পাইল করার আগেই জানা থাকতে হবে। `const` পরিবর্তনশীল implicitly `final` হয়। `const` শুধুমাত্র primitive types (number, string, boolean, null) এর সাথে ব্যবহার করা হয় না, const collections (List, Map, Set) এর সাথেও ব্যবহার করা যেতে পারে। `const` ব্যবহার করলে কম্পাইল-টাইমে অপটিমাইজেশন হয়।
```
dart
    const double PI = 3.14159; // কম্পাইল-টাইমে মান সেট করা হয়েছে
    // PI = 3.0; // Error: Constant variables can't be assigned a value.

    const List<int> numbers = [1, 2, 3]; // const List
    // numbers.add(4); // Error: Unsupported operation: Cannot add to an unmodifiable list
    
```
**মূল পার্থক্য:**

| বৈশিষ্ট্য        | `final`                                    | `const`                                        |
| :------------- | :----------------------------------------- | :--------------------------------------------- |
| মান নির্ধারণ     | রানটাইম বা কম্পাইল-টাইমে নির্ধারণ করা যায়  | অবশ্যই কম্পাইল-টাইমে নির্ধারণ করতে হবে           |
| অপটিমাইজেশন    | বিশেষ কোনো কম্পাইল-টাইম অপটিমাইজেশন নেই  | কম্পাইল-টাইম অপটিমাইজেশন হয়, মেমরি বাঁচায়       |
| ইমুটেবিলিটি    | পরিবর্তনশীল নিজে অপরিবর্তনীয়              | পরিবর্তনশীল এবং তার ভেতরের মানও অপরিবর্তনীয় (collections এর ক্ষেত্রে) |
| ব্যবহারের ক্ষেত্র | যখন মান রানটাইমে নির্ধারণ করা প্রয়োজন হয় | যখন মান কম্পাইল-টাইমে জানা থাকে এবং পারফরম্যান্স গুরুত্বপূর্ণ |

**সংক্ষেপে:** `const` `final`-এর চেয়ে কঠোর। যখনই সম্ভব `const` ব্যবহার করা ভালো কারণ এটি পারফরম্যান্সে সহায়তা করে।

## প্রশ্ন ১৩: Dart-এ Null Safety কি এবং এটি কীভাবে কাজ করে?

**উত্তর:**

Null Safety হলো Dart-এর একটি বৈশিষ্ট্য যা null reference errors (NullPointerException) এড়াতে সাহায্য করে। এটি নিশ্চিত করে যে একটি ভেরিয়েবল যখন non-nullable ঘোষণা করা হয়, তখন তার মান null হতে পারে না। এটি কোডের নির্ভরযোগ্যতা এবং রক্ষণাবেক্ষণযোগ্যতা উন্নত করে।

Null Safety তিনটি প্রধান ধারণা ব্যবহার করে কাজ করে:

1.  **Nullable Types (`?`):** একটি টাইপের শেষে `?` যোগ করে এটিকে nullable ঘোষণা করা হয়। এর মানে হলো এই ভেরিয়েবলের মান সেই টাইপের হতে পারে অথবা null হতে পারে।
    
```
dart
    String? name; // Nullable String
    int? age = null; // Nullable int
    
```
2.  **Non-nullable Types:** Default ভাবে Dart-এ ভেরিয়েবলগুলি non-nullable হয়। এর মানে হলো এই ভেরিয়েবলের মান null হতে পারে না যদি না আপনি explicitly এটিকে nullable ঘোষণা করেন।
```
dart
    String requiredName = "Alice"; // Non-nullable String
    int count = 0; // Non-nullable int
    // int score = null; // Error: A value of type 'Null' can't be assigned to a variable of type 'int'.
    
```
3.  **Null Checks and Promotions:** কম্পাইলার Null Safety ব্যবহার করে কোড বিশ্লেষণ করে। যখন আপনি একটি nullable ভেরিয়েবলের উপর null check (যেমন `if (variable != null)`) করেন, কম্পাইলার তখন সেই স্কোপের মধ্যে ভেরিয়েবলটিকে non-nullable হিসেবে প্রচার (promote) করে।

**উদাহরণ:**
```
dart
String? getName(int id) {
  if (id == 1) {
    return "Alice";
  }
  return null;
}

void printName(int id) {
  String? name = getName(id);
  if (name != null) {
    // এখানে 'name' non-nullable String হিসেবে প্রচার করা হয়েছে
    print("Name: ${name.toUpperCase()}"); // .toUpperCase() safe to call
  } else {
    print("Name not found.");
  }
}

void main() {
  printName(1); // আউটপুট: Name: ALICE
  printName(2); // আউটপুট: Name not found.
}
```
এই উদাহরণে, `getName` ফাংশনটি একটি nullable String রিটার্ন করে। `printName` ফাংশনে null check (`name != null`) করার পর, কম্পাইলার জানে যে ঐ স্কোপের মধ্যে `name` null নয়, তাই আপনি নিরাপদে String methods যেমন `toUpperCase()` কল করতে পারেন।

Null Safety কোড লেখার সময় null সম্পর্কিত ত্রুটিগুলি আগে থেকে ধরতে সাহায্য করে এবং কোডকে আরও নির্ভরযোগ্য করে তোলে।

## প্রশ্ন ১৪: Dart-এ Mixins কী এবং কখন এটি ব্যবহার করবেন?

**উত্তর:**

Dart-এ Mixins হলো এক ধরণের ক্লাস যা অন্য ক্লাসে তার মেথড এবং প্রোপার্টিগুলি ভাগ করে নেওয়ার জন্য ব্যবহার করা হয়, উত্তরাধিকার (inheritance) ব্যবহার না করে। Mixins কোড পুনরায় ব্যবহারযোগ্য করে তোলে এবং multiple inheritance-এর কিছু সুবিধা প্রদান করে, কিন্তু multiple inheritance-এর জটিলতাগুলি এড়িয়ে যায়।

একটি Mixin তৈরি করতে, আপনি সাধারণত একটি ক্লাস ব্যবহার করেন (বা abstract ক্লাস), কিন্তু এটি কনস্ট্রাক্টর declare করতে পারে না (Dart 2.1 এর পর থেকে)। Mixins ব্যবহার করার জন্য `with` কীওয়ার্ড ব্যবহার করা হয়।

**Mixin ব্যবহারের কারণ:**

*   **কোড ভাগ করে নেওয়া:** একাধিক ক্লাসের মধ্যে সাধারণ কার্যকারিতা ভাগ করে নিতে।
*   **লিনিয়ার উত্তরাধিকার (Linear Inheritance) এড়ানো:** যখন আপনি একাধিক ক্লাসের কার্যকারিতা একটি ক্লাসে যোগ করতে চান কিন্তু সেই ক্লাসগুলি একই উত্তরাধিকার hierarchy-তে নেই।
*   **"has-a" সম্পর্ক মডেলিং:** যখন একটি ক্লাস অন্য ক্লাসের "বৈশিষ্ট্য" বা "কার্যকারিতা" থাকতে চায়, কিন্তু এটি সেই ক্লাসের "is-a" টাইপ নয়।

**উদাহরণ:**
```
dart
mixin Walkable {
  void walk() {
    print("Walking...");
  }
}

mixin Swimmable {
  void swim() {
    print("Swimming...");
  }
}

class Animal with Walkable, Swimmable {
  String name;
  Animal(this.name);
}

class Bird with Walkable {
  String name;
  Bird(this.name);
}

void main() {
  var dog = Animal("Dog");
  dog.walk();
  dog.swim();

  var eagle = Bird("Eagle");
  eagle.walk();
  // eagle.swim(); // Error: The method 'swim' isn't defined for the type 'Bird'.
}
```
এই উদাহরণে, `Walkable` এবং `Swimmable` Mixins তৈরি করা হয়েছে। `Animal` ক্লাস দুটি Mixins ব্যবহার করে, তাই এটি `walk()` এবং `swim()` উভয় মেথড অ্যাক্সেস করতে পারে। `Bird` ক্লাস শুধুমাত্র `Walkable` Mixin ব্যবহার করে, তাই এটি শুধুমাত্র `walk()` মেথড অ্যাক্সেস করতে পারে।

Mixins একটি শক্তিশালী টুল যা কোড পুনরায় ব্যবহারযোগ্যতা এবং মডুলারিটি উন্নত করতে পারে।

## প্রশ্ন ১৫: Dart-এ Extensions কী এবং কখন এটি ব্যবহার করবেন?

**উত্তর:**

Dart-এ Extensions হলো একটি বৈশিষ্ট্য যা আপনাকে বিদ্যমান ক্লাসগুলিতে নতুন কার্যকারিতা (মেথড, গেটার, সেটার, এবং অপারেটর) যোগ করার অনুমতি দেয়, এমনকি যদি আপনার কাছে সেই ক্লাসের সোর্স কোড না থাকে। এটি একটি syntactic sugar যা আপনাকে বিদ্যমান টাইপের উপর নতুন মেথড কল করার অনুমতি দেয়, কিন্তু এটি আসলে সেই ক্লাসের মধ্যে কোড পরিবর্তন করে না।

Extensions ব্যবহার করার প্রধান কারণগুলি হল:

*   **কোড পঠনযোগ্যতা বৃদ্ধি:** যখন আপনি একটি নির্দিষ্ট টাইপের উপর একটি সাধারণ অপারেশন বারবার করেন, আপনি এটিকে একটি extension method হিসাবে সংজ্ঞায়িত করতে পারেন যা কোডকে আরও পঠনযোগ্য করে তোলে।
*   **ইউটিলিটি ফাংশনগুলি সংগঠিত করা:** যখন আপনার কাছে কিছু ইউটিলিটি ফাংশন আছে যা একটি নির্দিষ্ট টাইপের সাথে কাজ করে, আপনি সেগুলিকে একটি extension-এর মধ্যে সংগঠিত করতে পারেন।
*   **লাইব্রেরি ক্লাসগুলিতে কার্যকারিতা যোগ করা:** যখন আপনি একটি থার্ড-পার্টি লাইব্রেরির ক্লাসে নতুন কার্যকারিতা যোগ করতে চান কিন্তু সেই লাইব্রেরির সোর্স কোড পরিবর্তন করতে পারবেন না।

**Extension তৈরি করা:**

আপনি `extension` কীওয়ার্ড ব্যবহার করে একটি extension তৈরি করেন, তারপরে extension এর নাম এবং এটি যে টাইপের জন্য প্রযোজ্য তা উল্লেখ করেন।

**উদাহরণ:**
```
dart
extension StringExtensions on String {
  String capitalize() {
    if (this.isEmpty) {
      return this;
    }
    return this[0].toUpperCase() + this.substring(1);
  }

  String toTitleCase() {
    return this.split(' ').map((word) => word.capitalize()).join(' ');
  }
}

void main() {
  String name = "hello world";
  print(name.capitalize());     // আউটপুট: Hello world
  print(name.toTitleCase());  // আউটপুট: Hello World
}
```
এই উদাহরণে, আমরা `String` টাইপের জন্য দুটি extension methods (`capitalize` এবং `toTitleCase`) তৈরি করেছি। এখন আমরা যেকোনো String variable এর উপর এই মেথডগুলি সরাসরি কল করতে পারি।

Extensions কোডকে আরও পরিপাটি এবং ব্যবহারযোগ্য করে তোলে, বিশেষ করে যখন আপনি প্রায়শই একটি নির্দিষ্ট টাইপের উপর একই অপারেশনগুলি করেন।

## প্রশ্ন ১৬: Dart-এ Isolates কী এবং কেন এটি ব্যবহার করা হয়?

**উত্তর:**

Dart-এ Isolates হলো আলাদা মেমরি হিপ সহ স্বাধীন কার্যকরন থ্রেড। Dart একটি সিঙ্গেল-থ্রেডেড ভাষা, যার মানে ডিফল্টভাবে কোড একটি প্রধান থ্রেডে চলে। দীর্ঘ সময় ধরে চলা বা গণনা-ভারী কাজগুলি যদি প্রধান থ্রেডে চলে, তাহলে UI আটকে যেতে পারে এবং অ্যাপ্লিকেশন অনভিপ্রেতভাবে প্রতিক্রিয়াশীল হতে পারে।

Isolates ব্যবহার করে আপনি এই ধরনের কাজগুলি একটি আলাদা Isolate-এ সরিয়ে নিতে পারেন, যা নিজস্ব মেমরি এবং ইভেন্ট লুপে চলে। প্রতিটি Isolate অন্যদের থেকে সম্পূর্ণ বিচ্ছিন্ন থাকে এবং তাদের মধ্যে ডেটা পাস করার একমাত্র উপায় হলো পোর্টগুলির মাধ্যমে মেসেজ পাঠানো। এটি ডেটা রেস (data races) বা লকিং সমস্যাগুলি এড়াতে সাহায্য করে।

**Isolates ব্যবহারের কারণ:**

*   **UI ব্লক না করা:** গণনা-ভারী অপারেশনগুলি প্রধান UI থ্রেড থেকে আলাদা করে UI-কে মসৃণ এবং প্রতিক্রিয়াশীল রাখা।
*   **সমান্তরাল প্রক্রিয়াকরণ:** উপলব্ধ CPU কোরগুলির সদ্ব্যবহার করে কাজগুলিকে সমান্তরালভাবে চালানো।
*   **নিরাপত্তা:** প্রতিটি Isolate তার নিজস্ব মেমরিতে কাজ করে, তাই একটি Isolate-এর ত্রুটি অন্য Isolate-কে প্রভাবিত করে না।

**উদাহরণ (ধারণাগত):**
```
dart
import 'dart:isolate';

void complexTask(SendPort sendPort) {
  // কিছু গণনা-ভারী কাজ
  int result = 0;
  for (int i = 0; i < 1000000000; i++) {
    result += i;
  }
  sendPort.send(result); // প্রধান Isolate-এ ফলাফল পাঠানো
}

void main() async {
  ReceivePort receivePort = ReceivePort();
  // নতুন Isolate তৈরি করা
  Isolate newIsolate = await Isolate.spawn(complexTask, receivePort.sendPort);

  // প্রধান Isolate থেকে মেসেজ গ্রহণ করা
  receivePort.listen((message) {
    print("Received result from Isolate: $message");
    newIsolate.kill(); // Isolate বন্ধ করা
  });

  print("Main Isolate continues...");
}
```
এই উদাহরণে, `complexTask` ফাংশনটি একটি আলাদা Isolate-এ চালানো হয়। `ReceivePort` এবং `SendPort` ব্যবহার করে প্রধান Isolate এবং নতুন Isolate-এর মধ্যে যোগাযোগ স্থাপন করা হয়। দীর্ঘ গণনা প্রধান থ্রেডকে ব্লক করে না, তাই "Main Isolate continues..." মেসেজটি দ্রুত প্রিন্ট হয়।

Isolates Dart-এ concurrency ম্যানেজ করার একটি শক্তিশালী উপায়, বিশেষ করে যখন আপনি ব্যাকগ্রাউন্ডে দীর্ঘ সময় ধরে চলা কাজগুলি সম্পাদন করতে চান।

## প্রশ্ন ১৭: Dart-এ Future এবং Stream এর মধ্যে পার্থক্য কী?

**উত্তর:**

Dart-এ `Future` এবং `Stream` উভয়ই asynchronous প্রোগ্রামিং এর জন্য ব্যবহৃত হয়, কিন্তু তারা ডেটা হ্যান্ডলিং এর ক্ষেত্রে ভিন্ন:

*   **Future:** একটি `Future` একটি single asynchronous অপারেশনের ফলাফলকে Represents করে। যখন একটি asynchronous অপারেশন সম্পন্ন হয় (সাফল্যে বা ব্যর্থতায়), একটি `Future` একটি মান (বা ত্রুটি) দিয়ে পূর্ণ হয়। `Future` এককালীন ডেটা হ্যান্ডলিং এর জন্য ব্যবহৃত হয়।
```
dart
    Future<String> fetchUserData() {
      return Future.delayed(Duration(seconds: 2), () => "User data fetched");
    }

    void main() {
      fetchUserData().then((data) {
        print(data); // 2 সেকেন্ড পর প্রিন্ট হবে
      }).catchError((error) {
        print("Error: $error");
      });
      print("Fetching user data..."); // আগে প্রিন্ট হবে
    }
    
```
*   **Stream:** একটি `Stream` asynchronously এক বা একাধিক ইভেন্ট (ডেটা বা ত্রুটি) এর একটি সিকোয়েন্সকে Represents করে। এটি সময়ের সাথে সাথে একাধিক ডেটা মান নির্গত করতে পারে। `Stream` ইভেন্ট-ভিত্তিক ডেটা হ্যান্ডলিং এর জন্য ব্যবহৃত হয়, যেমন UI ইভেন্ট, নেটওয়ার্ক ডেটা স্ট্রিমিং, বা ফাইল রিডিং।
```
dart
    Stream<int> countStream(int max) async* {
      for (int i = 1; i <= max; i++) {
        await Future.delayed(Duration(seconds: 1));
        yield i; // মান নির্গত করা
      }
    }

    void main() {
      countStream(5).listen((number) {
        print("Count: $number");
      }, onDone: () {
        print("Stream finished");
      }, onError: (error) {
        print("Stream error: $error");
      });
      print("Starting stream...");
    }
    
```
**মূল পার্থক্য:**

| বৈশিষ্ট্য      | `Future`                                   | `Stream`                                   |
| :----------- | :----------------------------------------- | :----------------------------------------- |
| ডেটার পরিমাণ | একটি মাত্র মান (বা ত্রুটি) নির্গত করে        | সময়ের সাথে সাথে একাধিক মান (বা ত্রুটি) নির্গত করে |
| সম্পন্ন হওয়া   | একবার পূর্ণ হলে সম্পন্ন হয়                  | ডেটা নির্গত করা বন্ধ না হওয়া পর্যন্ত চলতে থাকে |
| ব্যবহারের ক্ষেত্র | একক asynchronous অপারেশন (যেমন HTTP অনুরোধ) | ইভেন্ট হ্যান্ডলিং, ডেটা স্ট্রিমিং (যেমন ফাইল পড়া, WebSocket) |
| শ্রোতা        | `.then()` ব্যবহার করে একটি শ্রোতা যুক্ত করা হয় | `.listen()` ব্যবহার করে একাধিক শ্রোতা যুক্ত করা যায় |

সংক্ষেপে, `Future` একটি single asynchronous মান এর জন্য, আর `Stream` সময়ের সাথে সাথে একাধিক asynchronous মান এর জন্য ব্যবহৃত হয়।

## প্রশ্ন ১৮: Dart-এ `async` এবং `await` কী এবং কীভাবে তারা asynchronous কোড সহজ করে তোলে?

**উত্তর:**

Dart-এ `async` এবং `await` কীওয়ার্ডগুলি asynchronous কোড লেখার জন্য একটি সহজ এবং আরও পঠনযোগ্য সিনট্যাক্স সরবরাহ করে। এগুলি callback-এর nesting ("callback hell") এড়াতে এবং asynchronous কোডকে synchronous কোডের মতো দেখতে ও আচরণ করতে সাহায্য করে।

*   **`async`:** এই কীওয়ার্ডটি একটি ফাংশনের আগে ব্যবহার করা হয় যাতে কম্পাইলার বোঝে যে এই ফাংশনের মধ্যে asynchronous অপারেশন থাকতে পারে। একটি `async` ফাংশন সবসময় একটি `Future` রিটার্ন করে (যদি না আপনি explicitly একটি `Future` রিটার্ন করেন, তাহলে সেই `Future`-টি ব্যবহার করা হবে)।
*   **`await`:** এই কীওয়ার্ডটি শুধুমাত্র `async` ফাংশনের ভিতরে ব্যবহার করা যেতে পারে। এটি একটি `Future` এর সামনে ব্যবহার করা হয় এবং কম্পাইলারকে বলে যে এখানে অপেক্ষা করতে হবে যতক্ষণ না `Future` টি সম্পন্ন হয়। যখন `await` একটি `Future` এর সামনে আসে, ফাংশনের কার্যকরন সাময়িকভাবে স্থগিত হয়ে যায় এবং অন্য কাজ চালানোর জন্য থ্রেড খালি হয়। যখন `Future` সম্পন্ন হয়, কার্যকরন আবার শুরু হয়।

**কীভাবে asynchronous কোড সহজ করে তোলে:**

`async` এবং `await` ব্যবহার করার আগে, asynchronous কোড লেখার জন্য callbacks বা `.then()` chains ব্যবহার করতে হতো। এটি কোডকে পড়তে এবং বুঝতে কঠিন করে তুলত, বিশেষ করে যখন একাধিক asynchronous অপারেশনের উপর নির্ভরতা থাকত।

**উদাহরণ (Callback vs. async/await):**

**Callback উদাহরণ:**

```
dart
void fetchUserData(String userId, void Function(String) callback) {
  Future.delayed(Duration(seconds: 2), () => "User data for $userId").then((data) {
    callback(data);
  });
}

void processUserData(String data, void Function(String) callback) {
  Future.delayed(Duration(seconds: 1), () => data.toUpperCase()).then((processedData) {
    callback(processedData);
  });
}

void main() {
  fetchUserData("123", (userData) {
    processUserData(userData, (processedData) {
      print(processedData);
    });
  });
  print("Fetching and processing data...");
}
```
**async/await উদাহরণ:**
```
dart
Future<String> fetchUserData(String userId) async {
  await Future.delayed(Duration(seconds: 2));
  return "User data for $userId";
}

Future<String> processUserData(String data) async {
  await Future.delayed(Duration(seconds: 1));
  return data.toUpperCase();
}

void main() async {
  print("Fetching and processing data...");
  try {
    String userData = await fetchUserData("123");
    String processedData = await processUserData(userData);
    print(processedData); // 3 সেকেন্ড পর প্রিন্ট হবে
  } catch (e) {
    print("Error: $e");
  }
}
```
`async` এবং `await` উদাহরণটি synchronous কোডের মতো আরও পঠনযোগ্য। `await` ব্যবহার করে আপনি সরাসরি `Future` এর ফলাফল পেতে পারেন এবং ত্রুটি হ্যান্ডলিং এর জন্য synchronous `try-catch` ব্লক ব্যবহার করতে পারেন। এটি কোডকে সহজ, পঠনযোগ্য এবং রক্ষণাবেক্ষণযোগ্য করে তোলে।

## প্রশ্ন ১৯: Dart-এ Generics কী এবং কেন এটি ব্যবহার করা হয়?

**উত্তর:**

Dart-এ Generics হলো একটি বৈশিষ্ট্য যা আপনাকে এমন কোড লিখতে সাহায্য করে যা টাইপ-সেফ কিন্তু বিভিন্ন ডেটা টাইপের সাথে কাজ করতে পারে। এটি টাইপ প্যারামিটার ব্যবহার করে একটি ক্লাস, মেথড, বা ইন্টারফেস তৈরি করার অনুমতি দেয় যা পরে ব্যবহারের সময় নির্দিষ্ট টাইপ দ্বারা প্রতিস্থাপিত হয়।

Generics ব্যবহার করার প্রধান কারণগুলি হল:

*   **টাইপ সেফটি:** এটি কম্পাইল-টাইমে টাইপ সম্পর্কিত ত্রুটিগুলি ধরতে সাহায্য করে, যা রানটাইমে unexpected behaviour বা ক্র্যাশ এড়াতে সহায়ক।
*   **কোড পুনরায় ব্যবহারযোগ্যতা:** আপনি একটি একক জেনেরিক বাস্তবায়ন লিখতে পারেন যা বিভিন্ন ডেটা টাইপের জন্য কাজ করে, প্রতিটি টাইপের জন্য আলাদা আলাদা কোড লেখার প্রয়োজনীয়তা দূর করে।
*   **পারফরম্যান্স:** JIT (Just-In-Time) এবং AOT (Ahead-Of-Time) কম্পাইলেশন উভয় ক্ষেত্রেই Generics পারফরম্যান্স উন্নত করতে সাহায্য করতে পারে।

**Generics ব্যবহারের উদাহরণ:**

আপনি যখন একটি `List` তৈরি করেন, আপনি তার উপাদানের টাইপ specify করতে পারেন:
```
dart
List<int> numbers = [1, 2, 3]; // এই লিস্টে শুধুমাত্র int থাকতে পারে
// numbers.add("hello"); // Error: The argument type 'String' can't be assigned to the parameter type 'int'.

List<String> names = ["Alice", "Bob"]; // এই লিস্টে শুধুমাত্র String থাকতে পারে
```
**কাস্টম জেনেরিক ক্লাস তৈরি:**

আপনি আপনার নিজের ক্লাসেও Generics ব্যবহার করতে পারেন:
```
dart
class Box<T> {
  T value;

  Box(this.value);

  T getValue() {
    return value;
  }
}

void main() {
  var intBox = Box<int>(123);
  print(intBox.getValue()); // আউটপুট: 123
  // var stringValue = intBox.getValue() as String; // Runtime error

  var stringBox = Box<String>("hello");
  print(stringBox.getValue()); // আউটপুট: hello
}
```
এই উদাহরণে, `Box<T>` ক্লাসটি একটি জেনেরিক ক্লাস যেখানে `T` হলো একটি টাইপ প্যারামিটার। আপনি যখন `Box` এর ইনস্ট্যান্স তৈরি করেন, আপনি `<int>` বা `<String>` এর মতো নির্দিষ্ট টাইপ Specify করেন। এটি নিশ্চিত করে যে `intBox` শুধুমাত্র int মান ধরে রাখতে পারে এবং `stringBox` শুধুমাত্র String মান ধরে রাখতে পারে, টাইপ সেফটি নিশ্চিত করে।

Generics Dart-কে আরও শক্তিশালী এবং নিরাপদ করে তোলে, বিশেষ করে যখন ডেটা স্ট্রাকচার এবং অ্যালগরিদম নিয়ে কাজ করা হয়।

## প্রশ্ন ২০: Dart-এ Typedef কী এবং কখন এটি ব্যবহার করবেন?

**উত্তর:**

Dart-এ `typedef` হলো একটি কীওয়ার্ড যা ফাংশন টাইপের জন্য একটি নতুন নাম বা alias তৈরি করতে ব্যবহৃত হয়। এটি কোডকে আরও পঠনযোগ্য এবং বজায় রাখা সহজ করে তোলে, বিশেষ করে যখন ফাংশন টাইপগুলি জটিল বা দীর্ঘ হয়। `typedef` শুধুমাত্র ফাংশন টাইপের জন্য ব্যবহৃত হয়, অন্য কোনো ডেটা টাইপের জন্য নয়।

**Typedef ব্যবহারের কারণ:**

*   **কোড পঠনযোগ্যতা:** জটিল ফাংশন সিগনেচারের জন্য একটি সহজ এবং অর্থপূর্ণ নাম প্রদান করে কোডকে আরও সহজবোধ্য করে তোলে।
*   **পুনরায় ব্যবহারযোগ্যতা:** একই ফাংশন টাইপ একাধিক জায়গায় ব্যবহার করার সময় `typedef` ব্যবহার করে কোড ডুপ্লিকেশন এড়ানো যায়।
*   **Callback ব্যবস্থাপনা:** বিশেষ করে ইভেন্ট হ্যান্ডলার বা callbacks সংজ্ঞায়িত করার সময় `typedef` খুবই উপযোগী।

**Typedef তৈরি করা:**

আপনি `typedef` কীওয়ার্ড ব্যবহার করে একটি alias তৈরি করেন, তারপরে নতুন নাম এবং এটি যে ফাংশন টাইপের জন্য alias তা উল্লেখ করেন।

**উদাহরণ:**
```
dart
// Typedef ছাড়া
void registerCallback(void Function(String message) callback) {
  // callback ব্যবহার করুন
}

// Typedef সহ
typedef MessageCallback = void Function(String message);

void registerCallbackWithTypedef(MessageCallback callback) {
  // callback ব্যবহার করুন
}

void main() {
  MessageCallback myCallback = (msg) {
    print("Received message: $msg");
  };

  registerCallbackWithTypedef(myCallback);

  // আপনি Typedef ছাড়াও ফাংশন পাস করতে পারেন
  registerCallbackWithTypedef((msg) {
    print("Another message: $msg");
  });
}
```
এই উদাহরণে, আমরা `MessageCallback` নামে একটি `typedef` তৈরি করেছি যা `void Function(String message)` ফাংশন টাইপের জন্য alias। `registerCallbackWithTypedef` ফাংশনটি এখন `MessageCallback` টাইপের একটি প্যারামিটার গ্রহণ করে, যা কোডকে আরও স্পষ্ট করে তোলে।

`typedef` কোডের readability এবং maintainability উন্নত করে, বিশেষ করে যখন আপনি ফাংশন টাইপগুলি নিয়ে ব্যাপকভাবে কাজ করেন।

## প্রশ্ন ২১: Dart-এ Callable Classes কী এবং কখন এটি ব্যবহার করবেন?

**উত্তর:**

Dart-এ একটি Callable Class হলো এমন একটি ক্লাস যা একটি ফাংশনের মতো কল করা যেতে পারে। এটি ক্লাসটিকে একটি ফাংশন অবজেক্টে পরিণত করে। আপনি একটি ক্লাসে একটি `call()` মেথড ইম্প্লিমেন্ট করে এটিকে callable করতে পারেন।

**Callable Classes ব্যবহারের কারণ:**

*   **অবস্থা (State) সহ ফাংশন:** যখন আপনার একটি ফাংশনের প্রয়োজন হয় যা কিছু অবস্থা বজায় রাখে। callable class ব্যবহার করে, আপনি ইনস্ট্যান্স ভেরিয়েবলগুলিতে অবস্থা store করতে পারেন।
*   **জটিল ফাংশনালিটি অ্যাবস্ট্রাক্ট করা:** যখন আপনার একটি ফাংশনের প্রয়োজন হয় যা কিছু জটিল সেটআপ বা ক্লিনআপ লজিক প্রয়োজন করে, আপনি এটিকে একটি callable class-এর মধ্যে enclose করতে পারেন।
*   **Decorator প্যাটার্ন:** callable classes ব্যবহার করে আপনি বিদ্যমান ফাংশনালিটি decorate করতে পারেন, যেমন লগিং বা পারফরম্যান্স ট্র্যাকিং যোগ করা।

**Callable Class তৈরি করা:**

আপনাকে ক্লাসের মধ্যে একটি `call()` মেথড সংজ্ঞায়িত করতে হবে। এই মেথডের রিটার্ন টাইপ এবং প্যারামিটারগুলি কল করার সময় প্রত্যাশিত ফাংশন সিগনেচারের সাথে মেলে।

**উদাহরণ:**
```
dart
class Multiplier {
  final int factor;

  Multiplier(this.factor);

  int call(int x) {
    return x * factor;
  }
}

void main() {
  var multiplyByTwo = Multiplier(2);
  print(multiplyByTwo(5)); // আউটপুট: 10 (ফাংশনের মতো কল করা হয়েছে)

  var multiplyByTen = Multiplier(10);
  print(multiplyByTen(5)); // আউটপুট: 50
}
```
এই উদাহরণে, `Multiplier` ক্লাস একটি `call()` মেথড ইম্প্লিমেন্ট করে এটিকে callable করে তোলে। `Multiplier(2)` ইনস্ট্যান্সটি এখন একটি ফাংশনের মতো আচরণ করে যা একটি ইনপুট গ্রহণ করে এবং এটিকে 2 দ্বারা গুণ করে রিটার্ন করে।

Callable classes বিশেষ করে দরকারী যখন আপনি এমন একটি অবজেক্ট চান যা একটি ফাংশনের মতো আচরণ করে তবে কিছু অভ্যন্তরীণ অবস্থা বজায় রাখতে পারে।

## প্রশ্ন ২২: Dart-এ Operators কী এবং কীভাবে আপনি Custom Operators Overload করবেন?

**উত্তর:**

Dart-এ Operators হলো বিশেষ সিম্বল বা কীওয়ার্ড যা এক বা একাধিক অপারেন্ডের উপর অপারেশন করে। যেমন `+`, `-`, `*`, `/`, `=`, `==`, `<`, `>`, `!` ইত্যাদি। Dart বিল্ট-ইন অপারেটরদের জন্য সংজ্ঞা প্রদান করে।

Operator overloading হলো একটি প্রোগ্রামিং ভাষার বৈশিষ্ট্য যা ব্যবহারকারীকে তাদের কাস্টম ক্লাসগুলির জন্য অপারেটরগুলির অর্থ বা আচরণ পরিবর্তন করার অনুমতি দেয়। এর মানে হলো আপনি আপনার নিজের ক্লাসের object গুলোর জন্য arithmetic (`+`, `-`, `*`, `/`), comparison (`==`, `<`, `>`), equality (`==`, `!=`) এবং অন্যান্য অপারেটরদের আচরণ নির্ধারণ করতে পারেন।

**Custom Operator Overloading:**

Dart-এ, আপনি আপনার ক্লাসের জন্য কিছু নির্দিষ্ট অপারেটর overload করতে পারেন। এটি করতে আপনাকে ক্লাসের মধ্যে একটি বিশেষ নামের মেথড সংজ্ঞায়িত করতে হবে। মেথডের নাম হলো `operator` কীওয়ার্ডের পরে যে অপারেটরটি overload করতে চান সেটি।

**Overloadable Operators:**

নিম্নলিখিত অপারেটরগুলি Dart-এ overload করা যেতে পারে:

*   Arithmetic: `+`, `-`, `*`, `/`, `~/`, `%`
*   Equality and Relational: `==`, `<`, `>`, `<=`, `>=`
*   Bitwise: `&`, `|`, `^`, `<<`, `>>`
*   Unary: `-`, `~`
*   Index: `[]`, `[]=`

**উদাহরণ:**

```
dart
class Point {
  final int x;
  final int y;

  const Point(this.x, this.y);

  // + অপারেটর ওভারলোড করা
  Point operator +(Point other) {
    return Point(x + other.x, y + other.y);
  }

  // == অপারেটর ওভারলোড করা
  @override
  bool operator ==(Object other) =>
      identical(this, other) ||
      other is Point &&
          runtimeType == other.runtimeType &&
          x == other.x &&
          y == other.y;

  // hashCode অপারেটর == এর সাথে সামঞ্জস্যপূর্ণ হতে হবে
  @override
  int get hashCode => x.hashCode ^ y.hashCode;
}

void main() {
  var p1 = const Point(1, 2);
  var p2 = const Point(3, 4);
  var p3 = p1 + p2; // + অপারেটর ব্যবহার করা হয়েছে
  print('Point 3: (${p3.x}, ${p3.y})'); // আউটপুট: Point 3: (4, 6)

  var p4 = const Point(1, 2);
  print(p1 == p4); // == অপারেটর ব্যবহার করা হয়েছে
  print(p1 == p2);
}
```
এই উদাহরণে, `Point` ক্লাসের জন্য `+` এবং `==` অপারেটর overload করা হয়েছে। `+` অপারেটর দুটি `Point` অবজেক্টের `x` এবং `y` মান যোগ করে একটি নতুন `Point` অবজেক্ট রিটার্ন করে। `==` অপারেটর দুটি `Point` অবজেক্ট সমান কিনা তা তাদের `x` এবং `y` মান তুলনা করে নির্ধারণ করে। মনে রাখবেন, যখন আপনি `==` অপারেটর overload করেন, তখন আপনাকে `hashCode` প্রোপার্টিও override করতে হবে যাতে তারা সামঞ্জস্যপূর্ণ থাকে।

Operator overloading আপনার কাস্টম ক্লাসগুলির সাথে কাজ করা আরও স্বজ্ঞাত এবং প্রাকৃতিক করে তুলতে পারে, বিশেষ করে যখন গাণিতিক বা যৌক্তিক অপারেশনগুলি সাধারণ হয়।

## প্রশ্ন ২৩: Dart-এ Metaprogramming কী এবং এর উদাহরণ দাও।

**উত্তর:**

Metaprogramming হলো একটি প্রোগ্রামিং টেকনিক যেখানে একটি প্রোগ্রাম অন্য প্রোগ্রামকে ডেটা হিসেবে ট্রিট করে। Dart-এর প্রেক্ষাপটে, এটি প্রোগ্রাম রান হওয়ার সময় তার নিজের স্ট্রাকচার পরিদর্শন (inspect) এবং পরিবর্তন করার ক্ষমতাকে বোঝায়। Dart-এ Metaprogramming সাধারণত Reflection এর মাধ্যমে অর্জন করা হয়।

**Reflection:** Reflection হলো রানটাইমে একটি প্রোগ্রামের স্ট্রাকচার পরিদর্শন এবং পরিবর্তন করার ক্ষমতা। Dart-এর `dart:mirrors` লাইব্রেরি reflection কার্যকারিতা প্রদান করে। এটি ব্যবহার করে আপনি ক্লাস, মেথড, ফিল্ড এবং লাইব্রেরি সম্পর্কে তথ্য পেতে পারেন এবং রানটাইমে সেগুলিকে কল বা modify করতে পারেন।

**Metaprogramming ব্যবহারের কারণ (সাধারণত লাইব্রেরি ডেভেলপমেন্টে):**

*   **সিরিয়ালাইজেশন/ডিসিডিকরণ:** JSON বা অন্যান্য ফরম্যাট থেকে অবজেক্ট serialize এবং deserialize করার জন্য ক্লাসের স্ট্রাকচার পরিদর্শন করা।
*   **ORM (Object-Relational Mapping):** ডাটাবেস টেবিলের সাথে ক্লাস mapping করার জন্য ক্লাসের মেম্বার সম্পর্কে তথ্য ব্যবহার করা।
*   **টেস্টিং ফ্রেমওয়ার্ক:** টেস্ট runner তৈরি করা যা dynamically টেস্ট ক্লাস এবং মেথড খুঁজে বের করতে পারে।
*   **ডায়নামিক কোড জেনারেশন:** রানটাইমে কোড generate এবং execute করা (যদিও Dart-এ এটি অন্যান্য ভাষার মতো সাধারণ নয়)।

**উদাহরণ (ধারণাগত - `dart:mirrors` ওয়েব বা Flutter-এ উপলব্ধ নয়):**

মনে রাখবেন `dart:mirrors` লাইব্রেরি সাধারণত সার্ভার-সাইড Dart বা কমান্ড-লাইন অ্যাপ্লিকেশনে ব্যবহৃত হয়। ওয়েব বা Flutter-এ AOT কম্পাইলেশনের কারণে Reflection সীমিত বা উপলব্ধ নয়।

```
dart
// শুধুমাত্র কমান্ড-লাইন অ্যাপ্লিকেশনে কাজ করবে
import 'dart:mirrors';

class MyClass {
  String name;
  int age;

  MyClass(this.name, this.age);

  void greet() {
    print("Hello, my name is $name and I am $age years old.");
  }
}

void main() {
  var instance = MyClass("Alice", 30);
  InstanceMirror instanceMirror = reflect(instance);
  ClassMirror classMirror = instanceMirror.type;

  // ক্লাসের নাম পাওয়া
  print("Class name: ${MirrorSystem.getName(classMirror.simpleName)}");

  // মেম্বারদের তালিকা পাওয়া
  classMirror.declarations.forEach((symbol, declarationMirror) {
    print("Member: ${MirrorSystem.getName(symbol)}");
  });

  // মেথড কল করা
  instanceMirror.invoke(
      const Symbol('greet'), []); // 'greet' মেথড কল করা হয়েছে
}
```
এই উদাহরণটি `dart:mirrors` ব্যবহার করে একটি ক্লাসের ইনস্ট্যান্স পরিদর্শন করে এবং তার মেথড কল করে।

Flutter বা ওয়েবে, Compile-time metaprogramming (যেমন build_runner এবং কোড জেনারেশন) Reflection এর বিকল্প হিসাবে ব্যবহৃত হয়। এটি পারফরম্যান্স উন্নত করে এবং AOT কম্পাইলেশন সম্ভব করে তোলে।

## প্রশ্ন ২৪: Dart-এ Const Constructors কী?

**উত্তর:**

Dart-এ Const Constructors হলো বিশেষ ধরণের কনস্ট্রাক্টর যা compile-time constant অবজেক্ট তৈরি করতে ব্যবহৃত হয়। একটি ক্লাসকে compile-time constant object তৈরি করার অনুমতি দিতে হলে তার অন্তত একটি `const` কনস্ট্রাক্টর থাকতে হবে।

**Const Constructor এর প্রয়োজনীয়তা:**

*   ক্লাসের সমস্ত ফিল্ড `final` হতে হবে।
*   কনস্ট্রাক্টর নিজেই `const` কীওয়ার্ড দিয়ে ঘোষণা করতে হবে।
*   Const constructor এর বডিতে কোনো side effect থাকতে পারে না।
*   যদি ক্লাসে কোনো final ফিল্ডের initializer থাকে, তাহলে সেই initializer অবশ্যই compile-time constant হতে হবে।

**Const Constructor ব্যবহারের কারণ:**

*   **পারফরম্যান্স:** Const object গুলি কম্পাইল-টাইমে তৈরি এবং অপটিমাইজ করা হয়। একই const object একাধিকবার ব্যবহার করলে, Dart শুধুমাত্র একবার মেমরি allocate করে। এটি মেমরি ব্যবহার কমায় এবং পারফরম্যান্স উন্নত করে।
*   **তুলনা (Equality):** দুটি identical const object `==` অপারেটর ব্যবহার করে তুলনা করলে `true` রিটার্ন করে কারণ তারা মেমরিতে একই ইনস্ট্যান্স share করে (যদিও এটি সবসময় গ্যারান্টিযুক্ত নয়, তবে এটি সাধারণ)।
*   **Widget Tree অপটিমাইজেশন (Flutter-এ):** Flutter-এ `const` উইজেটগুলি খুব গুরুত্বপূর্ণ। যখন একটি উইজেট `const` হয়, Flutter জানে যে এটি পরিবর্তন হবে না এবং বারবার রিbuild করার প্রয়োজন নেই, যা পারফরম্যান্স উল্লেখযোগ্যভাবে উন্নত করে।

**উদাহরণ:**