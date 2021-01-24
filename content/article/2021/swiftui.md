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