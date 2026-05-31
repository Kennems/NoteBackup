---
title : 'React Native'
date : 2026-03-22T20:43:04+08:00
lastmod: 2026-03-22T20:43:04+08:00
description : "React Native（简称 RN）是 Facebook（现 Meta）开源的跨平台移动应用框架，允许使用 JavaScript/TypeScript 和 R..."
image : img/cat.jpg
draft : false
categories : ["大前端"]
tags : ["学习笔记", "大前端"]
---
# React Native

## 概述

React Native（简称 RN）是 Facebook（现 Meta）开源的跨平台移动应用框架，允许使用 JavaScript/TypeScript 和 React 语法编写 iOS 和 Android 原生应用。与 React Web 不同，RN 组件映射到真实的原生 UI 组件（iOS 的 UIView、Android 的 ViewGroup），而非 DOM 元素。

## 核心组件

RN 提供了一套跨平台的基础组件，取代了 Web 开发中的 HTML 标签：

### View
最基础的布局容器，相当于 Web 的 `<div>`。支持 Flexbox 布局、触摸事件处理、样式和可访问性控制。

```jsx
<View style={{ flex: 1, justifyContent: 'center', alignItems: 'center' }}>
  <Text>Hello World</Text>
</View>
```

### Text
用于显示和样式化文本内容，支持嵌套（如在普通文本内嵌入加粗或彩色文字）。

```jsx
<Text>
  这是<Text style={{ fontWeight: 'bold' }}>加粗</Text>的文本
</Text>
```

### ScrollView
通用滚动容器，会一次性渲染所有子元素。适用于内容有限的页面（如设置页，约 20 项以内）。**不推荐用于长列表**。

### FlatList
高性能列表组件，采用**懒渲染**策略——只渲染屏幕可见区域内的项，滚动时回收复用。适合动态长列表（如信息流、商品列表）。常用属性：`data`、`renderItem`、`keyExtractor`、`onEndReached`。

```jsx
<FlatList
  data={products}
  renderItem={({ item }) => <ProductCard product={item} />}
  keyExtractor={item => item.id}
  onEndReached={loadMore}
/>
```

### 列表组件选型指南
- **< 20 项**：ScrollView 即可
- **长列表、无分组**：FlatList
- **分组数据+分组标题**：SectionList
- **极长列表（千项以上）**：推荐使用 Shopify 的 FlashList

## 导航

RN 使用第三方库 `react-navigation` 实现页面导航（2025 年事实标准）：

- **Stack Navigator**：标准推入/弹出页面切换（如首页 → 详情页）
- **Tab Navigator**：底部或顶部标签栏导航
- **Drawer Navigator**：侧边抽屉菜单

```jsx
import { NavigationContainer } from '@react-navigation/native';
import { createNativeStackNavigator } from '@react-navigation/native-stack';

const Stack = createNativeStackNavigator();
// 推荐使用 native-stack（原生实现）而非 JS 版 stack，性能更优
```

## 样式系统

RN 使用 JavaScript 对象写样式，采用 Flexbox 布局（默认 flexDirection 为 column，与 Web 的 row 不同）。不支持 CSS 选择器、动画使用 `Animated` API 或 `react-native-reanimated`。

## 与 React Web 的关键区别

| 维度 | React Web | React Native |
|------|-----------|--------------|
| 渲染目标 | DOM 元素 | 原生 UI 组件 |
| 样式 | CSS / CSS-in-JS | JS 对象 + Flexbox |
| 路由 | React Router | React Navigation |
| 动画 | CSS Animation | Animated / Reanimated |

## 2025 年趋势

- **新架构已成默认**：RN 0.76+ 默认启用 Fabric（新渲染系统）和 TurboModules，性能显著提升
- **Expo 推荐入门**：官方文档推荐使用 Expo 创建新项目
- **TypeScript 优先**：所有官方指南和课程以 TypeScript 为主
