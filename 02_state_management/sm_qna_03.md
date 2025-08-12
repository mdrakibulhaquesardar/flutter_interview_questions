### প্রশ্ন ১১: BLoC প্যাটার্ন কী? Core আইডিয়া কী?

**উত্তর (ডিটেইল):**

**BLoC (Business Logic Component)** হলো Flutter-এর জন্য একটি আর্কিটেকচারাল প্যাটার্ন যা UI এবং business logic-কে আলাদা করে। এটি Google-এর দ্বারা প্রস্তাবিত এবং Flutter community-তে ব্যাপকভাবে ব্যবহৃত হয়।

**BLoC-এর Core আইডিয়া:**

**১. Unidirectional Data Flow:**
- Event → BLoC → State → UI
- UI শুধুমাত্র Event পাঠায় এবং State গ্রহণ করে
- BLoC business logic handle করে এবং State emit করে

**২. Separation of Concerns:**
- UI layer: শুধুমাত্র presentation logic
- BLoC layer: business logic এবং state management
- Data layer: API calls, database operations

**৩. Reactive Programming:**
- UI automatically update হয় যখন state পরিবর্তিত হয়
- Stream-based architecture
- Declarative UI updates

**BLoC Architecture Diagram:**

```
UI Layer          BLoC Layer         Data Layer
┌─────────┐      ┌─────────┐      ┌─────────┐
│   UI    │─────▶│  BLoC   │─────▶│  Data   │
│         │◀─────│         │◀─────│         │
└─────────┘      └─────────┘      └─────────┘
   │                   │                   │
   │                   ▼                   ▼
Events              States             Services
```

**উদাহরণ:**

```dart
// 1. Event classes
abstract class CounterEvent {}

class IncrementEvent extends CounterEvent {}
class DecrementEvent extends CounterEvent {}
class ResetEvent extends CounterEvent {}

// 2. State class
class CounterState {
  final int count;
  final bool isLoading;
  final String? error;

  const CounterState({
    required this.count,
    this.isLoading = false,
    this.error,
  });

  CounterState copyWith({
    int? count,
    bool? isLoading,
    String? error,
  }) {
    return CounterState(
      count: count ?? this.count,
      isLoading: isLoading ?? this.isLoading,
      error: error ?? this.error,
    );
  }

  @override
  bool operator ==(Object other) =>
      identical(this, other) ||
      other is CounterState &&
          runtimeType == other.runtimeType &&
          count == other.count &&
          isLoading == other.isLoading &&
          error == other.error;

  @override
  int get hashCode => count.hashCode ^ isLoading.hashCode ^ error.hashCode;
}

// 3. BLoC class
class CounterBloc extends Bloc<CounterEvent, CounterState> {
  CounterBloc() : super(const CounterState(count: 0)) {
    on<IncrementEvent>(_onIncrement);
    on<DecrementEvent>(_onDecrement);
    on<ResetEvent>(_onReset);
  }

  void _onIncrement(IncrementEvent event, Emitter<CounterState> emit) {
    emit(state.copyWith(count: state.count + 1));
  }

  void _onDecrement(DecrementEvent event, Emitter<CounterState> emit) {
    emit(state.copyWith(count: state.count - 1));
  }

  void _onReset(ResetEvent event, Emitter<CounterState> emit) {
    emit(const CounterState(count: 0));
  }
}

// 4. UI Widget
class CounterWidget extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return BlocProvider(
      create: (context) => CounterBloc(),
      child: BlocBuilder<CounterBloc, CounterState>(
        builder: (context, state) {
          return Column(
            children: [
              Text('Count: ${state.count}'),
              if (state.isLoading) CircularProgressIndicator(),
              if (state.error != null) Text('Error: ${state.error}'),
              Row(
                children: [
                  ElevatedButton(
                    onPressed: () {
                      context.read<CounterBloc>().add(IncrementEvent());
                    },
                    child: Text('Increment'),
                  ),
                  ElevatedButton(
                    onPressed: () {
                      context.read<CounterBloc>().add(DecrementEvent());
                    },
                    child: Text('Decrement'),
                  ),
                  ElevatedButton(
                    onPressed: () {
                      context.read<CounterBloc>().add(ResetEvent());
                    },
                    child: Text('Reset'),
                  ),
                ],
              ),
            ],
          );
        },
      ),
    );
  }
}
```

**BLoC-এর সুবিধা:**

**১. Testability:**
- Business logic UI থেকে আলাদা, তাই unit test করা সহজ
- Event এবং State predictable
- Mock data সহজে inject করা যায়

**২. Reusability:**
- একই BLoC বিভিন্ন UI-তে ব্যবহার করা যায়
- Business logic share করা যায়

**৩. Maintainability:**
- Code organization ভালো
- Debugging সহজ
- Feature-based architecture

**৪. Scalability:**
- বড় অ্যাপ্লিকেশনের জন্য উপযুক্ত
- Complex business logic handle করতে পারে
- Team collaboration সহজ

**Pitfalls:**
1. Over-engineering: ছোট feature-এর জন্য BLoC complex
2. Boilerplate code: অনেক class এবং method লিখতে হয়
3. Learning curve: নতুন developer-দের জন্য শেখা কঠিন

**Interview Tips:** 
- Complex business logic → BLoC
- Simple state → setState বা Provider
- Event-driven architecture → BLoC
- Form validation, API calls, complex UI state → BLoC

---

### প্রশ্ন ১২: `bloc` vs `cubit` পার্থক্য কী?

**উত্তর (ডিটেইল):**

**BLoC** এবং **Cubit** উভয়ই BLoC pattern-এর implementation, কিন্তু এদের মধ্যে গুরুত্বপূর্ণ পার্থক্য রয়েছে।

**Cubit:**
- **Simple API**: সরাসরি method call করে state emit করে
- **Less Boilerplate**: Event class-এর প্রয়োজন নেই
- **Easy to Learn**: নতুন developer-দের জন্য সহজ
- **Quick Implementation**: দ্রুত prototype করার জন্য উপযুক্ত

**BLoC:**
- **Event-Driven**: Event class-এর মাধ্যমে action handle করে
- **More Structured**: Complex business logic-এর জন্য ভালো
- **Better for Large Apps**: বড় অ্যাপ্লিকেশনের জন্য উপযুক্ত
- **Analytics & Logging**: Event tracking সহজ

**Cubit উদাহরণ:**

```dart
// Cubit - Simple API
class CounterCubit extends Cubit<CounterState> {
  CounterCubit() : super(const CounterState(count: 0));

  void increment() {
    emit(state.copyWith(count: state.count + 1));
  }

  void decrement() {
    emit(state.copyWith(count: state.count - 1));
  }

  void reset() {
    emit(const CounterState(count: 0));
  }

  Future<void> incrementAsync() async {
    emit(state.copyWith(isLoading: true));
    
    await Future.delayed(Duration(seconds: 1));
    
    emit(state.copyWith(
      count: state.count + 1,
      isLoading: false,
    ));
  }
}

// UI with Cubit
class CounterWidget extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return BlocProvider(
      create: (context) => CounterCubit(),
      child: BlocBuilder<CounterCubit, CounterState>(
        builder: (context, state) {
          return Column(
            children: [
              Text('Count: ${state.count}'),
              if (state.isLoading) CircularProgressIndicator(),
              Row(
                children: [
                  ElevatedButton(
                    onPressed: () {
                      context.read<CounterCubit>().increment();
                    },
                    child: Text('Increment'),
                  ),
                  ElevatedButton(
                    onPressed: () {
                      context.read<CounterCubit>().decrement();
                    },
                    child: Text('Decrement'),
                  ),
                  ElevatedButton(
                    onPressed: () {
                      context.read<CounterCubit>().reset();
                    },
                    child: Text('Reset'),
                  ),
                ],
              ),
            ],
          );
        },
      ),
    );
  }
}
```

**BLoC উদাহরণ:**

```dart
// BLoC - Event-driven
abstract class CounterEvent {}

class IncrementEvent extends CounterEvent {}
class DecrementEvent extends CounterEvent {}
class ResetEvent extends CounterEvent {}
class IncrementAsyncEvent extends CounterEvent {}

class CounterBloc extends Bloc<CounterEvent, CounterState> {
  CounterBloc() : super(const CounterState(count: 0)) {
    on<IncrementEvent>(_onIncrement);
    on<DecrementEvent>(_onDecrement);
    on<ResetEvent>(_onReset);
    on<IncrementAsyncEvent>(_onIncrementAsync);
  }

  void _onIncrement(IncrementEvent event, Emitter<CounterState> emit) {
    emit(state.copyWith(count: state.count + 1));
  }

  void _onDecrement(DecrementEvent event, Emitter<CounterState> emit) {
    emit(state.copyWith(count: state.count - 1));
  }

  void _onReset(ResetEvent event, Emitter<CounterState> emit) {
    emit(const CounterState(count: 0));
  }

  Future<void> _onIncrementAsync(
    IncrementAsyncEvent event,
    Emitter<CounterState> emit,
  ) async {
    emit(state.copyWith(isLoading: true));
    
    await Future.delayed(Duration(seconds: 1));
    
    emit(state.copyWith(
      count: state.count + 1,
      isLoading: false,
    ));
  }
}

// UI with BLoC
class CounterWidget extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return BlocProvider(
      create: (context) => CounterBloc(),
      child: BlocBuilder<CounterBloc, CounterState>(
        builder: (context, state) {
          return Column(
            children: [
              Text('Count: ${state.count}'),
              if (state.isLoading) CircularProgressIndicator(),
              Row(
                children: [
                  ElevatedButton(
                    onPressed: () {
                      context.read<CounterBloc>().add(IncrementEvent());
                    },
                    child: Text('Increment'),
                  ),
                  ElevatedButton(
                    onPressed: () {
                      context.read<CounterBloc>().add(DecrementEvent());
                    },
                    child: Text('Decrement'),
                  ),
                  ElevatedButton(
                    onPressed: () {
                      context.read<CounterBloc>().add(ResetEvent());
                    },
                    child: Text('Reset'),
                  ),
                  ElevatedButton(
                    onPressed: () {
                      context.read<CounterBloc>().add(IncrementAsyncEvent());
                    },
                    child: Text('Async Increment'),
                  ),
                ],
              ),
            ],
          );
        },
      ),
    );
  }
}
```

**Cubit vs BLoC তুলনা:**

| বৈশিষ্ট্য | Cubit | BLoC |
|-----------|-------|------|
| API Complexity | Simple | Complex |
| Boilerplate | Less | More |
| Event Classes | ❌ | ✅ |
| Learning Curve | Easy | Steep |
| Use Case | Simple state | Complex logic |
| Analytics | Limited | Better |
| Testing | Easy | More structured |

**কখন কোনটা ব্যবহার করবেন:**

**Cubit ব্যবহার করবেন:**
- Simple state management
- Quick prototyping
- Small features
- Learning BLoC pattern
- Less complex business logic

**BLoC ব্যবহার করবেন:**
- Complex business logic
- Event-driven architecture
- Large applications
- Analytics requirements
- Team development

**Pitfalls:**
1. Cubit-এ complex logic handle করা কঠিন
2. BLoC-এ over-engineering হতে পারে
3. Cubit-এ event tracking কঠিন

**Interview Tips:** 
- Simple state → Cubit
- Complex logic → BLoC
- Learning curve → Cubit first
- Production app → BLoC for complex features

---

### প্রশ্ন ১৩: BlocBuilder/BlocListener/BlocConsumer-এর ভুমিকা কী?

**উত্তর (ডিটেইল):**

BLoC pattern-এ UI এবং BLoC-এর মধ্যে communication করার জন্য তিনটি মূল widget রয়েছে, প্রতিটির আলাদা উদ্দেশ্য আছে।

**১. `BlocBuilder`:**
- **কাজ**: State change হলে UI rebuild করে
- **কখন ব্যবহার করবেন**: যখন state-এর উপর নির্ভর করে UI update করতে হয়
- **সতর্কতা**: `build` method-এর বাইরে ব্যবহার করবেন না

```dart
class CounterWidget extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return BlocBuilder<CounterBloc, CounterState>(
      builder: (context, state) {
        return Column(
          children: [
            Text('Count: ${state.count}'),
            if (state.isLoading) CircularProgressIndicator(),
            if (state.error != null) Text('Error: ${state.error}'),
          ],
        );
      },
    );
  }
}
```

**২. `BlocListener`:**
- **কাজ**: State change হলে side effect trigger করে, কিন্তু UI rebuild করে না
- **কখন ব্যবহার করবেন**: Navigation, snackbar, dialog, বা অন্য side effect
- **সতর্কতা**: Listener-এ infinite loop তৈরি করবেন না

```dart
class AuthWidget extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return BlocListener<AuthBloc, AuthState>(
      listener: (context, state) {
        if (state is AuthStateAuthenticated) {
          // Navigate to home
          Navigator.of(context).pushReplacementNamed('/home');
        } else if (state is AuthStateUnauthenticated) {
          // Show login form
          ScaffoldMessenger.of(context).showSnackBar(
            SnackBar(content: Text('Please log in')),
          );
        } else if (state is AuthStateError) {
          // Show error
          ScaffoldMessenger.of(context).showSnackBar(
            SnackBar(content: Text('Error: ${state.message}')),
          );
        }
      },
      child: BlocBuilder<AuthBloc, AuthState>(
        builder: (context, state) {
          return state.when(
            authenticated: (user) => Text('Welcome ${user.name}'),
            unauthenticated: () => LoginForm(),
            loading: () => CircularProgressIndicator(),
            error: (message) => Text('Error: $message'),
          );
        },
      ),
    );
  }
}
```

**৩. `BlocConsumer`:**
- **কাজ**: `BlocBuilder` এবং `BlocListener` একসাথে
- **কখন ব্যবহার করবেন**: যখন UI update এবং side effect উভয়ই প্রয়োজন
- **সুবিধা**: Code organization ভালো

```dart
class LoginWidget extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return BlocConsumer<AuthBloc, AuthState>(
      listener: (context, state) {
        if (state is AuthStateAuthenticated) {
          Navigator.of(context).pushReplacementNamed('/home');
        } else if (state is AuthStateError) {
          ScaffoldMessenger.of(context).showSnackBar(
            SnackBar(content: Text('Login failed: ${state.message}')),
          );
        }
      },
      builder: (context, state) {
        return Form(
          child: Column(
            children: [
              TextFormField(
                decoration: InputDecoration(labelText: 'Email'),
                onChanged: (value) {
                  context.read<AuthBloc>().add(EmailChangedEvent(value));
                },
              ),
              TextFormField(
                decoration: InputDecoration(labelText: 'Password'),
                obscureText: true,
                onChanged: (value) {
                  context.read<AuthBloc>().add(PasswordChangedEvent(value));
                },
              ),
              ElevatedButton(
                onPressed: state.isLoading
                    ? null
                    : () {
                        context.read<AuthBloc>().add(LoginEvent());
                      },
                child: state.isLoading
                    ? CircularProgressIndicator()
                    : Text('Login'),
              ),
            ],
          ),
        );
      },
    );
  }
}
```

**Advanced Usage:**

**১. Conditional Building:**
```dart
class ConditionalWidget extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return BlocBuilder<CounterBloc, CounterState>(
      buildWhen: (previous, current) {
        // শুধুমাত্র count পরিবর্তন হলে rebuild হবে
        return previous.count != current.count;
      },
      builder: (context, state) {
        return Text('Count: ${state.count}');
      },
    );
  }
}
```

**২. Selective Listening:**
```dart
class NotificationWidget extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return BlocListener<NotificationBloc, NotificationState>(
      listenWhen: (previous, current) {
        // শুধুমাত্র notification count বাড়লে listen করবে
        return current.count > previous.count;
      },
      listener: (context, state) {
        ScaffoldMessenger.of(context).showSnackBar(
          SnackBar(content: Text('You have ${state.count} new notifications')),
        );
      },
      child: BlocBuilder<NotificationBloc, NotificationState>(
        builder: (context, state) {
          return Badge(
            label: Text('${state.count}'),
            child: Icon(Icons.notifications),
          );
        },
      ),
    );
  }
}
```

**৩. Multiple BLoCs:**
```dart
class DashboardWidget extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MultiBlocListener(
      listeners: [
        BlocListener<AuthBloc, AuthState>(
          listener: (context, state) {
            if (state is AuthStateUnauthenticated) {
              Navigator.of(context).pushReplacementNamed('/login');
            }
          },
        ),
        BlocListener<NotificationBloc, NotificationState>(
          listener: (context, state) {
            if (state.hasNewNotifications) {
              ScaffoldMessenger.of(context).showSnackBar(
                SnackBar(content: Text('New notification received')),
              );
            }
          },
        ),
      ],
      child: BlocBuilder<UserBloc, UserState>(
        builder: (context, state) {
          return Column(
            children: [
              Text('Welcome ${state.user.name}'),
              NotificationBadge(),
              UserProfile(),
            ],
          );
        },
      ),
    );
  }
}
```

**Performance Optimization:**

**১. `buildWhen` ব্যবহার:**
```dart
class OptimizedWidget extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return BlocBuilder<CounterBloc, CounterState>(
      buildWhen: (previous, current) {
        // শুধুমাত্র count পরিবর্তন হলে rebuild হবে
        return previous.count != current.count;
      },
      builder: (context, state) {
        return Text('Count: ${state.count}');
      },
    );
  }
}
```

**২. `listenWhen` ব্যবহার:**
```dart
class OptimizedListener extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return BlocListener<AuthBloc, AuthState>(
      listenWhen: (previous, current) {
        // শুধুমাত্র authentication status পরিবর্তন হলে listen করবে
        return previous.isAuthenticated != current.isAuthenticated;
      },
      listener: (context, state) {
        if (state.isAuthenticated) {
          Navigator.of(context).pushReplacementNamed('/home');
        }
      },
      child: LoginForm(),
    );
  }
}
```

**Pitfalls:**
1. `BlocBuilder`-এ complex logic রাখলে performance issue হয়
2. `BlocListener`-এ infinite loop তৈরি করলে app crash হতে পারে
3. `BlocConsumer`-এ overuse করলে code readability খারাপ হয়

**Interview Tips:** 
- UI update → `BlocBuilder`
- Side effect → `BlocListener`
- Both → `BlocConsumer`
- Performance → `buildWhen`/`listenWhen`
- Multiple BLoCs → `MultiBlocListener`

---

### প্রশ্ন ১৪: BLoC-এ পারফরম্যান্স অপ্টিমাইজ?

**উত্তর (ডিটেইল):**

BLoC-এ পারফরম্যান্স অপ্টিমাইজেশন একটি গুরুত্বপূর্ণ বিষয়, বিশেষত বড় অ্যাপ্লিকেশনে। এখানে কয়েকটি কার্যকর কৌশল:

**১. `buildWhen` এবং `listenWhen` ব্যবহার:**

```dart
class OptimizedCounterWidget extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return BlocBuilder<CounterBloc, CounterState>(
      buildWhen: (previous, current) {
        // শুধুমাত্র count পরিবর্তন হলে rebuild হবে
        return previous.count != current.count;
      },
      builder: (context, state) {
        return Text('Count: ${state.count}');
      },
    );
  }
}

class OptimizedAuthWidget extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return BlocListener<AuthBloc, AuthState>(
      listenWhen: (previous, current) {
        // শুধুমাত্র authentication status পরিবর্তন হলে listen করবে
        return previous.isAuthenticated != current.isAuthenticated;
      },
      listener: (context, state) {
        if (state.isAuthenticated) {
          Navigator.of(context).pushReplacementNamed('/home');
        }
      },
      child: LoginForm(),
    );
  }
}
```

**২. Widget Splitting:**

```dart
// খারাপ উদাহরণ - পুরো widget rebuild হয়
class BadCounterWidget extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return BlocBuilder<CounterBloc, CounterState>(
      builder: (context, state) {
        return Column(
          children: [
            Text('Count: ${state.count}'),
            if (state.isLoading) CircularProgressIndicator(),
            if (state.error != null) Text('Error: ${state.error}'),
            Row(
              children: [
                ElevatedButton(
                  onPressed: () {
                    context.read<CounterBloc>().add(IncrementEvent());
                  },
                  child: Text('Increment'),
                ),
                ElevatedButton(
                  onPressed: () {
                    context.read<CounterBloc>().add(DecrementEvent());
                  },
                  child: Text('Decrement'),
                ),
              ],
            ),
          ],
        );
      },
    );
  }
}

// ভালো উদাহরণ - শুধুমাত্র প্রয়োজনীয় অংশ rebuild হয়
class GoodCounterWidget extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Column(
      children: [
        CounterDisplay(),
        LoadingIndicator(),
        ErrorDisplay(),
        CounterButtons(),
      ],
    );
  }
}

class CounterDisplay extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return BlocBuilder<CounterBloc, CounterState>(
      buildWhen: (previous, current) => previous.count != current.count,
      builder: (context, state) {
        return Text('Count: ${state.count}');
      },
    );
  }
}

class LoadingIndicator extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return BlocBuilder<CounterBloc, CounterState>(
      buildWhen: (previous, current) => previous.isLoading != current.isLoading,
      builder: (context, state) {
        return state.isLoading ? CircularProgressIndicator() : SizedBox.shrink();
      },
    );
  }
}

class ErrorDisplay extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return BlocBuilder<CounterBloc, CounterState>(
      buildWhen: (previous, current) => previous.error != current.error,
      builder: (context, state) {
        return state.error != null
            ? Text('Error: ${state.error}')
            : SizedBox.shrink();
      },
    );
  }
}

class CounterButtons extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Row(
      children: [
        ElevatedButton(
          onPressed: () {
            context.read<CounterBloc>().add(IncrementEvent());
          },
          child: Text('Increment'),
        ),
        ElevatedButton(
          onPressed: () {
            context.read<CounterBloc>().add(DecrementEvent());
          },
          child: Text('Decrement'),
        ),
      ],
    );
  }
}
```

**৩. `Equatable` ব্যবহার:**

```dart
class CounterState extends Equatable {
  final int count;
  final bool isLoading;
  final String? error;

  const CounterState({
    required this.count,
    this.isLoading = false,
    this.error,
  });

  CounterState copyWith({
    int? count,
    bool? isLoading,
    String? error,
  }) {
    return CounterState(
      count: count ?? this.count,
      isLoading: isLoading ?? this.isLoading,
      error: error ?? this.error,
    );
  }

  @override
  List<Object?> get props => [count, isLoading, error];
}
```

**৪. List Optimization:**

```dart
class OptimizedListWidget extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return BlocBuilder<TodoBloc, TodoState>(
      builder: (context, state) {
        return ListView.builder(
          itemCount: state.todos.length,
          itemBuilder: (context, index) {
            final todo = state.todos[index];
            return TodoItemWidget(todo: todo);
          },
        );
      },
    );
  }
}

class TodoItemWidget extends StatelessWidget {
  final Todo todo;

  const TodoItemWidget({required this.todo, Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return BlocBuilder<TodoBloc, TodoState>(
      buildWhen: (previous, current) {
        // শুধুমাত্র এই specific todo পরিবর্তন হলে rebuild হবে
        final previousTodo = previous.todos.firstWhere(
          (t) => t.id == todo.id,
          orElse: () => todo,
        );
        return previousTodo != todo;
      },
      builder: (context, state) {
        return ListTile(
          title: Text(todo.title),
          trailing: Checkbox(
            value: todo.isCompleted,
            onChanged: (value) {
              context.read<TodoBloc>().add(ToggleTodoEvent(todo.id));
            },
          ),
        );
      },
    );
  }
}
```

**৫. Memory Management:**

```dart
class OptimizedBloc extends Bloc<CounterEvent, CounterState> {
  StreamSubscription? _subscription;

  OptimizedBloc() : super(const CounterState(count: 0)) {
    on<IncrementEvent>(_onIncrement);
    on<DecrementEvent>(_onDecrement);
    on<StartTimerEvent>(_onStartTimer);
    on<StopTimerEvent>(_onStopTimer);
  }

  void _onIncrement(IncrementEvent event, Emitter<CounterState> emit) {
    emit(state.copyWith(count: state.count + 1));
  }

  void _onDecrement(DecrementEvent event, Emitter<CounterState> emit) {
    emit(state.copyWith(count: state.count - 1));
  }

  void _onStartTimer(StartTimerEvent event, Emitter<CounterState> emit) {
    _subscription?.cancel();
    _subscription = Stream.periodic(Duration(seconds: 1), (_) {
      add(IncrementEvent());
    }).listen((_) {});
  }

  void _onStopTimer(StopTimerEvent event, Emitter<CounterState> emit) {
    _subscription?.cancel();
    _subscription = null;
  }

  @override
  Future<void> close() {
    _subscription?.cancel();
    return super.close();
  }
}
```

**৬. Debouncing এবং Throttling:**

```dart
class SearchBloc extends Bloc<SearchEvent, SearchState> {
  Timer? _debounceTimer;

  SearchBloc() : super(const SearchState()) {
    on<SearchQueryChangedEvent>(_onSearchQueryChanged);
  }

  void _onSearchQueryChanged(
    SearchQueryChangedEvent event,
    Emitter<SearchState> emit,
  ) {
    _debounceTimer?.cancel();
    
    _debounceTimer = Timer(Duration(milliseconds: 500), () {
      add(PerformSearchEvent(event.query));
    });
  }

  @override
  Future<void> close() {
    _debounceTimer?.cancel();
    return super.close();
  }
}
```

**Performance Monitoring:**

```dart
class PerformanceBloc extends Bloc<CounterEvent, CounterState> {
  PerformanceBloc() : super(const CounterState(count: 0)) {
    on<IncrementEvent>(_onIncrement);
  }

  void _onIncrement(IncrementEvent event, Emitter<CounterState> emit) {
    final stopwatch = Stopwatch()..start();
    
    emit(state.copyWith(count: state.count + 1));
    
    stopwatch.stop();
    print('Increment took: ${stopwatch.elapsedMicroseconds} microseconds');
  }
}
```

**Pitfalls:**
1. `buildWhen` না ব্যবহার করলে অপ্রয়োজনীয় rebuild হয়
2. `Equatable` না ব্যবহার করলে performance খারাপ হয়
3. Widget splitting না করলে পুরো widget rebuild হয়
4. Memory leak হতে পারে যদি subscription cancel না করা হয়

**Interview Tips:** 
- Always use `buildWhen`/`listenWhen` for performance
- Use `Equatable` for state classes
- Split widgets for granular rebuilds
- Cancel subscriptions in `close()` method
- Monitor performance with `Stopwatch`

---

### প্রশ্ন ১৫: BLoC টেস্টিং বেস্ট প্র্যাকটিস?

**উত্তর (ডিটেইল):**

BLoC testing একটি গুরুত্বপূর্ণ বিষয় যা code quality এবং reliability নিশ্চিত করে। এখানে comprehensive testing strategy:

**১. Unit Testing BLoCs:**

```dart
import 'package:bloc_test/bloc_test.dart';
import 'package:flutter_test/flutter_test.dart';

void main() {
  group('CounterBloc', () {
    late CounterBloc counterBloc;

    setUp(() {
      counterBloc = CounterBloc();
    });

    tearDown(() {
      counterBloc.close();
    });

    test('initial state is 0', () {
      expect(counterBloc.state.count, equals(0));
    });

    blocTest<CounterBloc, CounterState>(
      'emits [1] when increment is added',
      build: () => CounterBloc(),
      act: (bloc) => bloc.add(IncrementEvent()),
      expect: () => [CounterState(count: 1)],
    );

    blocTest<CounterBloc, CounterState>(
      'emits [0] when decrement is added',
      build: () => CounterBloc(),
      act: (bloc) => bloc.add(DecrementEvent()),
      expect: () => [CounterState(count: -1)],
    );

    blocTest<CounterBloc, CounterState>(
      'emits [0] when reset is added',
      build: () => CounterBloc(),
      act: (bloc) => bloc.add(ResetEvent()),
      expect: () => [CounterState(count: 0)],
    );
  });
}
```

**২. Complex BLoC Testing:**

```dart
class AuthBloc extends Bloc<AuthEvent, AuthState> {
  final AuthRepository authRepository;

  AuthBloc({required this.authRepository}) : super(AuthStateInitial()) {
    on<LoginEvent>(_onLogin);
    on<LogoutEvent>(_onLogout);
  }

  Future<void> _onLogin(LoginEvent event, Emitter<AuthState> emit) async {
    emit(AuthStateLoading());
    
    try {
      final user = await authRepository.login(event.email, event.password);
      emit(AuthStateAuthenticated(user));
    } catch (error) {
      emit(AuthStateError(error.toString()));
    }
  }

  void _onLogout(LogoutEvent event, Emitter<AuthState> emit) {
    emit(AuthStateUnauthenticated());
  }
}

// Test with mocked repository
void main() {
  group('AuthBloc', () {
    late AuthBloc authBloc;
    late MockAuthRepository mockAuthRepository;

    setUp(() {
      mockAuthRepository = MockAuthRepository();
      authBloc = AuthBloc(authRepository: mockAuthRepository);
    });

    tearDown(() {
      authBloc.close();
    });

    blocTest<AuthBloc, AuthState>(
      'emits [AuthStateLoading, AuthStateAuthenticated] when login is successful',
      build: () {
        when(mockAuthRepository.login('test@example.com', 'password'))
            .thenAnswer((_) async => User(id: '1', name: 'Test User'));
        return authBloc;
      },
      act: (bloc) => bloc.add(LoginEvent('test@example.com', 'password')),
      expect: () => [
        AuthStateLoading(),
        AuthStateAuthenticated(User(id: '1', name: 'Test User')),
      ],
    );

    blocTest<AuthBloc, AuthState>(
      'emits [AuthStateLoading, AuthStateError] when login fails',
      build: () {
        when(mockAuthRepository.login('test@example.com', 'password'))
            .thenThrow(Exception('Invalid credentials'));
        return authBloc;
      },
      act: (bloc) => bloc.add(LoginEvent('test@example.com', 'password')),
      expect: () => [
        AuthStateLoading(),
        AuthStateError('Exception: Invalid credentials'),
      ],
    );

    blocTest<AuthBloc, AuthState>(
      'emits [AuthStateUnauthenticated] when logout is added',
      build: () => authBloc,
      act: (bloc) => bloc.add(LogoutEvent()),
      expect: () => [AuthStateUnauthenticated()],
    );
  });
}
```

**৩. Widget Testing:**

```dart
void main() {
  group('CounterWidget', () {
    testWidgets('displays initial count of 0', (WidgetTester tester) async {
      await tester.pumpWidget(
        MaterialApp(
          home: BlocProvider(
            create: (context) => CounterBloc(),
            child: CounterWidget(),
          ),
        ),
      );

      expect(find.text('Count: 0'), findsOneWidget);
    });

    testWidgets('increments count when increment button is pressed',
        (WidgetTester tester) async {
      await tester.pumpWidget(
        MaterialApp(
          home: BlocProvider(
            create: (context) => CounterBloc(),
            child: CounterWidget(),
          ),
        ),
      );

      await tester.tap(find.text('Increment'));
      await tester.pump();

      expect(find.text('Count: 1'), findsOneWidget);
    });

    testWidgets('decrements count when decrement button is pressed',
        (WidgetTester tester) async {
      await tester.pumpWidget(
        MaterialApp(
          home: BlocProvider(
            create: (context) => CounterBloc(),
            child: CounterWidget(),
          ),
        ),
      );

      await tester.tap(find.text('Decrement'));
      await tester.pump();

      expect(find.text('Count: -1'), findsOneWidget);
    });
  });
}
```

**৪. Integration Testing:**

```dart
void main() {
  group('Auth Integration Test', () {
    testWidgets('complete login flow', (WidgetTester tester) async {
      await tester.pumpWidget(
        MaterialApp(
          home: BlocProvider(
            create: (context) => AuthBloc(
              authRepository: MockAuthRepository(),
            ),
            child: LoginWidget(),
          ),
        ),
      );

      // Enter email
      await tester.enterText(
        find.byKey(Key('email_field')),
        'test@example.com',
      );

      // Enter password
      await tester.enterText(
        find.byKey(Key('password_field')),
        'password',
      );

      // Tap login button
      await tester.tap(find.text('Login'));
      await tester.pump();

      // Verify navigation to home
      expect(find.text('Welcome'), findsOneWidget);
    });
  });
}
```

**৫. Testing with Dependencies:**

```dart
class UserBloc extends Bloc<UserEvent, UserState> {
  final UserRepository userRepository;
  final AnalyticsService analyticsService;

  UserBloc({
    required this.userRepository,
    required this.analyticsService,
  }) : super(UserStateInitial()) {
    on<LoadUserEvent>(_onLoadUser);
    on<UpdateUserEvent>(_onUpdateUser);
  }

  Future<void> _onLoadUser(LoadUserEvent event, Emitter<UserState> emit) async {
    emit(UserStateLoading());
    
    try {
      final user = await userRepository.getUser(event.userId);
      analyticsService.trackUserView(user.id);
      emit(UserStateLoaded(user));
    } catch (error) {
      emit(UserStateError(error.toString()));
    }
  }

  Future<void> _onUpdateUser(UpdateUserEvent event, Emitter<UserState> emit) async {
    if (state is UserStateLoaded) {
      final currentState = state as UserStateLoaded;
      emit(UserStateLoading());
      
      try {
        final updatedUser = await userRepository.updateUser(event.user);
        analyticsService.trackUserUpdate(updatedUser.id);
        emit(UserStateLoaded(updatedUser));
      } catch (error) {
        emit(UserStateError(error.toString()));
      }
    }
  }
}

// Test with multiple dependencies
void main() {
  group('UserBloc', () {
    late UserBloc userBloc;
    late MockUserRepository mockUserRepository;
    late MockAnalyticsService mockAnalyticsService;

    setUp(() {
      mockUserRepository = MockUserRepository();
      mockAnalyticsService = MockAnalyticsService();
      userBloc = UserBloc(
        userRepository: mockUserRepository,
        analyticsService: mockAnalyticsService,
      );
    });

    tearDown(() {
      userBloc.close();
    });

    blocTest<UserBloc, UserState>(
      'emits [UserStateLoading, UserStateLoaded] when user is loaded successfully',
      build: () {
        when(mockUserRepository.getUser('1'))
            .thenAnswer((_) async => User(id: '1', name: 'Test User'));
        return userBloc;
      },
      act: (bloc) => bloc.add(LoadUserEvent('1')),
      expect: () => [
        UserStateLoading(),
        UserStateLoaded(User(id: '1', name: 'Test User')),
      ],
      verify: (_) {
        verify(mockAnalyticsService.trackUserView('1')).called(1);
      },
    );
  });
}
```

**৬. Testing Async Operations:**

```dart
class TimerBloc extends Bloc<TimerEvent, TimerState> {
  Timer? _timer;

  TimerBloc() : super(TimerStateInitial()) {
    on<StartTimerEvent>(_onStartTimer);
    on<StopTimerEvent>(_onStopTimer);
  }

  void _onStartTimer(StartTimerEvent event, Emitter<TimerState> emit) {
    _timer?.cancel();
    emit(TimerStateRunning(0));
    
    _timer = Timer.periodic(Duration(seconds: 1), (timer) {
      final currentState = state;
      if (currentState is TimerStateRunning) {
        emit(TimerStateRunning(currentState.count + 1));
      }
    });
  }

  void _onStopTimer(StopTimerEvent event, Emitter<TimerState> emit) {
    _timer?.cancel();
    emit(TimerStateStopped());
  }

  @override
  Future<void> close() {
    _timer?.cancel();
    return super.close();
  }
}

// Test with fakeAsync
void main() {
  group('TimerBloc', () {
    late TimerBloc timerBloc;

    setUp(() {
      timerBloc = TimerBloc();
    });

    tearDown(() {
      timerBloc.close();
    });

    test('emits [TimerStateRunning(0), TimerStateRunning(1)] when timer starts',
        () async {
      expect(timerBloc.state, TimerStateInitial());

      timerBloc.add(StartTimerEvent());
      await expectLater(
        timerBloc.stream,
        emitsInOrder([
          TimerStateRunning(0),
          TimerStateRunning(1),
        ]),
      );
    });

    test('stops timer when StopTimerEvent is added', () async {
      timerBloc.add(StartTimerEvent());
      await expectLater(
        timerBloc.stream,
        emitsInOrder([
          TimerStateRunning(0),
          TimerStateStopped(),
        ]),
      );

      timerBloc.add(StopTimerEvent());
    });
  });
}
```

**৭. Testing Error Handling:**

```dart
class NetworkBloc extends Bloc<NetworkEvent, NetworkState> {
  final NetworkService networkService;

  NetworkBloc({required this.networkService}) : super(NetworkStateInitial()) {
    on<FetchDataEvent>(_onFetchData);
    on<RetryEvent>(_onRetry);
  }

  Future<void> _onFetchData(FetchDataEvent event, Emitter<NetworkState> emit) async {
    emit(NetworkStateLoading());
    
    try {
      final data = await networkService.fetchData();
      emit(NetworkStateLoaded(data));
    } catch (error) {
      emit(NetworkStateError(error.toString()));
    }
  }

  Future<void> _onRetry(RetryEvent event, Emitter<NetworkState> emit) async {
    add(FetchDataEvent());
  }
}

// Test error scenarios
void main() {
  group('NetworkBloc', () {
    late NetworkBloc networkBloc;
    late MockNetworkService mockNetworkService;

    setUp(() {
      mockNetworkService = MockNetworkService();
      networkBloc = NetworkBloc(networkService: mockNetworkService);
    });

    tearDown(() {
      networkBloc.close();
    });

    blocTest<NetworkBloc, NetworkState>(
      'emits [NetworkStateLoading, NetworkStateError] when network fails',
      build: () {
        when(mockNetworkService.fetchData())
            .thenThrow(NetworkException('No internet connection'));
        return networkBloc;
      },
      act: (bloc) => bloc.add(FetchDataEvent()),
      expect: () => [
        NetworkStateLoading(),
        NetworkStateError('NetworkException: No internet connection'),
      ],
    );

    blocTest<NetworkBloc, NetworkState>(
      'retries when RetryEvent is added',
      build: () {
        when(mockNetworkService.fetchData())
            .thenThrow(NetworkException('No internet connection'))
            .thenAnswer((_) async => 'Success data');
        return networkBloc;
      },
      act: (bloc) {
        bloc.add(FetchDataEvent());
        bloc.add(RetryEvent());
      },
      expect: () => [
        NetworkStateLoading(),
        NetworkStateError('NetworkException: No internet connection'),
        NetworkStateLoading(),
        NetworkStateLoaded('Success data'),
      ],
    );
  });
}
```

**Pitfalls:**
1. `bloc_test` package না ব্যবহার করলে boilerplate code বেশি হয়
2. Mock dependencies না করলে test unreliable হয়
3. Async operations test না করলে race condition হতে পারে
4. Error scenarios test না করলে edge cases miss হয়

**Interview Tips:** 
- Always use `bloc_test` package for BLoC testing
- Mock external dependencies
- Test both success and error scenarios
- Use `fakeAsync` for timer-based tests
- Test widget integration with BLoCs
- Verify side effects with mocks


