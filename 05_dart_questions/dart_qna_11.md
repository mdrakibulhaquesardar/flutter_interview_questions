### প্রশ্ন: Dart এ Factory Constructor কী এবং কেন এটি ব্যবহার করবেন?

**উত্তর:** Factory constructor হলো এক ধরণের constructor যা সবসময় একটি নতুন ইনস্ট্যান্স তৈরি করে না। এটি একটি existing ইনস্ট্যান্স রিটার্ন করতে পারে বা অন্য কোনো উপায়ে ইনস্ট্যান্স তৈরি করতে পারে। Factory constructor `factory` কীওয়ার্ড দিয়ে শুরু হয়।

এটি ব্যবহারের কারণগুলো হলো:

*   আপনি যখন চান constructor প্রতিবার নতুন ইনস্ট্যান্স তৈরি না করে একটি ক্যাশ করা ইনস্ট্যান্স বা সাবটাইপের ইনস্ট্যান্স ফেরত দিক।
*   যখন constructor এর মধ্যে কিছু লজিক প্রয়োজন হয় যা ইনস্ট্যান্স তৈরির আগে ডেটা প্রক্রিয়া করবে।

**উদাহরণ:**

```
dart
class Logger {
  final String name;
  static final Map<String, Logger> _cache = <String, Logger>{};

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
  logger1.log('Button clicked');

  var logger2 = Logger('UI'); // Returns the cached instance
  logger2.log('Another UI event');

  print(identical(logger1, logger2)); // Output: true
}
```
### প্রশ্ন: Dart এ Callable Class কী?

**উত্তর:** Dart এ একটি ক্লাসকে "callable" করা যায় যদি এটি `call()` মেথড ইমপ্লিমেন্ট করে। যখন আপনি এই ক্লাসের একটি ইনস্ট্যান্সকে একটি ফাংশনের মতো ডাকেন, তখন এর `call()` মেথড এক্সিকিউট হয়।

**উদাহরণ:**
```
dart
class WannabeFunction {
  String call(String a, String b, String c) => '$a $b $c!';
}

void main() {
  var wf = WannabeFunction();
  var out = wf('Hi', 'there', 'gang');
  print(out); // Output: Hi there gang!
}
```
### প্রশ্ন: Dart এ Type Aliases কী?

**উত্তর:** Type alias হলো একটি বিদ্যমান টাইপের জন্য অন্য একটি নাম বা ডাকনাম তৈরি করার উপায়। এটি কোডকে আরও পাঠযোগ্য করে তোলে, বিশেষ করে যখন জটিল টাইপ থাকে যেমন ফাংশন টাইপ বা দীর্ঘ জেনেরিক টাইপ। `typedef` কীওয়ার্ড ব্যবহার করে টাইপ alias তৈরি করা হয়।

**উদাহরণ:**

```
dart
typedef WidgetCallback = void Function(String widgetName);

void logWidgetAction(String name, WidgetCallback callback) {
  print('Performing action on $name');
  callback(name);
}

void main() {
  logWidgetAction('Button', (name) {
    print('$name action completed.');
  });
}
```
### প্রশ্ন: Dart এ `rethrow` এবং `throw` এর মধ্যে পার্থক্য কী?

**উত্তর:**

*   **`throw`:** এটি একটি নতুন ব্যতিক্রম তৈরি করে এবং সেটিকে থ্রো করে। এটি সাধারণত নতুন ত্রুটির জন্য ব্যবহৃত হয়।
*   **`rethrow`:** এটি একটি ইতিমধ্যে ধরা (caught) ব্যতিক্রমকে পুনরায় থ্রো করে, আসল ব্যতিক্রমের স্ট্যাক ট্রেস বজায় রাখে। এটি সাধারণত একটি `catch` ব্লকের মধ্যে ব্যবহৃত হয় যখন আপনি ব্যতিক্রমটি ধরতে চান, কিছু কাজ করতে চান (যেমন লগিং), এবং তারপর এটিকে আরও উপরে Propagation করতে চান।

**উদাহরণ:**
```
dart
void mightThrow() {
  throw Exception("Something went wrong!");
}

void processError() {
  try {
    mightThrow();
  } catch (e, s) {
    print("Caught error: $e");
    // Some handling logic...
    rethrow; // Rethrow the original exception with its stack trace
  }
}

void main() {
  try {
    processError();
  } catch (e, s) {
    print("Rethrown error caught in main: $e");
    print("Stack trace:\n$s");
  }
}
```
### প্রশ্ন: Dart এ Extension Methods কী?

**উত্তর:** Extension methods আপনাকে কোনো ক্লাসের সোর্স কোড অ্যাক্সেস না করেই সেই ক্লাসে নতুন কার্যকারিতা (methods, getters, setters, operators) যোগ করতে দেয়। এটি বিশেষ করে সেই ক্লাসগুলির জন্য উপযোগী যেগুলি আপনি লাইব্রেরি থেকে পেয়েছেন এবং সরাসরি পরিবর্তন করতে পারবেন না। Extension methods `extension` কীওয়ার্ড ব্যবহার করে তৈরি করা হয়।

**উদাহরণ:**

```
dart
extension StringManipulation on String {
  String capitalize() {
    if (this.isEmpty) {
      return this;
    }
    return this[0].toUpperCase() + this.substring(1);
  }
}

void main() {
  String greeting = "hello world";
  print(greeting.capitalize()); // Output: Hello world
}
```
### প্রশ্ন: Dart এ FFI (Foreign Function Interface) কী এবং কেন এটি ব্যবহৃত হয়?

**উত্তর:** FFI (Foreign Function Interface) হলো একটি ম্যাকানিজম যা Dart কোডকে অন্য প্রোগ্রামিং ভাষা, বিশেষ করে C-based লাইব্রেরিগুলির সাথে সরাসরি ইন্টারঅ্যাক্ট করতে দেয়। এর মাধ্যমে আপনি নেটিভ কোড (C, C++, Rust ইত্যাদি) থেকে ফাংশন কল করতে পারেন এবং তাদের ডেটা টাইপগুলি ব্যবহার করতে পারেন।

**ব্যবহারের কারণ:**

*   বিদ্যমান নেটিভ লাইব্রেরি ব্যবহার করা।
*   পারফরম্যান্স-ক্রিটিক্যাল টাস্কগুলির জন্য নেটিভ কোড ব্যবহার করা।
*   হার্ডওয়্যার অ্যাক্সেস করা যা Dart এর স্ট্যান্ডার্ড লাইব্রেরিতে উপলব্ধ নয়।

### প্রশ্ন: Dart এ Generators কী?

**উত্তর:** Generators হলো এক ধরণের ফাংশন যা অলসভাবে মান তৈরি করে। এটি `yield` কীওয়ার্ড ব্যবহার করে একটি করে মান ফেরত দেয়। Generators দুটি ধরণের হয়:

1.  **Synchronous Generators:** এগুলো একটি `Iterable` ফেরত দেয় এবং `sync*` কীওয়ার্ড ব্যবহার করে। মানগুলি এক এক করে তৈরি হয় যখন অনুরোধ করা হয়।
2.  **Asynchronous Generators:** এগুলো একটি `Stream` ফেরত দেয় এবং `async*` কীওয়ার্ড ব্যবহার করে। মানগুলি asynchronously তৈরি হয়।

**উদাহরণ (Synchronous):**
```
dart
Iterable<int> countUpTo(int max) sync* {
  for (int i = 1; i <= max; i++) {
    yield i;
  }
}

void main() {
  for (var value in countUpTo(5)) {
    print(value);
  }
}
```
**উদাহরণ (Asynchronous):**
```
dart
Stream<int> countUpToAsync(int max) async* {
  for (int i = 1; i <= max; i++) {
    await Future.delayed(Duration(milliseconds: 100));
    yield i;
  }
}

void main() async {
  await for (var value in countUpToAsync(5)) {
    print(value);
  }
}
```
### প্রশ্ন: Dart এ Isolates কী এবং কেন এটি গুরুত্বপূর্ণ?

**উত্তর:** Isolates হলো Dart এর কনকারেন্সি মডেল। একটি isolate হলো মেমরির একটি স্বাধীন স্পেস যেখানে একটি প্রোগ্রাম রান করে। দুটি isolate সরাসরি মেমরি শেয়ার করতে পারে না; তারা পোর্ট ব্যবহার করে মেসেজ পাসিং এর মাধ্যমে যোগাযোগ করে। এটি UI কে ব্লক না করে হেভি বা লং-রানিং টাস্ক সম্পাদন করতে সহায়তা করে।

**গুরুত্বপূর্ণতা:**

*   **Non-blocking UI:** ব্যাকগ্রাউন্ড টাস্কগুলি প্রধান UI থ্রেডকে ব্লক করে না।
*   **Parallel Execution:** মাল্টি-কোর প্রসেসরগুলির সুবিধা নিতে পারে।
*   **Safety:** যেহেতু মেমরি শেয়ার হয় না, ডেটা রেস এবং অন্যান্য কনকারেন্সি সমস্যা এড়ানো যায়।

### প্রশ্ন: Dart এ Mixins কী?

**উত্তর:** Mixins হলো একটি ক্লাস থেকে কোড রিইউজ করার উপায়। একটি mixin হলো একটি সাধারণ ক্লাস যা অন্য ক্লাস দ্বারা ইমপ্লিমেন্ট বা এক্সটেন্ড না করে `with` কীওয়ার্ড ব্যবহার করে ব্যবহার করা হয়। Mixin গুলি প্রোপার্টি এবং মেথড সরবরাহ করতে পারে যা ব্যবহারকারী ক্লাস দ্বারা অ্যাক্সেস করা যেতে পারে।

**উদাহরণ:**
```
dart
mixin Walkable {
  void walk() {
    print("I can walk.");
  }
}

mixin Swimmable {
  void swim() {
    print("I can swim.");
  }
}

class Animal with Walkable, Swimmable {}

void main() {
  var animal = Animal();
  animal.walk();
  animal.swim();
}
```
### প্রশ্ন: Dart এ `covariant` কীওয়ার্ড কী এবং কেন এটি ব্যবহৃত হয়?

**উত্তর:** `covariant` কীওয়ার্ড একটি মেথড প্যারামিটারের টাইপকে আরও নির্দিষ্ট (subtype) হতে অনুমতি দেয় যখন ক্লাসটি ওভাররাইড করা হয়। সাধারণত, একটি মেথড ওভাররাইড করার সময়, প্যারামিটারের টাইপ অবশ্যই বেস ক্লাসের প্যারামিটারের টাইপের সমান বা সুপারটাইপ হতে হবে। `covariant` এই নিয়ম শিথিল করে subtype ব্যবহারের অনুমতি দেয়। এটি পলিমরফিজমের ক্ষেত্রে টাইপ সেফটি বজায় রাখতে সাহায্য করে।

**উদাহরণ:**
