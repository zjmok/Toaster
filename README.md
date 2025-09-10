
# Toaster 原始版

原始仓库 <https://github.com/getActivity/Toaster>

---

# Toaster 修改版

此仓库是 Toaster 修改版

使用方式与原版一致，只需要替换依赖即可

## 依赖

[![](https://jitpack.io/v/zjmok/Toaster.svg)](https://jitpack.io/#zjmok/Toaster)

build.gradle

```groovy
implementation 'com.github.zjmok:Toaster:release-SNAPSHOT'
```

or build.gradle.kts

```kotlin
implementation("com.github.zjmok:Toaster:release-SNAPSHOT")
```

## 修改内容

### 调用定位跳过调用栈数

拦截器 `ToastLogInterceptor` 新增带参数 `stackSkips` 的构造方法， 
在封装方法使用 Toaster 时，根据给定的 `stackSkips` 决定如何打印的调用定位

```java
/**
 * @param stackSkips 打印调用栈时，跳过的调用栈数。 0 表示不跳过，小于 0 表示不打印调用栈，大于 0 表示跳过指定数目的调用栈
 */
ToastLogInterceptor(int stackSkips) { /* */ }
```

`stackSkips` 0 表示不跳过，小于 0 表示不打印调用栈，大于 0 表示跳过指定数目的调用栈

- 使用示例

```
fun toast(message: Any?) {
    Toaster.show(ToastParams().apply {
        text = message.toString()
        interceptor = ToastLogInterceptor(1) // 根据实际情况调整调用栈数，每一层封装方法就是一个调用栈
    })
}
```

- 使用效果

原始版

```
button.setOnClickListener {
    toast("I am a message")
}

fun toast(message: String) {
    Toaster.show(message) // 打印的调用代码定位在此处
    // do something
    // ...
}
```

修改版

```
button.setOnClickListener {
    toast("I am a message") // 打印的调用代码定位在此处
}

fun toast(message: String) {
    val params = ToastParams().apply {
        text = message
        interceptor = ToastLogInterceptor(1) // 根据实际情况调整调用栈数，每一层封装方法就是一个调用栈
    }
    Toaster.show(params)
    // do something
    // ...
}
```
