# SimpleDateFormat()

如果你的前缀都是中文字符（或者不是 `A–Z`／`a–z` 的字母），那么在 `SimpleDateFormat` 的 pattern 里可以直接写，不用额外占位符也不用转义。比如：

```xml
<!-- res/values/strings.xml（简体中文） -->
<resources>
    <!-- 这里“更新于：”都是中文，SimpleDateFormat 不会把它当作 pattern 字母去解析 -->
    <string name="long_date_format">更新于：yyyy/MM/dd</string>
</resources>
```

接着在代码里直接这样用，就完全不需要临时变量／占位符：

```kotlin
val pattern = getString(R.string.long_date_format)
val formatted = SimpleDateFormat(pattern, Locale.getDefault())
    .format(Date())
textView.text = formatted
```

运行效果（假设今天是 2025-05-08）：

```
更新于：2025/05/08
```

------

如果你还需要英文环境下也直接在 resource 里写（不想在代码里拼接），只要把英文前缀用单引号包起来并在 XML 中转义即可。例如：

```xml
<!-- res/values-en/strings.xml（英语） -->
<resources>
    <!-- 在 XML 里，单引号要写成 \' -->
    <string name="long_date_format">\'Updated on: \'MM/dd/yyyy</string>
</resources>
```

这样英文环境里 `pattern` 会被解析为

```
'Updated on: 'MM/dd/yyyy
```

输出

```
Updated on: 05/08/2025
```

两套资源对应两种语言，都不用在代码里做额外的字符串拼接或占位，仅靠 `SimpleDateFormat` 就能在不同语言下输出带前缀的日期。