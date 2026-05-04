---
title : 'Jetpack Compose(6)'
date : 2025-06-05T10:30:13+08:00
lastmod: 2025-06-05T10:20:13+08:00
description : "Android学习笔记" 
categories : ["Android学习笔记"]
tags : ["Android"]
---

# Jetpack Compose(6)

## 动画

简单值动画

`animateColorAdState()`



### animateColorAsState

```kotlin
    val backgroundColor by animateColorAsState(if (tabPage == TabPage.Home) Seashell else GreenLight)
```

### AnimatedVisibility

```kotlin
            AnimatedVisibility(extended) {
                Text(
                    text = stringResource(R.string.edit),
                    modifier = Modifier
                        .padding(start = 8.dp, top = 3.dp)
                )
            }
```

```kotlin
AnimatedVisibility(
    visible = shown,
    enter = slideInVertically(
        initialOffsetY = { fullHeight -> -fullHeight },
        animationSpec = tween(
            durationMillis = 300,
            easing = FastOutLinearInEasing
        )
    ),
    exit = slideOutVertically(
        targetOffsetY = { fullHeight -> -fullHeight },
        animationSpec = tween(
            durationMillis = 150,
            easing = LinearOutSlowInEasing
        )
    )
) {
    Surface(
        modifier = Modifier.fillMaxWidth(),
        color = MaterialTheme.colorScheme.secondary,
        shadowElevation = 18.dp
    ) {
        Text(
            text = stringResource(R.string.edit_message),
            modifier = Modifier.padding(16.dp)
        )
    }
}
```

### animateContentSize

```
modifier = Modifier
    .fillMaxWidth()
    .padding(16.dp)
    .animateContentSize()
```

### updateTransition

```kotlin
val transition = updateTransition(
    tabPage,
    label = "Tab indicator"
)
val indicatorLeft by transition.animateDp(
    transitionSpec = {
        if (TabPage.Home isTransitioningTo TabPage.Work) {
            spring(stiffness = Spring.StiffnessVeryLow)
        } else {
            spring(stiffness = Spring.StiffnessMedium)
        }
    },
    label = "Indicator left"
) { tabPage ->
    tabPositions[tabPage.ordinal].left
}
val indicatorRight by transition.animateDp(
    transitionSpec = {
        if (TabPage.Home isTransitioningTo TabPage.Work) {
            spring(stiffness = Spring.StiffnessMedium)
        } else {
            spring(stiffness = Spring.StiffnessVeryLow)
        }
    },
    label = "Indicator right"
) { tabPage ->
    tabPositions[tabPage.ordinal].right
}
val color by transition.animateColor(
    label = "Indicator color"
) { tabPage ->
    if (tabPage == TabPage.Home) PaleDogwood else Green
}
```



## 手势动画

### 点击

- `clickable` 修饰符允许应用检测对已应用该修饰符的元素的点击
- 当需要更大灵活性式，可以通过 `pointerInput` 修饰符提供点按手势检测器

```kotlin
@Composable
fun GestureSample() {
    Row(
        verticalAlignment = Alignment.CenterVertically,
        horizontalArrangement = Arrangement.Center,
        modifier = Modifier
            .fillMaxWidth()
    ) {
        ClickableSample()
    }
}

@Composable
fun ClickableSample() {
    val count = remember {
        mutableIntStateOf(0)
    }
    Text(
        text = count.intValue.toString(),
        textAlign = TextAlign.Center,
        modifier = Modifier
            .clickable {
                count.intValue += 1
            }
            .wrapContentSize()
            .background(Color.LightGray)
            .padding(horizontal = 50.dp, vertical = 40.dp)
    )
}
```

