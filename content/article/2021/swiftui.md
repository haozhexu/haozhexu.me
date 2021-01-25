---
title: "SwiftUI Quick Notes"
date: 2021-01-23T13:19:00+11:00
draft: true
ads: true
categories:
  - Notes
tags:
  - ios
  - swiftui
---

## Concept

```swift
import SwiftUI

struct ContentView: View {
    var body: some View {
        Text("Hello, World!")
            .padding()
    }
}

struct ContentView_Previews: PreviewProvider {
    static var previews: some View {
        ContentView()
    }
}
```

- by default, SwiftUI files declare two structures
  - one conforms to `View` protocol and describes the view's content and layout
  - the other declares a preview for that view
- UI is constructed using __Views__ and view can be customized by __Modifiers__
- Views can be organized in containers
  - `VStack` and `HStack`
  - `ZStack` organizes views one on top of another, a.k.a __Depth Stack__
  - Stack views' initializers have last parameter defined as `@ViewBuilder`, which allows multiple views declared in a single closure
  - maximum of 10 views are allowed in stack view, use another stack view for more

```swift
init(alignment: HorizontalAlignment = .center, spacing: CGFloat? = nil, @ViewBuilder content: () -> Content)
```

### Opaque Type

- keyword `some` specifies an opaque type
- the system doesn't need to know what exact type is returned, as long as the type conforms to `View` protocol
- all possible return types must be of the same type

## Wrappers

### @State

- modify value inside struct
  - storage of such property is managed by SwiftUI
  - SwiftUI can destroy and recreate struct without losing its states
- should be used for simple types, e.g. `String`, `Int`
- usually marked as `private`, to share values across views, use `@ObservedObject` or `@EnvironmentObject`

### @ObservedObject (with @Published property wrapper)

- custom type that may have multiple properties or methods
- shared across multiple views
- developer responsible for managing its data
- should conform to `ObservableObject` protocol
- `@Published` property wrapper can be used to notify view that data has changed
- other ways to notify data change, e.g. custom publisher from `Combine`, must happen on main thread

### @EnvironmentObject

- such object is available to all views through the application, like shared data