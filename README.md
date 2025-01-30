# POC Kotlin Multiplatform to making SDK for Kotlin and Swift

## Class

- Config
- Initializer
- Interceptor
- Error

## Usage

### Config

```kotlin
val config = MyAPI.Config(
   baseUrl = "https://api.example.com",
   token = "Akawdpl10i2103",
   interceptor = MyAPI.Interceptor(...)
)
```

### Core

```kotlin
val core = new MyAPI.Initializer(config)

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
val interceptor = MyAPI.Interceptor(
      
  // Must return config
  request = { config ->
      config // Return modified config
  },

  // Must return data
  response = { responseData, validator, config ->
      val validResponseData = validator(responseData)
      validResponseData // Return validated response
  }
)

```

- saat intercept request, **config yang di ubah itu hanya untuk request itu saja**, tidak mengubah untuk global
- saat intercept response, **config yang di dapat adalah config yang di pakai saat request**, artinya config yang paling terbaru termasuk jika diubah via interceptor request
- validator adalah DTO kita, by default data dari BE harus serialized dan di validasi terlebih dahulu


### Error

``` kotlin
sealed class MyAPIError {
    data class NetworkError(val message: String) : MyAPIError()
    data class ResponseMissMatch(val message: String) : MyAPIError()
    object UnknownError: SDKError()
}
```

nanti di swift tinggal gini
```swift
switch error {
case is MyAPIError.NetworkError:
    print("Networknya tidak baik")
case is MyAPIError.ResponseMissMatch:
    print("Backend salah kasih response")
default:
    print("Unknown error")
}

```
