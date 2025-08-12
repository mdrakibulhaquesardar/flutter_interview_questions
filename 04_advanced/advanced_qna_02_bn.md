## প্রশ্ন: Flutter এ কাস্টম ইম্প্লিসিট অ্যানিমেশনগুলি কীভাবে কাজ করে এবং কীভাবে এটি ব্যবহার করবেন তার বিস্তারিত ব্যাখ্যা দিন।

**উত্তর:**

Flutter এ অ্যানিমেশন তৈরি করার দুটি প্রধান উপায় রয়েছে: Explicit অ্যানিমেশন এবং Implicit অ্যানিমেশন। Implicit অ্যানিমেশনগুলি Explicit অ্যানিমেশনগুলির চেয়ে ব্যবহার করা অনেক সহজ, কারণ এগুলি স্বয়ংক্রিয়ভাবে নির্দিষ্ট প্রোপার্টির পরিবর্তন অ্যানিমেট করে। কাস্টম ইম্প্লিসিট অ্যানিমেশন আপনাকে নিজস্ব উইজেট তৈরি করতে দেয় যা Implicitly অ্যানিমেট করে।

**ইম্প্লিসিট অ্যানিমেশন কীভাবে কাজ করে:**

ইম্প্লিসিট অ্যানিমেশন উইজেটগুলি `ImplicitlyAnimatedWidget` থেকে এক্সটেন্ড করে। এই উইজেটগুলির একটি `Tween` প্রোপার্টি থাকে যা প্রোপার্টির শুরু এবং শেষের মান নির্ধারণ করে। যখন প্রোপার্টির মান পরিবর্তিত হয়, তখন উইজেট স্বয়ংক্রিয়ভাবে অ্যানিমেশন শুরু করে এবং মান পরিবর্তনকে নির্দিষ্ট সময় (duration) ধরে অ্যানিমেট করে।

**কাস্টম ইম্প্লিসিট অ্যানিমেশন তৈরির ধাপ:**

১. **`ImplicitlyAnimatedWidget` এক্সটেন্ড করুন:** একটি নতুন ক্লাস তৈরি করুন যা `ImplicitlyAnimatedWidget` থেকে এক্সটেন্ড করে।

২. **প্রোপার্টিগুলি সংজ্ঞায়িত করুন:** আপনি যে প্রোপার্টিগুলি অ্যানিমেট করতে চান তা সংজ্ঞায়িত করুন। এই প্রোপার্টিগুলি অবশ্যই `final` হতে হবে।

৩. **`AnimatedWidgetBaseState` তৈরি করুন:** আপনার উইজেটের জন্য একটি স্টেট ক্লাস তৈরি করুন যা `AnimatedWidgetBaseState` থেকে এক্সটেন্ড করে।

৪. **`tween` তৈরি করুন:** আপনার স্টেট ক্লাসে `createTween` মেথড ওভাররাইড করুন এবং আপনার প্রোপার্টির জন্য একটি `Tween` তৈরি করুন।

৫. **`lerp` মেথড ওভাররাইড করুন:** আপনার স্টেট ক্লাসে `lerp` মেথড ওভাররাইড করুন। এই মেথডটি অ্যানিমেশনের বর্তমান মান গণনা করে।

৬. **বিল্ড মেথড:** আপনার স্টেট ক্লাসের `build` মেথডে, অ্যানিমেশনের বর্তমান মান ব্যবহার করে উইজেটটি তৈরি করুন। `listenable` থেকে বর্তমান অ্যানিমেশন মান অ্যাক্সেস করতে পারেন।

**উদাহরণ:**

একটি উইজেট তৈরি করা যাক যা তার আকার (size) অ্যানিমেট করে:

```dart
import 'package:flutter/material.dart';

class AnimatedSizeWidget extends ImplicitlyAnimatedWidget {
  const AnimatedSizeWidget({
    Key? key,
    required this.size,
    required Duration duration,
    Curve curve = Curves.linear,
  }) : super(key: key, duration: duration, curve: curve);

  final double size;

  @override
  ImplicitlyAnimatedWidgetState<ImplicitlyAnimatedWidget> createState() => _AnimatedSizeWidgetState();
}

class _AnimatedSizeWidgetState extends AnimatedWidgetBaseState<AnimatedSizeWidget> {
  Tween<double>? _sizeTween;

  @override
  Tween<double>? createTween() {
    _sizeTween = Tween<double>(begin: widget.size, end: widget.size);
    return _sizeTween;
  }

  @override
  void forEachTween(TweenVisitor<dynamic> visitor) {
    _sizeTween = visitor(_sizeTween, widget.size, (dynamic value) => Tween<double>(begin: value, end: widget.size)) as Tween<double>?;
  }

  @override
  Widget build(BuildContext context) {
    final double animatedSize = _sizeTween!.evaluate(animation);
    return Container(
      width: animatedSize,
      height: animatedSize,
      color: Colors.blue,
    );
  }
}
```

এই উদাহরণে, `AnimatedSizeWidget` একটি `size` প্রোপার্টি নেয়। যখন `size` পরিবর্তিত হয়, তখন উইজেট স্বয়ংক্রিয়ভাবে আকার পরিবর্তনকে নির্দিষ্ট সময় ধরে অ্যানিমেট করে।

**কখন কাস্টম ইম্প্লিসিট অ্যানিমেশন ব্যবহার করবেন:**

* যখন আপনি একটি কাস্টম উইজেটের নির্দিষ্ট প্রোপার্টিগুলি অ্যানিমেট করতে চান।
* যখন আপনি অ্যানিমেশন লজিককে উইজেটের ভিতরে এনক্যাপসুলেট করতে চান।
* যখন আপনি Implicit অ্যানিমেশনের সুবিধা (যেমন সরলতা এবং স্বয়ংক্রিয় অ্যানিমেশন হ্যান্ডলিং) ব্যবহার করতে চান।

**সুবিধা:**

* **ব্যবহার করা সহজ:** Explicit অ্যানিমেশনের চেয়ে কম কোড লিখতে হয়।
* **স্বয়ংক্রিয় অ্যানিমেশন:** আপনাকে অ্যানিমেশন কন্ট্রোলার ম্যানুয়ালি নিয়ন্ত্রণ করতে হবে না।
* **পুনরায় ব্যবহারযোগ্য:** আপনার কাস্টম অ্যানিমেটেড উইজেটগুলি বিভিন্ন জায়গায় পুনরায় ব্যবহার করা যেতে পারে।

**অসুবিধা:**

* **সীমিত নমনীয়তা:** Explicit অ্যানিমেশনের চেয়ে কম নিয়ন্ত্রণ প্রদান করে।
* **কঠিন লজিক হ্যান্ডলিং:** জটিল অ্যানিমেশন সিকোয়েন্স বা নির্ভরতা হ্যান্ডেল করা কঠিন হতে পারে।

সংক্ষেপে, কাস্টম ইম্প্লিসিট অ্যানিমেশন Flutter এ নির্দিষ্ট প্রোপার্টিগুলি অ্যানিমেট করার জন্য একটি শক্তিশালী এবং সরল উপায় সরবরাহ করে। যখন আপনি একটি কাস্টম উইজেটের জন্য সহজ অ্যানিমেশন চান, তখন এটি একটি দুর্দান্ত বিকল্প।