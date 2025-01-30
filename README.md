# POC Kotlin Multiplatform to making SDK for Kotlin and Swift

## Class

- Config
- Core
- Interceptor

## Usage

### Config
```kotlin
let config = new MyAPI.Config(
   baseUrl = "https://api.example.com",
   token = "Akawdpl10i2103",
   interceptor = new MyAPI.Interceptor(...)
)
```

### Core

```kotlin
let core = new MyAPI.Core(config)

core.getUser(config)
```

- getUser adalah (contoh) function yang akan di call
- config di Core dan di getUser tidak wajib
- jika ada config di getUser, maka config di getUser akan di override dengan config di Core
- jika ada interceptor di getUser, maka interceptor di getUser akan di override dengan interceptor di Core
  - misal di getUser passing hanya `request` interceptor, maka `request` interceptor di getUser akan di override dengan `request` interceptor di Core tapi `response` interceptor di getUser tidak akan di override

### Interceptor

ini adalah cara membuat interceptor sekaligus memberitahukan logika bawaan interceptor jika tidak di passing via Core class / getUser function

```kotlin
new MyAPI.Interceptor(
      
  # must return config
  # jika tidak ada interceptor, return config
  request = (config) -> {
      return config
  },

  # must return data
  # jika tidak ada interceptor, return validated data
  response = (maybeValidData, validator) -> {
      # default interceptornya gini
      let validData = validator(maybeValidData)
      return validData
  }
)
```
