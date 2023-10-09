
# Table of Contents

1. AppStorage
2. SwiftData

## AppStorage

The @AppStorage Property Wrapper was introduced in SwiftUI to allow easy access to the user defaults.

```swift
    @AppStorage("username") var username: String = "Anonymous"
    @AppStorage("should_show_hello_world", store: .standard) var shouldShowHelloWorld: Bool = false
```

> The app storage property wrapper *has a projected value providing a binding* that you can use with SwiftUI controls.

- @AppStorage will watch UserDefaults.standard by default
- You can create a custom UserDefaullt storage:

```swift
    @AppStorage("username", store: UserDefaults(suiteName: "group.com.hackingwithswift.unwrap"))
```

### References:

- https://www.avanderlee.com/swift/appstorage-explained/

## SwiftData

### Introduction

- SwiftData is a fast, powerful, and easy-to-use way to store data in apps built for iOS, macOS, tvOS, watchOS, and even visionOS
- Behind the scenes, SwiftData is powered by a much bigger and more mature framework called Core Data.
- Support iOS 17+
- Many features on CoreData not yet supported on SwiftData

### References:

- https://www.hackingwithswift.com/quick-start/swiftdata
