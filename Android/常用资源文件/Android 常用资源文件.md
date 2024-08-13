# Android 常用资源文件

## TextView圆角矩形：

```xml
<?xml version="1.0" encoding="utf-8"?>
<shape xmlns:android="http://schemas.android.com/apk/res/android">
    <corners android:radius="20dp"/>
    <solid android:color="@color/chartreuse"/>
    <stroke
        android:width="2dp"
        android:color="@color/text_blue"/>
</shape>
```



## 搜索框：

![image-20240714170713755](https://cdn.jsdelivr.net/gh/kennems/blog-image/image-20240714170713755.png)

```xml
    <!-- 搜索框 -->
    <RelativeLayout
        android:id="@+id/rl_search"
        android:layout_width="0dp"
        android:layout_height="wrap_content"
        android:gravity="center"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toBottomOf="@id/tv_title">

        <RelativeLayout
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:layout_margin="16dp"
            android:background="@drawable/search_box"
            android:padding="10dp">

            <ImageView
                android:id="@+id/iv_search_icon"
                android:layout_width="24dp"
                android:layout_height="24dp"
                android:layout_alignParentStart="true"
                android:layout_centerVertical="true"
                android:src="@drawable/ic_magnifying_glass" />

            <EditText
                android:id="@+id/et_search"
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:layout_marginStart="14dp"
                android:layout_toEndOf="@id/iv_search_icon"
                android:background="@android:color/transparent"
                android:hint="请输入问题关键词"
                android:inputType="text"
                android:padding="0dp"
                android:textSize="16sp" />

        </RelativeLayout>
    </RelativeLayout>
```

## 下拉选择框：

![image-20240714171613886](https://cdn.jsdelivr.net/gh/kennems/blog-image/image-20240714171613886.png)

```xml
    <Spinner
        android:id="@+id/sp_language"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginStart="25dp"
        android:layout_marginTop="30dp"
        android:text="简体中文"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="@+id/iv_back" />
```

```java
        Spinner sp_language = findViewById(R.id.sp_language); // 获取Spinner控件
        String[] languages = {"简体中文", "繁体中文", "English"};
        ArrayAdapter<String> adapter = new ArrayAdapter<>(this, android.R.layout.simple_spinner_item, languages);
        adapter.setDropDownViewResource(android.R.layout.simple_spinner_dropdown_item);
        sp_language.setAdapter(adapter);
```

## 文字选择高亮， 背景变化

添加文字颜色选择xml实现文件选择变成蓝色， 动态设置背景色实现背景为蓝色。

![image-20240714173151639](https://cdn.jsdelivr.net/gh/kennems/blog-image/image-20240714173151639.png)

![image-20240714173304048](https://cdn.jsdelivr.net/gh/kennems/blog-image/image-20240714173304048.png)

![image-20240714173005557](https://cdn.jsdelivr.net/gh/kennems/blog-image/image-20240714173005557.png)



