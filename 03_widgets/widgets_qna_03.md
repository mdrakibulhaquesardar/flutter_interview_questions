# Flutter Widgets - প্রশ্নোত্তর সেট ৩

## Advanced Widget Concepts

### প্রশ্ন ১১: InheritedWidget কী এবং এটা কীভাবে কাজ করে?

**উত্তর (ডিটেইল):**

- **InheritedWidget** হলো Flutter-এর একটি powerful mechanism যা widget tree-এ data share করতে ব্যবহৃত হয়। এটা parent থেকে child widgets-এ data pass করে without explicit parameter passing।

**উদাহরণ:**

```dart
// Custom InheritedWidget
class UserData extends InheritedWidget {
  final String userName;
  final String userEmail;
  
  const UserData({
    super.key,
    required this.userName,
    required this.userEmail,
    required super.child,
  });
  
  static UserData of(BuildContext context) {
    return context.dependOnInheritedWidgetOfExactType<UserData>()!;
  }
  
  @override
  bool updateShouldNotify(UserData oldWidget) {
    return userName != oldWidget.userName || userEmail != oldWidget.userEmail;
  }
}

// Usage
class UserProfileScreen extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return UserData(
      userName: 'John Doe',
      userEmail: 'john@example.com',
      child: Scaffold(
        body: Column(
          children: [
            UserHeader(),
            UserDetails(),
          ],
        ),
      ),
    );
  }
}

class UserHeader extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    final userData = UserData.of(context);
    return Text('Hello, ${userData.userName}!');
  }
}
```

**Interview Tips:**
- `dependOnInheritedWidgetOfExactType` rebuild trigger করে
- `context.findAncestorWidgetOfExactType` rebuild trigger করে না
- Performance optimization-এ careful হোন

---

### প্রশ্ন ১২: CustomPainter কীভাবে ব্যবহার করবেন?

**উত্তর (ডিটেইল):**

- **CustomPainter** হলো Flutter-এ custom graphics drawing করার জন্য ব্যবহৃত class। এটা Canvas API ব্যবহার করে custom shapes, paths, এবং graphics draw করে।

**উদাহরণ:**

```dart
class ChartPainter extends CustomPainter {
  final List<double> data;
  final Color lineColor;
  
  ChartPainter({required this.data, required this.lineColor});
  
  @override
  void paint(Canvas canvas, Size size) {
    if (data.isEmpty) return;
    
    final paint = Paint()
      ..color = lineColor
      ..strokeWidth = 3.0
      ..style = PaintingStyle.stroke;
    
    final path = Path();
    final width = size.width;
    final height = size.height;
    final maxValue = data.reduce((a, b) => a > b ? a : b);
    
    // Draw line chart
    for (int i = 0; i < data.length; i++) {
      final x = (i / (data.length - 1)) * width;
      final y = height - (data[i] / maxValue) * height;
      
      if (i == 0) {
        path.moveTo(x, y);
      } else {
        path.lineTo(x, y);
      }
    }
    
    canvas.drawPath(path, paint);
  }
  
  @override
  bool shouldRepaint(ChartPainter oldDelegate) {
    return oldDelegate.data != data || oldDelegate.lineColor != lineColor;
  }
}

// Usage
CustomPaint(
  painter: ChartPainter(
    data: [10, 25, 15, 30, 20],
    lineColor: Colors.blue,
  ),
  size: Size.infinite,
)
```

**Interview Tips:**
- Canvas API-এর methods জানুন
- Path operations efficient করুন
- Repaint optimization গুরুত্বপূর্ণ

---

### প্রশ্ন ১৩: RepaintBoundary কী এবং কখন ব্যবহার করবেন?

**উত্তর (ডিটেইল):**

- **RepaintBoundary** হলো Flutter-এ performance optimization-এর জন্য ব্যবহৃত widget। এটা widget subtree-কে isolate করে যাতে unnecessary repaints avoid করা যায়।

**উদাহরণ:**

```dart
class AnimatedWidget extends StatefulWidget {
  @override
  State<AnimatedWidget> createState() => _AnimatedWidgetState();
}

class _AnimatedWidgetState extends State<AnimatedWidget>
    with SingleTickerProviderStateMixin {
  late AnimationController _controller;
  
  @override
  void initState() {
    super.initState();
    _controller = AnimationController(
      duration: const Duration(seconds: 2),
      vsync: this,
    )..repeat();
  }
  
  @override
  void dispose() {
    _controller.dispose();
    super.dispose();
  }
  
  @override
  Widget build(BuildContext context) {
    return Column(
      children: [
        // Widget that changes frequently
        Text('Counter: ${DateTime.now().millisecond}'),
        
        // RepaintBoundary around animated widget
        RepaintBoundary(
          child: AnimatedBuilder(
            animation: _controller,
            builder: (context, child) {
              return Transform.rotate(
                angle: _controller.value * 2 * 3.14159,
                child: Container(
                  width: 60,
                  height: 60,
                  color: Colors.blue,
                  child: const Icon(Icons.star, color: Colors.white),
                ),
              );
            },
          ),
        ),
      ],
    );
  }
}
```

**Interview Tips:**
- Expensive animations-এর জন্য ব্যবহার করুন
- Overuse avoid করুন
- Profile before optimizing
