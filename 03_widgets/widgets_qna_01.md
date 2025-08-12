# Flutter Widgets - প্রশ্নোত্তর সেট ১

## Widget System এর মৌলিক ধারণা

### প্রশ্ন ১: StatelessWidget আর StatefulWidget-এর মধ্যে মূল পার্থক্য কী?

**উত্তর (ডিটেইল):**

- **StatelessWidget**: Immutable widget যা একবার তৈরি হলে পরিবর্তন হয় না। এতে কোন internal state নেই, শুধুমাত্র parent থেকে props নিয়ে UI রেন্ডার করে। Performance-wise এটা বেশি efficient কারণ Flutter এটাকে optimize করতে পারে।

- **StatefulWidget**: Mutable state সহ widget যা user interaction বা data change অনুযায়ী নিজেকে rebuild করতে পারে। এতে `State` object থাকে যা widget-এর lifecycle ম্যানেজ করে।

**উদাহরণ:**

```dart
// StatelessWidget - immutable, no state
class WelcomeMessage extends StatelessWidget {
  final String name;
  
  const WelcomeMessage({super.key, required this.name});
  
  @override
  Widget build(BuildContext context) {
    return Text('Welcome, $name!');
  }
}

// StatefulWidget - mutable state
class Counter extends StatefulWidget {
  const Counter({super.key});
  
  @override
  State<Counter> createState() => _CounterState();
}

class _CounterState extends State<Counter> {
  int _count = 0;
  
  void _increment() {
    setState(() {
      _count++;
    });
  }
  
  @override
  Widget build(BuildContext context) {
    return Column(
      children: [
        Text('Count: $_count'),
        ElevatedButton(
          onPressed: _increment,
          child: const Text('Increment'),
        ),
      ],
    );
  }
}
```

**Common Pitfalls:**
- StatelessWidget-এ state রাখার চেষ্টা করা
- StatefulWidget ব্যবহার করা যখন StatelessWidget যথেষ্ট
- `setState` না দিয়ে state পরিবর্তন করা

**Interview Tips:** 
- UI static থাকলে → StatelessWidget
- UI dynamic থাকলে → StatefulWidget
- Performance critical হলে StatelessWidget prioritize করুন

---

### প্রশ্ন ২: BuildContext কী এবং এটা কেন গুরুত্বপূর্ণ?

**উত্তর (ডিটেইল):**

- **BuildContext** হলো widget tree-এর একটি handle যা widget-কে তার parent, ancestor, এবং global services-এর সাথে connect করে। এটি widget-এর position এবং environment সম্পর্কে information প্রদান করে।

- **মূল ভূমিকা:**
  - Theme, MediaQuery, Navigator access
  - Provider/Riverpod state access
  - Parent widget-এর সাথে communication
  - Localization access

**উদাহরণ:**

```dart
class MyWidget extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    // Theme access
    final theme = Theme.of(context);
    final textColor = theme.colorScheme.onSurface;
    
    // MediaQuery access
    final size = MediaQuery.of(context).size;
    final isLandscape = size.width > size.height;
    
    // Provider access
    final counter = context.watch<Counter>();
    
    // Navigator access
    void _navigateToNext() {
      Navigator.of(context).pushNamed('/next');
    }
    
    return Container(
      color: textColor,
      child: Text(
        'Counter: ${counter.value}',
        style: theme.textTheme.headlineMedium,
      ),
    );
  }
}
```

**Common Pitfalls:**
- `context` null check না করা
- Wrong context ব্যবহার করা (parent vs child)
- BuildContext-কে store করা

**Interview Tips:**
- BuildContext = widget-এর identity card
- Always use the context from build method
- Context-কে class field হিসেবে store করবেন না

---

### প্রশ্ন ৩: Custom Widget কীভাবে তৈরি করবেন এবং কখন ব্যবহার করবেন?

**উত্তর (ডিটেইল):**

- **Custom Widget** হলো reusable UI component যা business logic এবং presentation logic encapsulate করে। এটা code reusability, maintainability, এবং testing improve করে।

**উদাহরণ:**

```dart
// Custom Button Widget
class CustomButton extends StatelessWidget {
  final String text;
  final VoidCallback? onPressed;
  final bool isLoading;
  final Color? backgroundColor;
  
  const CustomButton({
    super.key,
    required this.text,
    this.onPressed,
    this.isLoading = false,
    this.backgroundColor,
  });
  
  @override
  Widget build(BuildContext context) {
    return ElevatedButton(
      onPressed: isLoading ? null : onPressed,
      style: ElevatedButton.styleFrom(
        backgroundColor: backgroundColor,
        minimumSize: const Size(120, 48),
      ),
      child: isLoading
          ? const SizedBox(
              width: 20,
              height: 20,
              child: CircularProgressIndicator(strokeWidth: 2),
            )
          : Text(text),
    );
  }
}

// Usage
CustomButton(
  text: 'Submit',
  isLoading: _isSubmitting,
  onPressed: _handleSubmit,
  backgroundColor: Colors.blue,
)
```

**কখন ব্যবহার করবেন:**
- UI component বারবার ব্যবহার করতে হবে
- Complex logic সহ widget
- Team collaboration-এ consistency দরকার
- Testing-এ widget-কে isolate করতে হবে

**Interview Tips:**
- Single Responsibility Principle follow করুন
- Props-কে required/optional হিসেবে organize করুন
- Default values provide করুন
- Documentation লিখুন

---

### প্রশ্ন ৪: Widget Keys কী এবং কখন ব্যবহার করবেন?

**উত্তর (ডিটেইল):**

- **Widget Keys** হলো Flutter-এ widget-কে uniquely identify করার জন্য ব্যবহৃত identifier। এটা widget tree-এ widget-এর identity maintain করে যখন parent rebuild হয়।

**Key Types:**
- **ValueKey**: String, int, বা object value দিয়ে
- **GlobalKey**: Global scope-এ unique
- **UniqueKey**: প্রতিবার unique key generate করে
- **ObjectKey**: Object reference দিয়ে

**উদাহরণ:**

```dart
class TodoList extends StatefulWidget {
  @override
  State<TodoList> createState() => _TodoListState();
}

class _TodoListState extends State<TodoList> {
  final List<Todo> _todos = [];
  
  @override
  Widget build(BuildContext context) {
    return ListView.builder(
      itemCount: _todos.length,
      itemBuilder: (context, index) {
        final todo = _todos[index];
        return TodoItem(
          key: ValueKey(todo.id), // Unique identification
          todo: todo,
          onDelete: () => _deleteTodo(todo.id),
        );
      },
    );
  }
}

// GlobalKey example
class MyForm extends StatefulWidget {
  @override
  State<MyForm> createState() => _MyFormState();
}

class _MyFormState extends State<MyForm> {
  final _formKey = GlobalKey<FormState>();
  final _nameController = TextEditingController();
  
  void _submitForm() {
    if (_formKey.currentState!.validate()) {
      // Form is valid
      print('Name: ${_nameController.text}');
    }
  }
  
  @override
  Widget build(BuildContext context) {
    return Form(
      key: _formKey,
      child: Column(
        children: [
          TextFormField(
            controller: _nameController,
            validator: (value) {
              if (value == null || value.isEmpty) {
                return 'Please enter a name';
              }
              return null;
            },
          ),
          ElevatedButton(
            onPressed: _submitForm,
            child: const Text('Submit'),
          ),
        ],
      ),
    );
  }
}
```

**কখন Keys ব্যবহার করবেন:**
- List-এ items add/remove করার সময়
- Form validation-এ
- StatefulWidget-এর state preserve করতে
- Widget-কে programmatically access করতে

**Interview Tips:**
- Keys expensive, শুধু প্রয়োজন হলে ব্যবহার করুন
- ValueKey সবচেয়ে efficient
- GlobalKey sparingly ব্যবহার করুন
- Keys-কে meaningful রাখুন

---

### প্রশ্ন ৫: Widget Lifecycle কীভাবে manage করবেন?

**উত্তর (ডিটেইল):**

- **Widget Lifecycle** হলো widget-এর creation থেকে destruction পর্যন্ত সম্পূর্ণ journey। প্রতিটি stage-এ specific কাজ করা যায়।

**StatefulWidget Lifecycle:**

```dart
class LifecycleWidget extends StatefulWidget {
  const LifecycleWidget({super.key});
  
  @override
  State<LifecycleWidget> createState() => _LifecycleWidgetState();
}

class _LifecycleWidgetState extends State<LifecycleWidget> 
    with WidgetsBindingObserver {
  
  late final AnimationController _animationController;
  late final StreamSubscription _subscription;
  
  @override
  void initState() {
    super.initState();
    print('1. initState: Widget created');
    
    // Initialize controllers, streams, etc.
    _animationController = AnimationController(
      duration: const Duration(seconds: 1),
      vsync: this,
    );
    
    _subscription = Stream.periodic(
      const Duration(seconds: 1),
      (i) => i,
    ).listen((data) {
      print('Stream data: $data');
    });
    
    // Add observer for app lifecycle
    WidgetsBinding.instance.addObserver(this);
  }
  
  @override
  void didChangeDependencies() {
    super.didChangeDependencies();
    print('2. didChangeDependencies: Dependencies changed');
    
    // Access inherited widgets, media query, etc.
    final mediaQuery = MediaQuery.of(context);
    print('Screen size: ${mediaQuery.size}');
  }
  
  @override
  void didUpdateWidget(LifecycleWidget oldWidget) {
    super.didUpdateWidget(oldWidget);
    print('3. didUpdateWidget: Parent updated this widget');
    
    // Compare old and new widget properties
    if (oldWidget.key != widget.key) {
      print('Widget key changed');
    }
  }
  
  @override
  void didChangeAppLifecycleState(AppLifecycleState state) {
    super.didChangeAppLifecycleState(state);
    print('App lifecycle: $state');
    
    switch (state) {
      case AppLifecycleState.resumed:
        _animationController.forward();
        break;
      case AppLifecycleState.paused:
        _animationController.stop();
        break;
      case AppLifecycleState.detached:
        _animationController.dispose();
        break;
      default:
        break;
    }
  }
  
  @override
  void deactivate() {
    print('4. deactivate: Widget removed from tree');
    super.deactivate();
  }
  
  @override
  void dispose() {
    print('5. dispose: Widget destroyed');
    
    // Clean up resources
    _animationController.dispose();
    _subscription.cancel();
    WidgetsBinding.instance.removeObserver(this);
    
    super.dispose();
  }
  
  @override
  Widget build(BuildContext context) {
    print('6. build: Building widget tree');
    
    return Scaffold(
      appBar: AppBar(title: const Text('Lifecycle Demo')),
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            const Text('Check console for lifecycle logs'),
            ElevatedButton(
              onPressed: () {
                setState(() {
                  // Trigger rebuild
                });
              },
              child: const Text('Trigger Rebuild'),
            ),
          ],
        ),
      ),
    );
  }
}
```

**Lifecycle Best Practices:**
- `initState`-এ async কাজ করবেন না
- `dispose`-এ সব resource cleanup করুন
- `didChangeDependencies`-এ context-dependent initialization করুন
- `didUpdateWidget`-এ performance optimization করুন

**Interview Tips:**
- Lifecycle methods-এর order মনে রাখুন
- Resource management গুরুত্বপূর্ণ
- Context access-এর timing বুঝুন
- Performance optimization-এ focus করুন
