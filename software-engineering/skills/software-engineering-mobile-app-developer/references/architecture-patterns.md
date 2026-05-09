---
type: reference
project: skills-library
scope: skill
plugin: software-engineering
parent: "[[mobile-app-developer]]"
tags: [type/reference, plugin/software-engineering, scope/skill, topic/software]
status: active
---
# Mobile Architecture Patterns (2025)

## Modular Architecture for Scaling

### Feature-Based Organization
```
lib/
├── features/
│   ├── auth/
│   │   ├── data/
│   │   ├── domain/
│   │   └── presentation/
│   ├── home/
│   ├── profile/
│   └── settings/
├── core/
│   ├── constants/
│   ├── network/
│   ├── storage/
│   └── utils/
└── main.dart
```

**Benefits**:
- Each feature is independent
- Easy to onboard new developers
- Clear dependencies
- Testable in isolation

## State Management

### 1. Provider (Recommended for most apps)
Simple, performance-friendly, minimal boilerplate.

```dart
// Define state
final counterProvider = StateNotifier<int>((ref) => 0);

// Use in widget
Consumer(builder: (context, ref, child) {
  final count = ref.watch(counterProvider);
  return Text('$count');
});
```

### 2. Redux (for complex apps)
Predictable state, time-travel debugging, but more boilerplate.

### 3. BLoC (Event-driven)
Good for event-driven apps, more overhead.

## Offline-First Patterns

### Data Flow
```
Local Cache (SQLite) → Network Request → Merge Results → Cache Update
```

### Conflict Resolution
```
1. If online: Push local changes, fetch remote, merge intelligently
2. If offline: Queue changes, mark as pending, sync when online
3. Conflict: Manual resolution or last-write-wins (simple) or crdt (complex)
```

### Implementation Example
```dart
// Sync manager
class SyncManager {
  final localDb = LocalDatabase();
  final api = ApiClient();

  Future<void> sync() async {
    // Upload pending changes
    final pending = await localDb.getPendingChanges();
    for (var change in pending) {
      await api.upload(change);
    }

    // Fetch and merge
    final remote = await api.getLatest();
    await localDb.merge(remote);
  }
}
```

## Performance Optimization

### 1. Lazy Loading
Load data as needed, not all upfront.

### 2. Image Caching
Cache images locally with size limits.

```dart
Image.network(
  url,
  cacheWidth: 500,
  cacheHeight: 500,
  fadeInDuration: Duration(milliseconds: 300),
)
```

### 3. List Optimization
Use ListView.builder or GridView.builder, not ListView with all items.

### 4. Memory Management
Monitor memory with DevTools, avoid memory leaks in listeners.

## UI/Component Reusability

Define a component library (like shadcn/ui for Flutter):

```dart
// Custom button with consistent styling
class AppButton extends StatelessWidget {
  final String label;
  final VoidCallback onPressed;
  final AppButtonStyle style; // primary, secondary, outline

  @override
  Widget build(BuildContext context) {
    // Apply design system styling
  }
}
```

Reuse across all screens for consistency.
