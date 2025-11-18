---
name: mobile-development
description: Build native iOS and Android applications, or use cross-platform frameworks like React Native and Flutter. Use when developing mobile apps or working with device-specific features.
---

# Mobile Development

## Quick Start

### React Native with TypeScript:

```typescript
import React, { useState } from 'react';
import {
  View,
  Text,
  TouchableOpacity,
  StyleSheet,
  FlatList,
} from 'react-native';

interface Item {
  id: string;
  title: string;
}

export default function App() {
  const [items, setItems] = useState<Item[]>([]);

  const addItem = () => {
    setItems([
      ...items,
      { id: Date.now().toString(), title: `Item ${items.length + 1}` }
    ]);
  };

  return (
    <View style={styles.container}>
      <Text style={styles.title}>My App</Text>
      <TouchableOpacity style={styles.button} onPress={addItem}>
        <Text style={styles.buttonText}>Add Item</Text>
      </TouchableOpacity>
      <FlatList
        data={items}
        keyExtractor={item => item.id}
        renderItem={({ item }) => (
          <Text style={styles.item}>{item.title}</Text>
        )}
      />
    </View>
  );
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    padding: 20,
    backgroundColor: '#fff',
  },
  title: {
    fontSize: 24,
    fontWeight: 'bold',
    marginBottom: 20,
  },
  button: {
    backgroundColor: '#007AFF',
    padding: 12,
    borderRadius: 8,
    alignItems: 'center',
  },
  buttonText: {
    color: '#fff',
    fontSize: 16,
    fontWeight: '600',
  },
  item: {
    paddingVertical: 10,
    fontSize: 16,
    borderBottomWidth: 1,
    borderBottomColor: '#eee',
  },
});
```

### Swift for iOS:

```swift
import SwiftUI

struct ContentView: View {
    @State private var count = 0

    var body: some View {
        VStack(spacing: 20) {
            Text("Counter")
                .font(.title)
                .fontWeight(.bold)

            Text("\(count)")
                .font(.system(size: 48))
                .fontWeight(.bold)

            HStack(spacing: 20) {
                Button(action: { count -= 1 }) {
                    Text("Decrease")
                        .frame(maxWidth: .infinity)
                        .padding()
                        .background(Color.red)
                        .foregroundColor(.white)
                        .cornerRadius(8)
                }

                Button(action: { count += 1 }) {
                    Text("Increase")
                        .frame(maxWidth: .infinity)
                        .padding()
                        .background(Color.blue)
                        .foregroundColor(.white)
                        .cornerRadius(8)
                }
            }
        }
        .padding()
    }
}
```

### Flutter with Dart:

```dart
import 'package:flutter/material.dart';

void main() {
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Flutter Demo',
      theme: ThemeData(primarySwatch: Colors.blue),
      home: const MyHomePage(),
    );
  }
}

class MyHomePage extends StatefulWidget {
  const MyHomePage({Key? key}) : super(key: key);

  @override
  State<MyHomePage> createState() => _MyHomePageState();
}

class _MyHomePageState extends State<MyHomePage> {
  int _counter = 0;

  void _incrementCounter() {
    setState(() {
      _counter++;
    });
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: const Text('Flutter Counter')),
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            const Text('You have pushed the button this many times:'),
            Text('$_counter', style: Theme.of(context).textTheme.headline4),
          ],
        ),
      ),
      floatingActionButton: FloatingActionButton(
        onPressed: _incrementCounter,
        child: const Icon(Icons.add),
      ),
    );
  }
}
```

## Core Concepts

**Mobile UI Patterns**
- Navigation patterns (tabs, stacks, drawers)
- List and scroll performance
- Touch handling and gestures
- Responsive layouts
- Platform design guidelines (iOS vs Android)

**Native Development**
- iOS: SwiftUI for UI, Core Data for persistence
- Android: Jetpack Compose for UI, Room for persistence
- Platform-specific APIs (camera, location, sensors)
- Performance optimization for mobile

**Cross-Platform Development**
- Shared code between platforms
- Platform-specific modules
- Performance considerations
- Native module bridges

**State Management**
- Flutter: Provider, GetX, Riverpod
- React Native: Redux, Zustand, Context API
- Reactive programming patterns
- Lifecycle management

## Best Practices

1. **Performance**
   - Lazy load images and data
   - Optimize list rendering
   - Memory management
   - Battery optimization
   - Network efficiency

2. **UI/UX**
   - Follow platform guidelines
   - Responsive design
   - Touch-friendly controls
   - Accessibility (a11y)
   - Dark mode support

3. **Testing**
   - Unit tests for business logic
   - Widget/component testing
   - Integration testing
   - E2E testing
   - Device testing

4. **Storage**
   - Local database (SQLite, Realm)
   - Secure storage for credentials
   - Cache management
   - File handling

## Common Patterns

**Network request with error handling:**
```dart
Future<List<User>> fetchUsers() async {
  try {
    final response = await http.get(
      Uri.parse('https://api.example.com/users'),
      headers: {'Authorization': 'Bearer $token'},
    );

    if (response.statusCode == 200) {
      return List<User>.from(
        jsonDecode(response.body).map((u) => User.fromJson(u))
      );
    } else {
      throw Exception('Failed to load users');
    }
  } catch (e) {
    print('Error: $e');
    rethrow;
  }
}
```

**Local storage with shared preferences:**
```dart
import 'package:shared_preferences/shared_preferences.dart';

final prefs = await SharedPreferences.getInstance();

// Save
await prefs.setString('username', 'john_doe');

// Read
final username = prefs.getString('username');

// Remove
await prefs.remove('username');
```

## Resources

- **React Native**: reactnative.dev
- **Flutter**: flutter.dev
- **Swift**: developer.apple.com/swift
- **Kotlin**: kotlinlang.org
- **Testing**: testing libraries for each platform
