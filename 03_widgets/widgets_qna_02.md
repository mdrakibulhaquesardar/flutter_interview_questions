# Flutter Widgets - প্রশ্নোত্তর সেট ২

## Layout এবং Styling Widgets

### প্রশ্ন ৬: Row, Column, এবং Stack-এর মধ্যে পার্থক্য কী এবং কখন কোনটা ব্যবহার করবেন?

**উত্তর (ডিটেইল):**

- **Row**: Widgets-কে horizontally (বাম থেকে ডান) arrange করে। Main axis হলো horizontal, cross axis হলো vertical।

- **Column**: Widgets-কে vertically (উপর থেকে নিচ) arrange করে। Main axis হলো vertical, cross axis হলো horizontal।

- **Stack**: Widgets-কে overlap করে arrange করে, যেখানে প্রথম child সবচেয়ে নিচে থাকে এবং শেষ child সবচেয়ে উপরে থাকে।

**উদাহরণ:**

```dart
class LayoutDemo extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: const Text('Layout Demo')),
      body: Padding(
        padding: const EdgeInsets.all(16.0),
        child: Column(
          children: [
            // Row example - horizontal arrangement
            Container(
              padding: const EdgeInsets.all(8),
              decoration: BoxDecoration(
                border: Border.all(color: Colors.blue),
                borderRadius: BorderRadius.circular(8),
              ),
              child: Row(
                mainAxisAlignment: MainAxisAlignment.spaceEvenly,
                crossAxisAlignment: CrossAxisAlignment.center,
                children: [
                  Icon(Icons.star, color: Colors.amber),
                  Text('Rating: 4.5'),
                  ElevatedButton(
                    onPressed: () {},
                    child: Text('Rate'),
                  ),
                ],
              ),
            ),
            
            const SizedBox(height: 20),
            
            // Column example - vertical arrangement
            Container(
              padding: const EdgeInsets.all(8),
              decoration: BoxDecoration(
                border: Border.all(color: Colors.green),
                borderRadius: BorderRadius.circular(8),
              ),
              child: Column(
                mainAxisAlignment: MainAxisAlignment.start,
                crossAxisAlignment: CrossAxisAlignment.stretch,
                children: [
                  Text(
                    'User Profile',
                    style: Theme.of(context).textTheme.headlineSmall,
                    textAlign: TextAlign.center,
                  ),
                  const SizedBox(height: 8),
                  CircleAvatar(
                    radius: 30,
                    child: Icon(Icons.person, size: 30),
                  ),
                  const SizedBox(height: 8),
                  Text('John Doe'),
                  Text('Software Developer'),
                  ElevatedButton(
                    onPressed: () {},
                    child: Text('Edit Profile'),
                  ),
                ],
              ),
            ),
            
            const SizedBox(height: 20),
            
            // Stack example - overlapping widgets
            Container(
              height: 200,
              decoration: BoxDecoration(
                border: Border.all(color: Colors.red),
                borderRadius: BorderRadius.circular(8),
              ),
              child: Stack(
                alignment: Alignment.center,
                children: [
                  // Background image
                  Container(
                    width: double.infinity,
                    height: double.infinity,
                    decoration: BoxDecoration(
                      color: Colors.grey[300],
                      borderRadius: BorderRadius.circular(8),
                    ),
                    child: Icon(
                      Icons.image,
                      size: 80,
                      color: Colors.grey[600],
                    ),
                  ),
                  
                  // Overlay text
                  Container(
                    padding: const EdgeInsets.all(16),
                    decoration: BoxDecoration(
                      color: Colors.black54,
                      borderRadius: BorderRadius.circular(8),
                    ),
                    child: Text(
                      'Overlay Text',
                      style: TextStyle(
                        color: Colors.white,
                        fontSize: 18,
                        fontWeight: FontWeight.bold,
                      ),
                    ),
                  ),
                  
                  // Positioned element
                  Positioned(
                    top: 16,
                    right: 16,
                    child: Container(
                      padding: const EdgeInsets.all(8),
                      decoration: BoxDecoration(
                        color: Colors.red,
                        shape: BoxShape.circle,
                      ),
                      child: Text(
                        'NEW',
                        style: TextStyle(
                          color: Colors.white,
                          fontSize: 12,
                          fontWeight: FontWeight.bold,
                        ),
                      ),
                    ),
                  ),
                ],
              ),
            ),
          ],
        ),
      ),
    );
  }
}
```

**কখন কোনটা ব্যবহার করবেন:**
- **Row**: Horizontal list, toolbar, button group
- **Column**: Form, profile, vertical list
- **Stack**: Overlay, badge, floating elements

**Interview Tips:**
- MainAxisAlignment vs CrossAxisAlignment বুঝুন
- Stack-এ Positioned widget ব্যবহার করে precise positioning করুন
- Responsive design-এ Expanded/Flexible ব্যবহার করুন

---

### প্রশ্ন ৭: Container, SizedBox, এবং Padding-এর মধ্যে পার্থক্য কী?

**উত্তর (ডিটেইল):**

- **Container**: সবচেয়ে flexible widget যা size, decoration, alignment, margin, padding সবকিছু support করে। এটা heavy widget কারণ অনেক properties handle করে।

- **SizedBox**: Simple size constraint widget যা শুধুমাত্র width, height, এবং child support করে। এটা lightweight এবং efficient।

- **Padding**: শুধুমাত্র padding add করে। এটা SizedBox-এর চেয়েও lightweight।

**উদাহরণ:**

```dart
class ContainerVsSizedBox extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: const Text('Container vs SizedBox vs Padding')),
      body: Padding(
        padding: const EdgeInsets.all(16.0),
        child: Column(
          children: [
            // Container - full featured
            Container(
              width: 200,
              height: 100,
              margin: const EdgeInsets.all(8),
              padding: const EdgeInsets.all(16),
              decoration: BoxDecoration(
                color: Colors.blue,
                borderRadius: BorderRadius.circular(12),
                boxShadow: [
                  BoxShadow(
                    color: Colors.black26,
                    blurRadius: 8,
                    offset: const Offset(0, 4),
                  ),
                ],
              ),
              alignment: Alignment.center,
              child: Text(
                'Container',
                style: TextStyle(
                  color: Colors.white,
                  fontSize: 18,
                  fontWeight: FontWeight.bold,
                ),
              ),
            ),
            
            const SizedBox(height: 20),
            
            // SizedBox - simple size constraint
            SizedBox(
              width: 200,
              height: 100,
              child: ElevatedButton(
                onPressed: () {},
                style: ElevatedButton.styleFrom(
                  backgroundColor: Colors.green,
                  shape: RoundedRectangleBorder(
                    borderRadius: BorderRadius.circular(12),
                  ),
                ),
                child: Text(
                  'SizedBox',
                  style: TextStyle(
                    color: Colors.white,
                    fontSize: 18,
                    fontWeight: FontWeight.bold,
                  ),
                ),
              ),
            ),
            
            const SizedBox(height: 20),
            
            // Padding - just spacing
            Container(
              decoration: BoxDecoration(
                color: Colors.orange,
                borderRadius: BorderRadius.circular(12),
              ),
              child: Padding(
                padding: const EdgeInsets.all(16),
                child: Text(
                  'Padding Only',
                  style: TextStyle(
                    color: Colors.white,
                    fontSize: 18,
                    fontWeight: FontWeight.bold,
                  ),
                ),
              ),
            ),
            
            const SizedBox(height: 20),
            
            // Performance comparison
            Row(
              mainAxisAlignment: MainAxisAlignment.spaceEvenly,
              children: [
                // Container with decoration
                Container(
                  width: 80,
                  height: 80,
                  decoration: BoxDecoration(
                    color: Colors.purple,
                    shape: BoxShape.circle,
                  ),
                  child: Icon(Icons.favorite, color: Colors.white),
                ),
                
                // SizedBox with Container child
                SizedBox(
                  width: 80,
                  height: 80,
                  child: Container(
                    decoration: BoxDecoration(
                      color: Colors.purple,
                      shape: BoxShape.circle,
                    ),
                    child: Icon(Icons.favorite, color: Colors.white),
                  ),
                ),
                
                // Padding with Container
                Padding(
                  padding: const EdgeInsets.all(8),
                  child: Container(
                    width: 64,
                    height: 64,
                    decoration: BoxDecoration(
                      color: Colors.purple,
                      shape: BoxShape.circle,
                    ),
                    child: Icon(Icons.favorite, color: Colors.white),
                  ),
                ),
              ],
            ),
          ],
        ),
      ),
    );
  }
}
```

**Performance Tips:**
- Simple size constraint → SizedBox
- Just spacing → Padding
- Complex styling → Container
- Avoid nested Container + SizedBox

**Interview Tips:**
- Container সবচেয়ে expensive
- SizedBox সবচেয়ে efficient
- Widget tree depth minimize করুন

---

### প্রশ্ন ৮: Expanded, Flexible, এবং Spacer-এর মধ্যে পার্থক্য কী?

**উত্তর (ডিটেইল):**

- **Expanded**: Child-কে available space-এর সবটুকু দেয়। এটা `Flexible(fit: FlexFit.tight)` এর equivalent।

- **Flexible**: Child-কে flexible size দেয়। `fit` property দিয়ে control করা যায়।

- **Spacer**: Available space-এ flexible space add করে। এটা `Expanded(child: SizedBox())` এর equivalent।

**উদাহরণ:**

```dart
class FlexDemo extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: const Text('Flex Widgets Demo')),
      body: Padding(
        padding: const EdgeInsets.all(16.0),
        child: Column(
          children: [
            // Expanded example
            Container(
              decoration: BoxDecoration(
                border: Border.all(color: Colors.blue),
                borderRadius: BorderRadius.circular(8),
              ),
              child: Row(
                children: [
                  Container(
                    width: 60,
                    height: 60,
                    color: Colors.red,
                    child: Icon(Icons.star, color: Colors.white),
                  ),
                  Expanded( // Takes remaining space
                    child: Container(
                      height: 60,
                      color: Colors.green,
                      child: Center(
                        child: Text(
                          'Expanded - takes all remaining space',
                          style: TextStyle(color: Colors.white),
                        ),
                      ),
                    ),
                  ),
                ],
              ),
            ),
            
            const SizedBox(height: 20),
            
            // Flexible example
            Container(
              decoration: BoxDecoration(
                border: Border.all(color: Colors.green),
                borderRadius: BorderRadius.circular(8),
              ),
              child: Row(
                children: [
                  Container(
                    width: 60,
                    height: 60,
                    color: Colors.red,
                    child: Icon(Icons.favorite, color: Colors.white),
                  ),
                  Flexible( // Flexible size
                    fit: FlexFit.loose, // Default - child size অনুযায়ী
                    child: Container(
                      height: 60,
                      color: Colors.blue,
                      child: Center(
                        child: Text(
                          'Flexible - flexible size',
                          style: TextStyle(color: Colors.white),
                        ),
                      ),
                    ),
                  ),
                  Flexible(
                    fit: FlexFit.tight, // Same as Expanded
                    child: Container(
                      height: 60,
                      color: Colors.orange,
                      child: Center(
                        child: Text(
                          'Flexible tight',
                          style: TextStyle(color: Colors.white),
                        ),
                      ),
                    ),
                  ),
                ],
              ),
            ),
            
            const SizedBox(height: 20),
            
            // Spacer example
            Container(
              decoration: BoxDecoration(
                border: Border.all(color: Colors.orange),
                borderRadius: BorderRadius.circular(8),
              ),
              child: Row(
                children: [
                  Container(
                    width: 60,
                    height: 60,
                    color: Colors.red,
                    child: Icon(Icons.home, color: Colors.white),
                  ),
                  Spacer(flex: 1), // Flexible space
                  Container(
                    width: 60,
                    height: 60,
                    color: Colors.green,
                    child: Icon(Icons.settings, color: Colors.white),
                  ),
                  Spacer(flex: 2), // Double flexible space
                  Container(
                    width: 60,
                    height: 60,
                    color: Colors.blue,
                    child: Icon(Icons.person, color: Colors.white),
                  ),
                ],
              ),
            ),
            
            const SizedBox(height: 20),
            
            // Practical example - responsive layout
            Container(
              decoration: BoxDecoration(
                border: Border.all(color: Colors.purple),
                borderRadius: BorderRadius.circular(8),
              ),
              child: Row(
                children: [
                  // Fixed width sidebar
                  Container(
                    width: 80,
                    height: 120,
                    color: Colors.grey[300],
                    child: Column(
                      mainAxisAlignment: MainAxisAlignment.center,
                      children: [
                        Icon(Icons.menu),
                        Text('Menu'),
                      ],
                    ),
                  ),
                  
                  // Flexible content area
                  Expanded(
                    child: Container(
                      height: 120,
                      color: Colors.blue[100],
                      child: Center(
                        child: Text(
                          'Main Content Area\n(Takes remaining space)',
                          textAlign: TextAlign.center,
                        ),
                      ),
                    ),
                  ),
                  
                  // Fixed width actions
                  Container(
                    width: 100,
                    height: 120,
                    color: Colors.grey[300],
                    child: Column(
                      mainAxisAlignment: MainAxisAlignment.center,
                      children: [
                        Icon(Icons.search),
                        Text('Search'),
                        Icon(Icons.notifications),
                        Text('Notifications'),
                      ],
                    ),
                  ),
                ],
              ),
            ),
          ],
        ),
      ),
    );
  }
}
```

**কখন কোনটা ব্যবহার করবেন:**
- **Expanded**: Child-কে সব available space দিতে চাইলে
- **Flexible**: Child-এর size flexible রাখতে চাইলে
- **Spacer**: Widgets-এর মধ্যে space add করতে চাইলে

**Interview Tips:**
- Expanded = Flexible(fit: FlexFit.tight)
- Spacer = Expanded(child: SizedBox())
- flex property দিয়ে space distribution control করুন

---

### প্রশ্ন ৯: ListView, GridView, এবং CustomScrollView-এর মধ্যে পার্থক্য কী?

**উত্তর (ডিটেইল):**

- **ListView**: Linear list of widgets. Vertical বা horizontal scroll support করে।

- **GridView**: 2D grid layout. Rows এবং columns-এ widgets arrange করে।

- **CustomScrollView**: Multiple scrollable widgets combine করে। Sliver widgets ব্যবহার করে।

**উদাহরণ:**

```dart
class ScrollViewDemo extends StatelessWidget {
  final List<String> items = List.generate(50, (index) => 'Item ${index + 1}');
  
  @override
  Widget build(BuildContext context) {
    return DefaultTabController(
      length: 3,
      child: Scaffold(
        appBar: AppBar(
          title: const Text('Scroll Views Demo'),
          bottom: const TabBar(
            tabs: [
              Tab(text: 'ListView'),
              Tab(text: 'GridView'),
              Tab(text: 'CustomScrollView'),
            ],
          ),
        ),
        body: TabBarView(
          children: [
            // ListView
            ListView.builder(
              itemCount: items.length,
              itemBuilder: (context, index) {
                return ListTile(
                  leading: CircleAvatar(
                    backgroundColor: Colors.primaries[index % Colors.primaries.length],
                    child: Text('${index + 1}'),
                  ),
                  title: Text(items[index]),
                  subtitle: Text('Subtitle for ${items[index]}'),
                  trailing: Icon(Icons.arrow_forward_ios),
                  onTap: () {
                    ScaffoldMessenger.of(context).showSnackBar(
                      SnackBar(content: Text('Tapped ${items[index]}')),
                    );
                  },
                );
              },
            ),
            
            // GridView
            GridView.builder(
              padding: const EdgeInsets.all(16),
              gridDelegate: const SliverGridDelegateWithFixedCrossAxisCount(
                crossAxisCount: 2,
                crossAxisSpacing: 16,
                mainAxisSpacing: 16,
                childAspectRatio: 1.2,
              ),
              itemCount: items.length,
              itemBuilder: (context, index) {
                return Card(
                  elevation: 4,
                  child: Column(
                    mainAxisAlignment: MainAxisAlignment.center,
                    children: [
                      Icon(
                        Icons.star,
                        size: 48,
                        color: Colors.primaries[index % Colors.primaries.length],
                      ),
                      const SizedBox(height: 8),
                      Text(
                        items[index],
                        style: Theme.of(context).textTheme.titleMedium,
                        textAlign: TextAlign.center,
                      ),
                      const SizedBox(height: 4),
                      Text(
                        'Grid Item',
                        style: Theme.of(context).textTheme.bodySmall,
                        textAlign: TextAlign.center,
                      ),
                    ],
                  ),
                );
              },
            ),
            
            // CustomScrollView
            CustomScrollView(
              slivers: [
                // SliverAppBar
                SliverAppBar(
                  expandedHeight: 200,
                  floating: false,
                  pinned: true,
                  flexibleSpace: FlexibleSpaceBar(
                    title: const Text('Custom Scroll View'),
                    background: Container(
                      decoration: BoxDecoration(
                        gradient: LinearGradient(
                          begin: Alignment.topLeft,
                          end: Alignment.bottomRight,
                          colors: [
                            Colors.blue,
                            Colors.purple,
                          ],
                        ),
                      ),
                      child: const Center(
                        child: Icon(
                          Icons.flutter_dash,
                          size: 100,
                          color: Colors.white,
                        ),
                      ),
                    ),
                  ),
                ),
                
                // SliverList
                SliverList(
                  delegate: SliverChildBuilderDelegate(
                    (context, index) {
                      if (index >= items.length) return null;
                      return ListTile(
                        leading: CircleAvatar(
                          backgroundColor: Colors.primaries[index % Colors.primaries.length],
                          child: Text('${index + 1}'),
                        ),
                        title: Text(items[index]),
                        subtitle: Text('Sliver item ${index + 1}'),
                      );
                    },
                    childCount: items.length,
                  ),
                ),
                
                // SliverGrid
                SliverGrid(
                  gridDelegate: const SliverGridDelegateWithFixedCrossAxisCount(
                    crossAxisCount: 3,
                    childAspectRatio: 1.0,
                  ),
                  delegate: SliverChildBuilderDelegate(
                    (context, index) {
                      if (index >= 20) return null;
                      return Card(
                        child: Center(
                          child: Text(
                            'Grid ${index + 1}',
                            style: Theme.of(context).textTheme.bodySmall,
                          ),
                        ),
                      );
                    },
                    childCount: 20,
                  ),
                ),
                
                // SliverToBoxAdapter
                SliverToBoxAdapter(
                  child: Container(
                    margin: const EdgeInsets.all(16),
                    padding: const EdgeInsets.all(16),
                    decoration: BoxDecoration(
                      color: Colors.orange[100],
                      borderRadius: BorderRadius.circular(8),
                    ),
                    child: const Text(
                      'This is a SliverToBoxAdapter widget that can contain any widget.',
                      style: TextStyle(fontSize: 16),
                    ),
                  ),
                ),
              ],
            ),
          ],
        ),
      ),
    );
  }
}
```

**Performance Tips:**
- ListView.builder/GridView.builder lazy loading support করে
- CustomScrollView complex layouts-এর জন্য
- Sliver widgets memory efficient

**Interview Tips:**
- ListView → Linear data
- GridView → 2D data
- CustomScrollView → Complex scrollable layouts

---

### প্রশ্ন ১০: Form Widgets এবং Validation কীভাবে implement করবেন?

**উত্তর (ডিটেইল):**

- **Form Widgets**: User input collect করার জন্য ব্যবহৃত widgets (TextFormField, Checkbox, Radio, etc.)
- **Validation**: User input-এর correctness check করা

**উদাহরণ:**

```dart
class FormDemo extends StatefulWidget {
  @override
  State<FormDemo> createState() => _FormDemoState();
}

class _FormDemoState extends State<FormDemo> {
  final _formKey = GlobalKey<FormState>();
  final _nameController = TextEditingController();
  final _emailController = TextEditingController();
  final _passwordController = TextEditingController();
  
  String? _selectedGender;
  bool _agreeToTerms = false;
  List<String> _selectedInterests = [];
  
  final List<String> _interests = [
    'Flutter', 'Dart', 'Mobile Development',
    'Web Development', 'UI/UX', 'Testing'
  ];
  
  @override
  void dispose() {
    _nameController.dispose();
    _emailController.dispose();
    _passwordController.dispose();
    super.dispose();
  }
  
  void _submitForm() {
    if (_formKey.currentState!.validate()) {
      // Form is valid
      final formData = {
        'name': _nameController.text,
        'email': _emailController.text,
        'password': _passwordController.text,
        'gender': _selectedGender,
        'agreeToTerms': _agreeToTerms,
        'interests': _selectedInterests,
      };
      
      print('Form Data: $formData');
      
      ScaffoldMessenger.of(context).showSnackBar(
        SnackBar(
          content: Text('Form submitted successfully!'),
          backgroundColor: Colors.green,
        ),
      );
      
      // Reset form
      _formKey.currentState!.reset();
      setState(() {
        _selectedGender = null;
        _agreeToTerms = false;
        _selectedInterests.clear();
      });
    }
  }
  
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: const Text('Form Demo')),
      body: SingleChildScrollView(
        padding: const EdgeInsets.all(16.0),
        child: Form(
          key: _formKey,
          child: Column(
            crossAxisAlignment: CrossAxisAlignment.stretch,
            children: [
              // Name field
              TextFormField(
                controller: _nameController,
                decoration: const InputDecoration(
                  labelText: 'Full Name',
                  hintText: 'Enter your full name',
                  prefixIcon: Icon(Icons.person),
                  border: OutlineInputBorder(),
                ),
                validator: (value) {
                  if (value == null || value.isEmpty) {
                    return 'Please enter your name';
                  }
                  if (value.length < 2) {
                    return 'Name must be at least 2 characters';
                  }
                  if (!RegExp(r'^[a-zA-Z\s]+$').hasMatch(value)) {
                    return 'Name can only contain letters and spaces';
                  }
                  return null;
                },
                textInputAction: TextInputAction.next,
              ),
              
              const SizedBox(height: 16),
              
              // Email field
              TextFormField(
                controller: _emailController,
                decoration: const InputDecoration(
                  labelText: 'Email',
                  hintText: 'Enter your email',
                  prefixIcon: Icon(Icons.email),
                  border: OutlineInputBorder(),
                ),
                keyboardType: TextInputType.emailAddress,
                validator: (value) {
                  if (value == null || value.isEmpty) {
                    return 'Please enter your email';
                  }
                  if (!RegExp(r'^[\w-\.]+@([\w-]+\.)+[\w-]{2,4}$').hasMatch(value)) {
                    return 'Please enter a valid email';
                  }
                  return null;
                },
                textInputAction: TextInputAction.next,
              ),
              
              const SizedBox(height: 16),
              
              // Password field
              TextFormField(
                controller: _passwordController,
                decoration: const InputDecoration(
                  labelText: 'Password',
                  hintText: 'Enter your password',
                  prefixIcon: Icon(Icons.lock),
                  border: OutlineInputBorder(),
                ),
                obscureText: true,
                validator: (value) {
                  if (value == null || value.isEmpty) {
                    return 'Please enter your password';
                  }
                  if (value.length < 6) {
                    return 'Password must be at least 6 characters';
                  }
                  if (!RegExp(r'^(?=.*[a-z])(?=.*[A-Z])(?=.*\d)').hasMatch(value)) {
                    return 'Password must contain uppercase, lowercase, and number';
                  }
                  return null;
                },
              ),
              
              const SizedBox(height: 16),
              
              // Gender selection
              Text(
                'Gender',
                style: Theme.of(context).textTheme.titleMedium,
              ),
              Row(
                children: [
                  Expanded(
                    child: RadioListTile<String>(
                      title: const Text('Male'),
                      value: 'Male',
                      groupValue: _selectedGender,
                      onChanged: (value) {
                        setState(() {
                          _selectedGender = value;
                        });
                      },
                    ),
                  ),
                  Expanded(
                    child: RadioListTile<String>(
                      title: const Text('Female'),
                      value: 'Female',
                      groupValue: _selectedGender,
                      onChanged: (value) {
                        setState(() {
                          _selectedGender = value;
                        });
                      },
                    ),
                  ),
                ],
              ),
              
              const SizedBox(height: 16),
              
              // Interests selection
              Text(
                'Interests',
                style: Theme.of(context).textTheme.titleMedium,
              ),
              Wrap(
                spacing: 8,
                children: _interests.map((interest) {
                  final isSelected = _selectedInterests.contains(interest);
                  return FilterChip(
                    label: Text(interest),
                    selected: isSelected,
                    onSelected: (selected) {
                      setState(() {
                        if (selected) {
                          _selectedInterests.add(interest);
                        } else {
                          _selectedInterests.remove(interest);
                        }
                      });
                    },
                  );
                }).toList(),
              ),
              
              const SizedBox(height: 16),
              
              // Terms agreement
              CheckboxListTile(
                title: const Text('I agree to the terms and conditions'),
                value: _agreeToTerms,
                onChanged: (value) {
                  setState(() {
                    _agreeToTerms = value ?? false;
                  });
                },
                controlAffinity: ListTileControlAffinity.leading,
              ),
              
              const SizedBox(height: 24),
              
              // Submit button
              ElevatedButton(
                onPressed: _submitForm,
                style: ElevatedButton.styleFrom(
                  padding: const EdgeInsets.symmetric(vertical: 16),
                ),
                child: const Text(
                  'Submit Form',
                  style: TextStyle(fontSize: 18),
                ),
              ),
            ],
          ),
        ),
      ),
    );
  }
}
```

**Validation Best Practices:**
- Real-time validation vs submit-time validation
- Clear error messages
- Input formatting (phone, credit card)
- Server-side validation

**Interview Tips:**
- FormKey global scope-এ রাখুন
- Validation methods reusable করুন
- Input decoration consistent রাখুন
- Error handling implement করুন
