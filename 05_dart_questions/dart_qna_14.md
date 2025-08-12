## Dart Interview Questions and Answers (91-100)

### Question 91: Dart-এ `covariant` কীওয়ার্ডের ভূমিকা কী?
**উত্তর:** Dart-এ `covariant` কীওয়ার্ডটি মেথড প্যারামিটারগুলিতে ব্যবহৃত হয় যাতে সাবটাইপগুলি সুপারটাইপের প্যারামিটার টাইপের পরিবর্তে ব্যবহৃত হতে পারে। এটি টাইপ সেফটি বজায় রেখে সাবটাইপগুলিতে মেথড ওভাররাইড করার সময় ফ্লেক্সিবিলিটি প্রদান করে।

উদাহরণ:
```
dart
class Animal {
  void chase(Animal x) {
    print('Animal chasing ${x.runtimeType}');
  }
}

class Cat extends Animal {
  @override
  void chase(covariant Animal x) { // covariant keyword
    print('Cat chasing ${x.runtimeType}');
  }
}

void main() {
  Animal animal = Cat();
  Animal mouse = Animal();
  animal.chase(mouse); // This is allowed due to covariant
}
```
### Question 92: Dart-এর `typedef` কী এবং কেন এটি ব্যবহার করবেন?
**উত্তর:** `typedef` হলো Dart-এ ফাংশন টাইপের জন্য একটি অ্যালিয়াস তৈরি করার উপায়। এটি জটিল ফাংশন টাইপগুলিকে সহজ নাম দিয়ে প্রতিস্থাপন করতে সাহায্য করে, কোডকে আরও পঠনযোগ্য এবং রক্ষণাবেক্ষণযোগ্য করে তোলে।

উদাহরণ:
```
dart
typedef IntList = List<int>; // typedef for List<int>

void processIntList(IntList list) {
  // ...
}

void main() {
  IntList numbers = [1, 2, 3];
  processIntList(numbers);
}
```
### Question 93: Dart-এ অ্যাসিঙ্ক্রোনাস প্রোগ্রামিং-এ `Future.wait` এর ব্যবহার কী?
**উত্তর:** `Future.wait` হল একটি ফাংশন যা একলিস্ট `Future` গ্রহণ করে এবং একটি নতুন `Future` প্রদান করে যা সমস্ত ইনপুট `Future` সম্পন্ন হলে সম্পন্ন হয়। এটি একাধিক অ্যাসিঙ্ক্রোনাস অপারেশন সমান্তরালভাবে চালানোর জন্য এবং সেগুলির সমস্ত ফলাফল একসাথে অপেক্ষা করার জন্য ব্যবহৃত হয়।

উদাহরণ:
```
dart
Future<String> fetchUserData() async {
  await Future.delayed(Duration(seconds: 2));
  return 'User Data';
}

Future<String> fetchOrderData() async {
  await Future.delayed(Duration(seconds: 3));
  return 'Order Data';
}

void main() async {
  try {
    List<String> results = await Future.wait([
      fetchUserData(),
      fetchOrderData(),
    ]);
    print('User Data: ${results[0]}');
    print('Order Data: ${results[1]}');
  } catch (e) {
    print('Error fetching data: $e');
  }
}
```
### Question 94: Dart-এ `yield` কীওয়ার্ডটি Stream-এর সাথে কীভাবে কাজ করে?
**উত্তর:** `yield` কীওয়ার্ডটি `async*` জেনারেটর ফাংশনগুলিতে ব্যবহৃত হয় Stream-এ ডেটা পুশ করার জন্য। যখন `yield` স্টেটমেন্ট কার্যকর হয়, তখন ফাংশনটি সাময়িকভাবে স্থগিত হয় এবং উত্পাদিত মান Stream-এ সরবরাহ করা হয়। পরবর্তীবার যখন Stream থেকে ডেটার অনুরোধ করা হয়, তখন ফাংশনটি আবার চালু হয়।

উদাহরণ:
```
dart
Stream<int> countUpTo(int max) async* {
  for (int i = 1; i <= max; i++) {
    await Future.delayed(Duration(seconds: 1));
    yield i; // Yield values to the stream
  }
}

void main() async {
  await for (int number in countUpTo(5)) {
    print(number);
  }
}
```
### Question 95: Dart-এর `extension methods` কী এবং এর সুবিধা কী?
**উত্তর:** Extension methods হলো বিদ্যমান ক্লাসগুলিতে নতুন ফাংশনালিটি যোগ করার একটি উপায়, সেই ক্লাসগুলির সোর্স কোড অ্যাক্সেস না করেই। এটি কোড রিইউজেবিলিটি বাড়াতে এবং ডেটা টাইপগুলির সাথে সম্পর্কিত সহায়ক ফাংশন সরবরাহ করতে সহায়ক।

উদাহরণ:
```
dart
extension StringExtensions on String {
  String capitalize() {
    if (isEmpty) return this;
    return this[0].toUpperCase() + substring(1);
  }
}

void main() {
  String name = "hello";
  print(name.capitalize()); // Output: Hello
}
```
### Question 96: Dart-এ `late` কীওয়ার্ডের ব্যবহার ব্যাখ্যা করো।
**উত্তর:** `late` কীওয়ার্ডটি এমন একটি ভ্যারিয়েবল ঘোষণা করতে ব্যবহৃত হয় যা প্রথমে ঘোষিত হওয়ার সময় ইনিশিয়ালাইজড হয় না, তবে এটি প্রথমবার অ্যাক্সেস করার আগে ইনিশিয়ালাইজড হবে বলে আশা করা হয়। এটি নন-নাল্লেবল ভ্যারিয়েবলগুলিকে ইনিশিয়ালাইজ করার প্রয়োজন মেটাতে ব্যবহৃত হয় যখন ইনিশিয়ালাইজেশন কনস্ট্রাক্টরের বাইরে বা শর্তসাপেক্ষভাবে হয়।

উদাহরণ:
```
dart
late String name; // Declared as late

void main() {
  name = "Alice"; // Initialized later
  print(name);
}
```
### Question 97: Dart-এর `factory constructor` কী এবং কখন এটি ব্যবহার করবে?
**উত্তর:** একটি factory constructor ক্লাস ইনস্ট্যান্স তৈরি করার জন্য ব্যবহৃত হয় যা প্রতিবার কল করার সময় নতুন ইনস্ট্যান্স তৈরি নাও করতে পারে। এটি ক্যাশিং, সাবটাইপ রিটার্ন করা বা বিদ্যমান ইনস্ট্যান্স রিটার্ন করার মতো পরিস্থিতিতে ব্যবহৃত হয়। Factory constructor এর আগে `factory` কীওয়ার্ডটি থাকতে হবে এবং এটি একটি ইনস্ট্যান্স রিটার্ন করে।

উদাহরণ:
```
dart
class Logger {
  final String name;
  static final Map<String, Logger> _cache = {};

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
  print(identical(logger1, logger2)); // Output: true
}
```
### Question 98: Dart-এ `callable classes` কী?
**উত্তর:** Dart-এ একটি ক্লাসকে callable class বলা হয় যদি এটি একটি `call()` মেথড ইমপ্লিমেন্ট করে। এটি ক্লাসের ইনস্ট্যান্সগুলিকে ফাংশন হিসাবে কল করার অনুমতি দেয়।

উদাহরণ:
```
dart
class MultiplyBy {
  final int factor;

  MultiplyBy(this.factor);

  int call(int x) {
    return factor * x;
  }
}

void main() {
  var multiplyByTwo = MultiplyBy(2);
  print(multiplyByTwo(5)); // Output: 10
}
```
### Question 99: Dart-এর `isolates` কীভাবে কাজ করে এবং কেন এটি ব্যবহৃত হয়?
**উত্তর:** Isolates হলো Dart-এ কনকারেন্সি অর্জনের একটি উপায়। প্রতিটি isolate এর নিজস্ব মেমরি হিপ এবং ইভেন্ট লুপ থাকে, যা সেগুলিকে সম্পূর্ণ স্বাধীন করে তোলে। এটি দীর্ঘ-চলমান বা CPU-ইনটেনসিভ কাজগুলিকে প্রধান UI থ্রেড ব্লক না করে চালানোর অনুমতি দেয়, যার ফলে অ্যাপ্লিকেশনটি রেসপন্সিভ থাকে। Isolates মেসেজ পাসিংয়ের মাধ্যমে যোগাযোগ করে।

### Question 100: Dart-এর `FFI (Foreign Function Interface)` কী এবং এর ব্যবহার কী?
**উত্তর:** FFI (Foreign Function Interface) হলো Dart-এর একটি মেকানিজম যা Dart কোডকে নেটিভ কোড (যেমন C লাইব্রেরী) এর সাথে ইন্টারঅ্যাক্ট করার অনুমতি দেয়। এটি Dart অ্যাপ্লিকেশন থেকে সরাসরি নেটিভ লাইব্রেরীগুলির ফাংশন কল করতে এবং তাদের ডেটা স্ট্রাকচার অ্যাক্সেস করতে ব্যবহৃত হয়। এটি পারফরম্যান্স-ক্রিটিকাল কাজ, বিদ্যমান নেটিভ লাইব্রেরী ব্যবহার বা প্ল্যাটফর্ম-স্পেসিফিক API অ্যাক্সেস করার জন্য কার্যকর।