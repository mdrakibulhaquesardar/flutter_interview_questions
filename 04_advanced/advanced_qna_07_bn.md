## প্রশ্ন ৫১: Flutter-এ `Key` এর গুরুত্ব ব্যাখ্যা করুন।

**উত্তর:** Flutter উইজেট ট্রি-তে উইজেটগুলি সনাক্ত করতে এবং তাদের স্টেট বজায় রাখতে `Key` গুলি ব্যবহৃত হয়। যখন উইজেট ট্রি পরিবর্তিত হয়, Flutter উইজেটগুলির তুলনা করতে এবং দক্ষতার সাথে আপডেট করতে `Key` ব্যবহার করে। বিশেষ করে একই ধরনের একাধিক উইজেটের ক্ষেত্রে বা যখন উইজেটগুলির ক্রম পরিবর্তিত হয়, তখন `Key` ব্যবহার করা গুরুত্বপূর্ণ যাতে Flutter সঠিক উইজেটের সাথে সঠিক স্টেট মেলাতে পারে। তিন ধরণের `Key` আছে: `ValueKey`, `ObjectKey`, এবং `GlobalKey`।

```
dart
ListView.builder(
  itemCount: items.length,
  itemBuilder: (context, index) {
    return ListItemWidget(key: ValueKey(items[index].id), item: items[index]);
  },
);
```
## প্রশ্ন ৫২: Flutter-এ `repaintBoundary` কখন ব্যবহার করা উচিত?

**উত্তর:** `RepaintBoundary` একটি উইজেট যা Flutter-কে নির্দেশ দেয় যে একটি নির্দিষ্ট সাবট্রিকে আলাদাভাবে রেন্ডার করতে হবে। এটি পারফরম্যান্স উন্নত করতে সাহায্য করে যখন একটি উইজেটের একটি অংশ প্রায়শই রিফ্রেশ হয় কিন্তু তার চারপাশের উইজেটগুলি হয় না। `RepaintBoundary` ব্যবহার করলে Flutter শুধুমাত্র সেই নির্দিষ্ট অংশটি রিফ্রেশ করবে, পুরো উইজেট ট্রি নয়। অ্যানিমেশন, ভিডিও প্লেয়ার বা ঘন ঘন আপডেট হওয়া গ্রাফিক্সের ক্ষেত্রে এটি খুব কার্যকর।

```
dart
RepaintBoundary(
  child: AnimatedBuilder(
    animation: _controller,
    builder: (context, child) {
      return CustomPaint(
        painter: MyAnimatedPainter(_controller.value),
      );
    },
  ),
);
```
## প্রশ্ন ৫৩: Flutter-এ `addPostFrameCallback` এর কাজ কি?

**উত্তর:** `addPostFrameCallback` একটি ফাংশন যা Flutter এর ফ্রেম রেন্ডারিং সম্পন্ন হওয়ার পরে কল করা হয়। এটি সাধারণত উইজেটগুলি বিল্ড হওয়ার পরে ডেটা পেতে বা UI আপডেট করতে ব্যবহৃত হয়, যেমন স্ক্রিনের আকার বা অবস্থানের উপর নির্ভর করে কিছু করা। এটি `WidgetsBinding.instance!.addPostFrameCallback` এর মাধ্যমে অ্যাক্সেস করা যায়।

```
dart
@override
void initState() {
  super.initState();
  WidgetsBinding.instance!.addPostFrameCallback((_) {
    // Do something after the first frame is rendered
    print('First frame rendered');
  });
}
```
## প্রশ্ন ৫৪: Flutter-এ `blendMode` কীভাবে কাজ করে?

**উত্তর:** `BlendMode` দুটি পিক্সেলের রঙকে মিশ্রিত করার উপায় নির্ধারণ করে। এটি ইমেজ, আইকন বা গ্রাফিক্সের ওভারলে প্রভাব তৈরি করতে ব্যবহৃত হয়। Flutter-এ বিভিন্ন ধরণের `BlendMode` উপলব্ধ রয়েছে, যেমন `multiply`, `screen`, `overlay`, `darken`, `lighten`, ইত্যাদি। এটি `ColorFiltered` উইজেট বা `Canvas` এ ব্যবহার করা যেতে পারে।
```
dart
ColorFiltered(
  colorFilter: ColorFilter.mode(Colors.red, BlendMode.multiply),
  child: Image.asset('assets/image.png'),
);
```
## প্রশ্ন ৫৫: Flutter-এ `compute` ফাংশনের উদ্দেশ্য কী?

**উত্তর:** `compute` Flutter এর `foundation` লাইব্রেরীতে উপলব্ধ একটি ফাংশন যা একটি আলাদা আইসোলেটে একটি লং-রানিং বা কম্পিউটেশনালি ইন্টেন্সিভ টাস্ক চালানোর জন্য ব্যবহৃত হয়। এটি UI থ্রেডকে ব্লক হওয়া থেকে রক্ষা করে এবং অ্যাপের প্রতিক্রিয়াশীলতা বজায় রাখে। এটি প্যারামিটার নেয় এবং একটি ফিউচার প্রদান করে।
```
dart
Future<int> heavyComputation(int value) async {
  // Simulate a heavy computation
  await Future.delayed(Duration(seconds: 5));
  return value * 2;
}

Future<void> startComputation() async {
  int result = await compute(heavyComputation, 10);
  print('Result: $result');
}
```
## প্রশ্ন ৫৬: Flutter-এ `AutomaticKeepAliveClientMixin` এর ব্যবহার ব্যাখ্যা করুন।

**উত্তর:** `AutomaticKeepAliveClientMixin` একটি মিক্সিন যা লিস্টভিউ, গ্রিডভিউ বা পেইজভিউয়ের মতো স্ক্রোলযোগ্য তালিকাগুলির মধ্যে উইজেটগুলির স্টেট বজায় রাখতে ব্যবহৃত হয়। ডিফল্টরূপে, যখন একটি উইজেট স্ক্রিনের বাইরে স্ক্রোল করে তখন Flutter এটিকে ডিসপোজ করতে পারে। এই মিক্সিন ব্যবহার করে, উইজেটগুলি স্ক্রিনের বাইরে গেলেও তাদের স্টেট বজায় রাখে, যা স্ক্রোলিং পারফরম্যান্স উন্নত করে।

```
dart
class MyListItem extends StatefulWidget {
  const MyListItem({Key? key}) : super(key: key);

  @override
  _MyListItemState createState() => _MyListItemState();
}

class _MyListItemState extends State<MyListItem> with AutomaticKeepAliveClientMixin<MyListItem> {
  int _counter = 0;

  @override
  bool get wantKeepAlive => true; // Keep the state alive

  void _incrementCounter() {
    setState(() {
      _counter++;
    });
  }

  @override
  Widget build(BuildContext context) {
    super.build(context); // Call super.build
    return ListTile(
      title: Text('Item $_counter'),
      onTap: _incrementCounter,
    );
  }
}
```
## প্রশ্ন ৫৭: Flutter-এ `Shader` এর ভূমিকা কী?

**উত্তর:** `Shader` ব্যবহার করে Flutter-এ কাস্টম গ্রাফিক্স এফেক্ট তৈরি করা যায়। এটি পিক্সেল-লেভেলে কাজ করে এবং বিভিন্ন ভিজ্যুয়াল এফেক্ট যেমন গ্রেডিয়েন্ট, প্যাটার্ন, ব্লার বা ওয়ার্প তৈরি করতে ব্যবহৃত হয়। Flutter Skia Graphics Engine ব্যবহার করে, যা SKSL (Skia Shader Language) সমর্থন করে।
```
dart
// Example of a simple gradient shader
Shader linearGradient = LinearGradient(
  colors: <Color>[Colors.red, Colors.blue],
).createShader(Rect.fromLTWH(0.0, 0.0, 200.0, 70.0));
```
## প্রশ্ন ৫৮: Flutter-এ `RenderObject` কীভাবে কাজ করে?

**উত্তর:** `RenderObject` হল Flutter এর রেন্ডারিং পাইপলাইনের একটি গুরুত্বপূর্ণ অংশ। এটি লেআউট এবং পেইন্টিং লজিক ধারণ করে। প্রতিটি উইজেটের একটি সংশ্লিষ্ট `RenderObject` থাকে যা নির্ধারণ করে কীভাবে উইজেটটি স্ক্রিনে প্রদর্শিত হবে। `RenderObject` ট্রি উইজেট ট্রি থেকে আলাদা এবং এটি UI এর ভিউরাল রিপ্রেজেন্টেশন পরিচালনা করে।

## প্রশ্ন ৫৯: Flutter-এ `Slivers` কখন ব্যবহার করা উচিত?

**উত্তর:** `Slivers` হলো স্ক্রোলযোগ্য এলাকার ছোট ছোট অংশ যা কাস্টম স্ক্রোলিং এফেক্ট তৈরি করতে ব্যবহৃত হয়। এটি সাধারণত `CustomScrollView` এর সাথে ব্যবহৃত হয়। `Slivers` ব্যবহার করে আপনি অ্যাপ বারের আকার পরিবর্তন করতে পারেন, লিস্ট আইটেমগুলিকে ভিন্নভাবে রেন্ডার করতে পারেন বা বিভিন্ন ধরণের স্ক্রোলিং উপাদান একত্রিত করতে পারেন। এটি পারফরম্যান্স উন্নত করতেও সাহায্য করে কারণ এটি শুধুমাত্র স্ক্রিনে দৃশ্যমান আইটেমগুলি রেন্ডার করে।

```
dart
CustomScrollView(
  slivers: <Widget>[
    SliverAppBar(
      expandedHeight: 200.0,
      flexibleSpace: FlexibleSpaceBar(
        title: Text('Sliver Example'),
      ),
    ),
    SliverList(
      delegate: SliverChildBuilderDelegate(
        (BuildContext context, int index) {
          return ListTile(title: Text('Item $index'));
        },
        childCount: 50,
      ),
    ),
  ],
);
```
## প্রশ্ন ৬০: Flutter-এ মেমরি লিক কীভাবে সনাক্ত এবং সমাধান করবেন?

**উত্তর:** মেমরি লিক ঘটে যখন মেমরি আর প্রয়োজন হয় না কিন্তু গার্বেজ কালেক্টর এটিকে সংগ্রহ করতে পারে না। Flutter-এ মেমরি লিক সনাক্ত করতে DevTools ব্যবহার করা যেতে পারে, বিশেষ করে মেমরি ট্যাব। এটি মেমরি ব্যবহারের গ্রাফ এবং অবজেক্ট বরাদ্দ দেখতে সাহায্য করে। মেমরি লিক সমাধানের জন্য:

* নিশ্চিত করুন যে ডিসপোজেবল অবজেক্ট (যেমন `AnimationController`, `StreamSubscription`) সঠিকভাবে ডিসপোজ করা হয়েছে।
* লিসেনারগুলি সরানো হয়েছে তা নিশ্চিত করুন যখন তাদের আর প্রয়োজন হয় না।
* সার্কুলার রেফারেন্স এড়িয়ে চলুন।
* প্রয়োজন না হলে বড় ডেটা স্ট্রাকচার বা ইমেজ মেমরিতে ধরে রাখবেন না।