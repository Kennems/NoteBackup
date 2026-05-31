---
title : '青训营 x 字节前端训练营'
date : 2024-08-14T03:36:29+08:00
lastmod: 2026-03-22T20:43:04+08:00
description : "- `<header>`, `<nav>`, `<main>`, `<section>`, `<article>`, `<aside>`, `<footer>`"
image : img/cat.jpg
draft : false
categories : ["青训营"]
tags : ["学习笔记", "青训营"]
---
# 青训营 x 字节前端训练营

## 一、HTML5 基础

### 1.1 语义化标签
- `<header>`, `<nav>`, `<main>`, `<section>`, `<article>`, `<aside>`, `<footer>`
- 优点：SEO 友好、可访问性提升、代码可维护性高

### 1.2 新特性
- **Canvas & SVG**：2D 绘图能力
- **音视频**：`<video>`、`<audio>` 原生支持
- **WebStorage**：`localStorage`（持久化）与 `sessionStorage`（会话级）
- **WebWorker**：多线程脚本，不阻塞 UI
- **WebSocket**：全双工通信
- **History API**：pushState / replaceState 实现 SPA 路由

### 1.3 表单增强
- 新 input 类型：`email`、`tel`、`number`、`range`、`date`、`color`
- 新属性：`placeholder`、`required`、`pattern`、`autofocus`

## 二、CSS3 核心

### 2.1 盒模型
- `box-sizing: content-box`（默认）vs `border-box`
- 标准盒模型：width = content
- IE/怪异盒模型：width = content + padding + border

### 2.2 布局

#### Flexbox
- 主轴（main axis）与交叉轴（cross axis）
- 容器属性：`display: flex`、`flex-direction`、`justify-content`、`align-items`、`flex-wrap`
- 项目属性：`flex-grow`、`flex-shrink`、`flex-basis`、`align-self`

#### Grid
- 容器属性：`display: grid`、`grid-template-columns`、`grid-template-rows`、`gap`
- 项目属性：`grid-column`、`grid-row`、`grid-area`
- 移动端优先：`auto-fit` + `minmax()` 实现响应式

### 2.3 动画与过渡
- `transition`：属性变化过渡（`transition-property`、`duration`、`timing-function`、`delay`）
- `@keyframes` + `animation`：关键帧动画
- GPU 加速属性：`transform`、`opacity`（触发合成层）

### 2.4 预处理器
- **Sass/SCSS**：变量、嵌套、mixin、@extend
- **PostCSS**：Autoprefixer 自动添加厂商前缀

## 三、JavaScript 进阶

### 3.1 作用域与闭包
- 全局/函数/块级作用域（let/const）
- 执行上下文：变量提升（hoisting）、暂时性死区
- 闭包：函数 + 其词法环境的引用
  - 应用：柯里化、防抖节流、模块模式
  - 注意：内存泄漏风险

### 3.2 this 指向
- 默认绑定（全局 / undefined strict）
- 隐式绑定（调用者）
- 显式绑定（call / apply / bind）
- new 绑定（实例对象）
- 箭头函数：继承外层 this

### 3.3 原型与继承
- 每个函数有 `prototype`，每个对象有 `__proto__`
- 原型链查找机制
- ES6 class：`extends`、`super`、`static`

### 3.4 异步编程
- **Callback**：回调地狱
- **Promise**：三种状态（pending / fulfilled / rejected）
  - 链式调用 `.then().catch().finally()`
  - 静态方法：`Promise.all`、`Promise.race`、`Promise.allSettled`、`Promise.any`
- **async/await**：语法糖，本质是 Generator + Promise
- **事件循环**：宏任务（setTimeout、I/O）→ 微任务（Promise.then、MutationObserver）

### 3.5 ES6+ 常用特性
- 解构赋值、展开运算符
- `Map` / `Set` / `WeakMap` / `WeakSet`
- `Symbol`、`Proxy`、`Reflect`
- 模块化：`import` / `export`

## 四、TypeScript 基础

### 4.1 类型系统
```typescript
// 基础类型
let name: string = "hello";
let count: number = 42;
let isDone: boolean = false;

// 数组与元组
let arr: number[] = [1, 2, 3];
let tuple: [string, number] = ["age", 25];

// 枚举
enum Direction { Up, Down, Left, Right }

// 联合类型与交叉类型
type Status = "success" | "error";
type A = { a: number } & { b: string };

// 泛型
function identity<T>(arg: T): T { return arg; }
```

### 4.2 接口与类型别名
- `interface`：可继承（extends）、可合并声明
- `type`：可为联合/交叉/元组类型起别名
- 优先用 interface，需要联合类型时用 type

### 4.3 高级类型
- **类型守卫**：`typeof`、`instanceof`、`in`、自定义守卫 `arg is Type`
- **条件类型**：`T extends U ? X : Y`
- **映射类型**：`Partial<T>`、`Required<T>`、`Pick<T, K>`、`Omit<T, K>`
- **工具类型**：`Record<K, V>`、`Exclude<T, U>`、`ReturnType<T>`

### 4.4 声明文件
- `.d.ts` 为 JS 库提供类型
- `declare module`、`declare global`

## 五、前端框架

### 5.1 React

#### 核心概念
- **JSX**：语法糖，`React.createElement` 的语法扩展
- **Virtual DOM**：内存中的 DOM 表示，Diff 算法（同层比较、key 优化）
- **Fiber 架构**：可中断的协调过程，分片渲染

#### Hooks
- `useState`：状态管理
- `useEffect`：副作用（生命周期替代）
  - [] 仅挂载、[dep] 依赖变化、return 清理
- `useMemo` / `useCallback`：性能优化（记忆化）
- `useRef`：DOM 引用 + 可变值
- `useContext`：跨层级传递数据
- 自定义 Hook：抽取逻辑复用

#### 状态管理
- **Redux**：单向数据流 → Action → Dispatch → Reducer → Store
- **Zustand**：轻量、无 Provider 包裹
- **React Query / SWR**：服务端状态管理

### 5.2 Vue

#### 核心特性
- **响应式系统**：Vue2 `Object.defineProperty`（数组限制），Vue3 `Proxy`（全量拦截）
- **模板语法**：`{{ }}` 插值、`v-bind`、`v-for`、`v-if`、`v-model`
- **Composition API**：`setup()`、`ref()`、`reactive()`、`computed()`、`watch()`
- **生命周期**：onMounted、onUnmounted、onUpdated 等

#### 组件通信
- props / emit（父子）
- provide / inject（跨级）
- EventBus / mitt（任意组件）
- Pinia（Vue3 推荐状态管理）

## 六、浏览器渲染原理

### 6.1 渲染流程（关键渲染路径）
1. **HTML 解析** → DOM Tree（字节 → 字符 → Token → Node）
2. **CSS 解析** → CSSOM Tree
3. **合并** → Render Tree（只包含可见节点）
4. **布局（Layout）**：计算几何位置（box 尺寸、位置）
5. **绘制（Paint）**：绘制像素（颜色、阴影、文字）
6. **合成（Composite）**：图层合并 → GPU 显示

### 6.2 重排（Reflow）与重绘（Repaint）
- **Reflow**：几何属性变化，成本极高
  - 触发：增删 DOM、尺寸变化、字体变化、窗口 resize
- **Repaint**：外观变化（颜色、背景），不影响布局
- **优化策略**：
  - 批量 DOM 操作（DocumentFragment）
  - 使用 class 而非内联样式
  - 开启 GPU 加速（`transform`、`opacity`）
  - 减少布局信息读取（如 `offsetHeight` 强制同步布局）

### 6.3 加载优化
- `<link rel="preload">`：提前加载关键资源
- `<link rel="prefetch">`：空闲时加载下次导航资源
- async / defer 脚本加载差异

## 七、Web 性能优化

### 7.1 加载优化
- **代码分割**：动态 import、React.lazy、Vue 异步组件
- **Tree Shaking**：移除未引用代码（ES Module）
- **资源压缩**：JS/CSS/HTML minify、WebP 图片格式
- **CDN**：静态资源边缘分发，减少延迟
- **HTTP 缓存**：强缓存（Cache-Control: max-age）与协商缓存（ETag/Last-Modified）

### 7.2 渲染优化
- **懒加载**：图片懒加载（IntersectionObserver）、路由懒加载
- **虚拟滚动**：只渲染可视区域（react-window / vue-virtual-scroller）
- **防抖（debounce）与节流（throttle）**：
  - 防抖：触发后等待 N 秒执行（搜索建议）
  - 节流：N 秒内只执行一次（滚动事件）

### 7.3 性能指标（Web Vitals）
- **LCP（Largest Contentful Paint）**：<= 2.5s，最大内容渲染
- **FID（First Input Delay）**：<= 100ms，首次输入延迟
- **CLS（Cumulative Layout Shift）**：<= 0.1，累积布局偏移
- TTFB、FCP、INP 等辅助指标

### 7.4 构建层优化
- Webpack：loader、plugin、splitChunks、缓存（cache-loader / persistent cache）
- Vite：基于 ESM 的 dev server + Rollup 生产构建
- 图片压缩：imagemin、sharp

## 八、前端工程化

### 8.1 模块化演进
- IIFE → CommonJS（Node.js）→ AMD（RequireJS）→ ES Module（标准）
- ESM 优势：静态分析、Tree Shaking、循环依赖处理好

### 8.2 包管理
- **npm**：依赖树、package-lock.json 锁定版本
- **yarn**：确定性安装、Plug'n'Play
- **pnpm**：硬链接 + 软链接节省磁盘，严格隔离依赖

### 8.3 构建工具
- **Webpack**：配置复杂但生态强大
- **Vite**：开发冷启动快（esbuild 预构建）、HMR 即时更新
- **Turbopack**：Rust 编写，增量编译极致性能

### 8.4 代码规范
- **ESLint**：代码质量规则，配合 Prettier 格式化
- **Husky + lint-staged**：pre-commit 钩子，提交前自动检查
- **Commitlint**：约定式提交（Conventional Commits）

### 8.5 测试体系
- **单元测试**：Jest / Vitest（组件 + 工具函数）
- **组件测试**：Testing Library（React Testing Library / Vue Test Utils）
- **E2E 测试**：Cypress / Playwright
- 覆盖率：istanbul v8 报告

### 8.6 CI/CD
- GitHub Actions / GitLab CI
- 流程：lint → test → build → deploy
- Docker 容器化部署，Nginx 反向代理

## 九、前沿与拓展

### 9.1 微前端
- qiankun（蚂蚁开源的沙箱架构）
- Module Federation（Webpack 5：运行时依赖共享）
- 优点：独立开发、独立部署、技术栈无关

### 9.2 SSR / SSG
- **SSR（服务端渲染）**：Next.js（React）、Nuxt.js（Vue）
  - 优点：首屏快、SEO 友好
  - 缺点：服务器负载高
- **SSG（静态站点生成）**：构建时生成 HTML
  - 适合内容型网站（博客、文档）

### 9.3 WebAssembly
- 将 C/C++/Rust 编译为 WASM 在浏览器运行
- 适用场景：计算密集型（图像处理、游戏引擎）

### 9.4 跨端方案
- **React Native**：React 语法 → 原生组件
- **Flutter**：Dart 语言，自绘引擎
- **Taro / uni-app**：一套代码多端发布（H5 / 小程序 / App）
- **Electron / Tauri**：桌面端应用

## 十、面试高频题整理

### 10.1 手写代码
```javascript
// 防抖
function debounce(fn, delay) {
  let timer = null;
  return function (...args) {
    clearTimeout(timer);
    timer = setTimeout(() => fn.apply(this, args), delay);
  };
}

// 节流
function throttle(fn, delay) {
  let last = 0;
  return function (...args) {
    const now = Date.now();
    if (now - last >= delay) {
      last = now;
      fn.apply(this, args);
    }
  };
}

// 深拷贝（简易版）
function deepClone(obj, map = new WeakMap()) {
  if (obj === null || typeof obj !== 'object') return obj;
  if (map.has(obj)) return map.get(obj);
  const clone = Array.isArray(obj) ? [] : {};
  map.set(obj, clone);
  for (const key in obj) {
    if (obj.hasOwnProperty(key)) {
      clone[key] = deepClone(obj[key], map);
    }
  }
  return clone;
}

// 手写 Promise.all
Promise.myAll = function (promises) {
  return new Promise((resolve, reject) => {
    const results = [];
    let count = 0;
    if (promises.length === 0) resolve(results);
    promises.forEach((p, i) => {
      Promise.resolve(p).then(val => {
        results[i] = val;
        count++;
        if (count === promises.length) resolve(results);
      }, reject);
    });
  });
};
```

### 10.2 常见问题
| 类别 | 问题 |
|------|------|
| JS | var/let/const 区别、闭包应用、事件循环输出顺序 |
| TS | interface vs type、泛型约束、工具类型实现 |
| React | setState 同步异步、Fiber 原理、diff 策略 |
| Vue | v-model 原理、nextTick 实现、响应式拦截 |
| 工程 | Webpack 构建流程、Vite 对比 Webpack 差异 |
| 网络 | 跨域方案（CORS、JSONP、代理）、HTTPS 握手 |
| 安全 | XSS 与 CSRF 防御、CSP 策略 |

---

*注：本笔记基于字节青训营前端课程内容整理，持续更新中。*
