# Dart Language - প্রশ্নোত্তর সেট ৩

## Advanced Dart Concepts

### প্রশ্ন ১১: Mixins এবং Extensions কীভাবে ব্যবহার করবেন?

**উত্তর (ডিটেইল):**

- **Mixins** হলো multiple classes-এ code share করার জন্য ব্যবহৃত mechanism। **Extensions** হলো existing classes-এ new functionality add করার জন্য ব্যবহৃত feature।

**Mixins vs Extensions:**
- Mixins: Code reuse across multiple classes
- Extensions: Add methods to existing classes

**উদাহরণ:**

```dart
// Mixin example
mixin Flying {
  void fly() => print('Flying high!');
}

mixin Swimming {
  void swim() => print('Swimming deep!');
}

class Duck with Flying, Swimming {
  final String name;
  Duck(this.name);
  
  void introduce() => print('I am $name');
}

// Extension example
extension StringExtension on String {
  String get reversed => split('').reversed.join();
  bool get isPalindrome => this == reversed;
  
  String capitalize() {
    if (isEmpty) return this;
    return '${this[0].toUpperCase()}${substring(1)}';
  }
}

// Usage
void mixinExtensionDemo() {
  final duck = Duck('Donald');
  duck.introduce();
  duck.fly();
  duck.swim();
  
  final text = 'hello';
  print(text.capitalize()); // Hello
  print(text.reversed); // olleh
  print('racecar'.isPalindrome); // true
}
```

**Interview Tips:**
- Mixins multiple inheritance simulate করে
- Extensions existing classes modify করে না
- with keyword mixins use করে
- Extensions utility methods add করার জন্য

---

### প্রশ্ন ১২: Isolates কী এবং এটা কীভাবে ব্যবহার করবেন?

**উত্তর (ডিটেইল):**

- **Isolates** হলো Dart-এ concurrent programming-এর জন্য ব্যবহৃত mechanism। এটা separate memory spaces-এ code run করে।

**Isolate Features:**
- Separate memory space
- No shared state
- Communication via messages
- Heavy computations-এর জন্য

**উদাহরণ:**

```dart
import 'dart:isolate';

// Heavy computation function
int heavyComputation(int n) {
  int result = 0;
  for (int i = 0; i < n; i++) {
    result += i * i;
  }
  return result;
}

// Isolate usage
Future<int> computeInIsolate(int n) async {
  final receivePort = ReceivePort();
  
  await Isolate.spawn((SendPort sendPort) {
    final result = heavyComputation(n);
    sendPort.send(result);
  }, receivePort.sendPort);
  
  return await receivePort.first as int;
}

// Usage
void isolateDemo() async {
  print('Starting computation...');
  final result = await computeInIsolate(1000000);
  print('Result: $result');
}
```

**Interview Tips:**
- Isolates heavy computations-এর জন্য
- No shared memory between isolates
- Communication via SendPort/ReceivePort
- Main isolate UI thread block করে না

---

### প্রশ্ন ১৩: Dart-এ Memory Management কীভাবে কাজ করে?

**উত্তর (ডিটেইল):**

- **Memory Management** হলো Dart-এ automatic garbage collection দ্বারা handle হয়। Developers manually memory manage করতে হয় না।

**Memory Management Features:**
- Automatic garbage collection
- Reference counting
- Memory leak prevention
- Performance optimization

**উদাহরণ:**

```dart
class MemoryExample {
  List<String> _data = [];
  
  void addData(String item) {
    _data.add(item);
  }
  
  void clearData() {
    _data.clear();
    _data = []; // Old list becomes eligible for GC
  }
  
  void dispose() {
    _data.clear();
    _data = [];
  }
}

// Usage
void memoryDemo() {
  final example = MemoryExample();
  
  // Add data
  for (int i = 0; i < 1000; i++) {
    example.addData('Item $i');
  }
  
  // Clear data
  example.clearData();
  
  // Dispose
  example.dispose();
}
```

**Interview Tips:**
- Garbage collection automatic
- Dispose methods cleanup resources
- Weak references memory leaks prevent করে
- Memory profiling tools use করুন
