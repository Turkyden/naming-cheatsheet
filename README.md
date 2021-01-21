
<p align="center">
  <a href="https://github.com/kettanaito/naming-cheatsheet">
    <img src="https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/f4079a9a15364be48d8a80395c578d01~tplv-k3u1fbpfcp-watermark.image" alt="Naming cheatsheet" />
  </a>
</p>

# 命名小抄

- [英文](#英文)
- [命名约定](#命名约定)
- [S-I-D](#s-i-d)
- [避免缩写](#避免缩写)
- [避免上下文重复](#避免上下文重复)
- [反映预期结果](#反映预期结果)
- [函数命名](#函数命名)
  - [A/HC/LC 模式](#ahclc-模式)
    - [行为](#行为)
    - [上下文](#上下文)
    - [前缀](#前缀)
- [单数和复数](#单数和复数)

---

为一个东西取名字是挺难的，相信这篇小抄能够帮到你。

虽然这些命名建议可以使用任何语言描述，但我将通过 JavaScript 在具体使用场景中说明它。

## 英文

当命名你的变量和函数时，请使用英文。

```js
/* Bad */
const primerNombre = 'Gustavo'
const amigos = ['Kate', 'John']

/* Good */
const firstName = 'Gustavo'
const friends = ['Kate', 'John']
```

> 无论你是否喜欢，英文是编程的主要语言：所有编程语言的语法都是用英语写的，除此之外还有许多文档和学习资料也都是。通过全英文编写代码，你能极大的提高代码的一致性。

## 命名约定

选择**一个**命名约定然后遵循它。可能是 `小写` 或者 `蛇形下划线`，或者任何其他的，都无所谓。重点是让它们保持一致。

```js
/* Bad */
const pages_count = 5
const shouldUpdate = true

/* Good */
const pageCount = 5
const shouldUpdate = true

/* Good as well */
const page_count = 5
const should_update = true
```

## S-I-D

一个名称必须是 _简短_，_直观_，_描述性_ 的：

- **简短**. 一个名字一定不要花很长的时间来键入，因此，请记住
- **直观**. 一个名字必须读起来自然，尽可能接近日常用语;
- **描述性**. 一个名字必须以最有效的方式反映它所执行/包含的东西。

```js
/* Bad */
const a = 5 // "a" 可能意味着任何东西
const isPaginatable = a > 10 // "Paginatable" 听起来极不自然
const shouldPaginatize = a > 10 // 造一个动词看起来很搞笑!

/* Good */
const postCount = 5
const hasPagination = postCount > 10
const shouldDisplayPagination = postCount > 10 // 或者
```

## 避免缩写

千万**不要**使用缩写。他们只会降低代码的可读性，寻找一个简短的、描述性的名字可能很难，但是它们不应该是你使用缩写的借口。

```js
/* Bad */
const onItmClk = () => {}

/* Good */
const onItemClick = () => {}
```

## 避免上下文重复

名称不应该与定义它的上下文重复。如果你不想降低名称的可读性，请始终删除名称的上下文。

```js
class MenuItem {
  /* 方法名重复了上下文（即 "MenuItem"）*/
  handleMenuItemClick = (event) => { ... }

  /* 更方便阅读 `MenuItem.handleClick()` */
  handleClick = (event) => { ... }
}
```

## 反映预期结果

命名应该反映预期结果

```jsx
/* Bad */
const isEnabled = itemCount > 3
return <Button disabled={!isEnabled} />

/* Good */
const isDisabled = itemCount <= 3
return <Button disabled={isDisabled} />
```

---

# 函数命名

## A/HC/LC Pattern

命名的时候要遵循这一有用的公式:

```
prefix? + action (A) + high context (HC) + low context? (LC)
```

在下面的表中，看看如何应用这个模式。

| 命名                   | 前缀   | 行为 (A) | 高语境 (HC) | 低语境 (LC) |
| ---------------------- | -------- | ---------- | ----------------- | ---------------- |
| `getPost`              |          | `get`      | `Post`            |                  |
| `getPostData`          |          | `get`      | `Post`            | `Data`           |
| `handleClickOutside`   |          | `handle`   | `Click`           | `Outside`        |
| `shouldDisplayMessage` | `should` | `Display`  | `Message`         |                  |

> **笔记：** 上下文的顺序会影响变量的含义。例如：`shouldUpdateComponent` 意味着组件你要更新组件，而 `shouldComponentUpdate` 会告诉你组件将自己更新，你只需要控制它什么时候应该更新就好了。换句话就是说，**高语境强调变量的意义**。

---

## 行为

函数名的动词部分。描述函数功能的最重要部分。

### `get`

快速获取数据 (i.e. 内部数据获取的简短方式).

```js
function getFruitCount() {
  return this.fruits.length
}
```

> 可以参考下 [compose](#compose)

### `set`

声明式地设置一个变量，把 `A` 设为 `B`。

```js
let fruits = 0

function setFruits(nextFruits) {
  fruits = nextFruits
}

setFruits(5)
console.log(fruits) // 5
```

### `reset`

将变量设置为初始值或状态。

```js
const initialFruits = 5
let fruits = initialFruits
setFruits(10)
console.log(fruits) // 10

function resetFruits() {
  fruits = initialFruits
}

resetFruits()
console.log(fruits) // 5
```

### `fetch`

在某些不确定的时刻请求一些数据（即异步请求）。

```js
function fetchPosts(postCount) {
  return fetch('https://api.dev/posts', {...})
}
```

### `remove`

从某个地方移除某个东西。

例如，如果你在一个搜索页面上有一个精选过滤器的集合，从集合中删除他们之一是`removeFilter`， **而不是** `deleteFilter` (这是你在英语中自然的口语化的表达方式):

```js
function removeFilter(filterName, filters) {
  return filters.filter((name) => name !== filterName)
}

const selectedFilters = ['price', 'availability', 'size']
removeFilter('price', selectedFilters)
```

> 可以参考下 [delete](#delete).

### `delete`

完全从存储的地方抹除去了一些东西。

假设你是一名内容编辑，还有一个你想删掉的臭名昭著的帖子。当你点击高亮的“删除文章”按钮时，CMS 内容管理系统就执行的是 `deletePost` 行为，**而不是** `removePost`。

```js
function deletePost(id) {
  return database.find({ id }).delete()
}
```

> 可以参考下 [remove](#remove).

### `compose`

从现在已有的数据创建一个新的数据。大多数用于字符串，对象，或者函数。

```js
function composePageUrl(pageName, pageId) {
  return `${pageName.toLowerCase()}-${pageId}`
}
```

> 可以参考下 [get](#get).

### `handle`

处理一个动作。经常在命名回调方法时使用。

```js
function handleLinkClick() {
  console.log('Clicked a link!')
}

link.addEventListener('click', handleLinkClick)
```

---

## 上下文

函数作用的域。

函数通常是对某件 _事物_ 的操作。重要的是要说明它的可操作域是什么，或者至少是预期的数据类型。

```js
/* 一个使用 js 原生操作的纯函数 */
function filter(predicate, list) {
  return list.filter(predicate)
}

/* 函数精准地对 post 进行操作 */
function getRecentPosts(posts) {
  return filter(posts, (post) => post.date === Date.now())
}
```

> 一些特定于语言的推断可以允许省略上下文。例如，在 JavaScript 中，使用 `filter` 对数组进行操作是很常见的。没有必要添加显式的 `filterArray`。

---

## 前缀

前缀增强了变量的含义。但它很少用于函数名中。

### `is`

描述当前上下文的特性或状态（通常为布尔值 `boolean`）。

```js
const color = 'blue'
const isBlue = color === 'blue' // 特性
const isPresent = true // 状态

if (isBlue && isPresent) {
  console.log('Blue is present!')
}
```

### `has`

描述当前上下文是否具有某个值或状态(通常为布尔值 `boolean`)。

```js
/* Bad */
const isProductsExist = productsCount > 0
const areProductsPresent = productsCount > 0

/* Good */
const hasProducts = productsCount > 0
```

### `should`

反映一个主动性的条件语句和一个特定的动作(通常是布尔值 `boolean`)。

```js
function shouldUpdateUrl(url, expectedUrl) {
  return url !== expectedUrl
}
```

### `min`/`max`

表示最小值或最大值。用于描述界限或限制。

```js
/**
 * 渲染一个随机数量的文章
 * 给出一个最大/最小边界值.
 */
function renderPosts(posts, minPosts, maxPosts) {
  return posts.slice(0, randomBetween(minPosts, maxPosts))
}
```

### `prev`/`next`

指示当前上下文中变量的前一个或下一个状态。在描述状态转换时使用。

```jsx
function fetchPosts() {
  const prevPosts = this.state.posts

  const fetchedPosts = fetch('...')
  const nextPosts = concat(prevPosts, fetchedPosts)

  this.setState({ posts: nextPosts })
}
```

## 单数和复数

和前缀一样，变量名也可以是单数或复数，这取决于它们是包含单个值还是多个值。

```js
/* Bad */
const friends = 'Bob'
const friend = ['Bob', 'Tony', 'Tanya']

/* Good */
const friend = 'Bob'
const friends = ['Bob', 'Tony', 'Tanya']
```
