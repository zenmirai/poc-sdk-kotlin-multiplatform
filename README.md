# POC Kotlin Multiplatform to making SDK for Kotlin and Swift

## Class

- Config
- Initializer
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
let core = new MyAPI.Initializer(config)

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
  request = (config) -> {
      return config
  },

  # must return data
  response = (responseData, validator, config) -> {
      let validResponseData = validator(responseData)
      return validResponseData
  }
)
```

- saat intercept request, **config yang di ubah itu hanya untuk request itu saja**, tidak mengubah untuk global
- saat intercept response, **config yang di dapat adalah config yang di pakai saat request**, artinya config yang paling terbaru termasuk jika diubah via interceptor request
- validator adalah DTO kita, by default data dari BE harus serialized dan di validasi terlebih dahulu
