---
name: mobile-development
description: iOS Swift, Android Kotlin, React Native, Flutter, native APIs, performance, deployment.
---

# Mobile Development Skills

## Swift for iOS

```swift
import SwiftUI

@main
struct MyApp: App {
    var body: some Scene {
        WindowGroup {
            ContentView()
        }
    }
}

struct ContentView: View {
    @State private var items: [String] = []
    @State private var inputText: String = ""
    
    var body: some View {
        NavigationStack {
            VStack(spacing: 16) {
                HStack {
                    TextField("Add item", text: $inputText)
                    Button("Add") {
                        guard !inputText.isEmpty else { return }
                        items.append(inputText)
                        inputText = ""
                    }
                }
                .padding()
                
                List {
                    ForEach(items, id: \.self) { item in
                        Text(item)
                            .swipeActions {
                                Button(role: .destructive) {
                                    items.removeAll { $0 == item }
                                } label: {
                                    Label("Delete", systemImage: "trash")
                                }
                            }
                    }
                }
            }
            .navigationTitle("My Items")
        }
    }
}
```

## Kotlin for Android

```kotlin
@Composable
fun TodoApp() {
    val items = remember { mutableStateListOf<String>() }
    var inputText by remember { mutableStateOf("") }
    
    Scaffold(
        topBar = { TopAppBar(title = { Text("Todos") }) }
    ) { padding ->
        Column(modifier = Modifier.padding(padding)) {
            Row(modifier = Modifier.padding(16.dp)) {
                TextField(
                    value = inputText,
                    onValueChange = { inputText = it },
                    modifier = Modifier.weight(1f)
                )
                Button(
                    onClick = {
                        if (inputText.isNotEmpty()) {
                            items.add(inputText)
                            inputText = ""
                        }
                    }
                ) {
                    Text("Add")
                }
            }
            
            LazyColumn {
                items(items) { item ->
                    ListItem(
                        headlineContent = { Text(item) },
                        trailingContent = {
                            IconButton(onClick = { items.remove(item) }) {
                                Icon(Icons.Default.Delete, "Delete")
                            }
                        }
                    )
                }
            }
        }
    }
}
```

## React Native with TypeScript

```typescript
import React, { useState } from 'react';
import { View, TextInput, Button, FlatList, Text } from 'react-native';

interface TodoItem {
  id: string;
  text: string;
}

export const TodoApp = () => {
  const [items, setItems] = useState<TodoItem[]>([]);
  const [input, setInput] = useState('');

  const addItem = () => {
    if (input.trim()) {
      setItems([...items, { id: Date.now().toString(), text: input }]);
      setInput('');
    }
  };

  const removeItem = (id: string) => {
    setItems(items.filter(item => item.id !== id));
  };

  return (
    <View style={{ flex: 1, padding: 16 }}>
      <View style={{ marginBottom: 16 }}>
        <TextInput
          value={input}
          onChangeText={setInput}
          placeholder="Enter item"
          style={{ borderWidth: 1, padding: 8, marginBottom: 8 }}
        />
        <Button title="Add" onPress={addItem} />
      </View>
      <FlatList
        data={items}
        keyExtractor={item => item.id}
        renderItem={({ item }) => (
          <View style={{ padding: 12, flexDirection: 'row', justifyContent: 'space-between' }}>
            <Text>{item.text}</Text>
            <Button title="Delete" onPress={() => removeItem(item.id)} />
          </View>
        )}
      />
    </View>
  );
};
```

