---
title: VueRouter
date: 2018-08-06 15:50:00
categories:
- VueJS
tags:
- VueJS
- Router
---

{% cq %}

{% asset_img vuerouter.jpg %}

<font style="font-size:20px;">整理 Vue.js Router 筆記</font>

{% endcq %}

<!-- more -->

---

## 路由設定

下面範例為簡單使用路由的起手式。

### 設定頁面路徑

```javascript
import Vue from 'vue';
import Router from 'vue-router';

// 載入頁面元件
import Index from '@/components/Index';
import Product from '@/components/Product';

Vue.use(Router);

export default new Router({
  routes: [
    {
      path: '/', // 對應的虛擬路徑
      name: 'Index', // 元件呈現的名稱
      component: Index // 對應的元件
    },
    {
      path: '/product',
      name: 'Product',
      component: Product
    },
    {
      // 如果無對應的網址，就導回首頁。
      path: '*',
      name: 'Index',
      component: Index
    }
  ]
});
```

### 切換頁面

```html
<template>
  <div id="app">

    <!-- 路由切換的方式 -->
    <!-- 直接用 網址 切換路由(to 沒冒號) -->
    <router-link to="/product">PRODUCT</router-link>

    <!-- 使用變數 路徑path 切換路由(to 有冒號) -->
    <router-link :to="{name: 'product'}">PRODUCT</router-link>
    <!-- 使用變數 名稱name 切換路由(to 有冒號) -->
    <router-link :to="{name: 'product'}">PRODUCT</router-link>

    <!-- ------------------------------------------------ -->
    <!-- 路由顯示界面 -->
    <router-view/>

  </div>
</template>
```

<div class="note info">官方：[router-link-props](https://router.vuejs.org/zh/api/#router-link-props)</div>

---

## 巢狀路由

籍由**巢狀路由**的設定，可以分不同區塊集中管理。

### 範例

**設定頁面路徑**

```javascript
export default new Router({
  routes: [
    {
      path: '/product', // 父層
      name: 'Product',
      component: Product,
      children: [
        {
          path: '', // 第一個子項(default)
          name: '全部',
          component: categoryAll
        },
        {
          path: '/category1', // 第二個子項
          name: '分類1',
          component: category1
        },
        {
          path: '/category2', // 第三個子項
          name: '分類2',
          component: category2
        }
      ]
    }
  ]
});
```

根據上列程式，切換路由有下列幾種情況

1.  網址：`/product` → 會取得`children` 底下的第一個子項。
2.  網址：`/product/category1` → 會取得`children` 底下的第二個子項。

**切換頁面**

```html
<template>
  <div id="app">

    <!-- 路由連結 -->
    <router-link to="/product">全部</router-link>
    <router-link to="/product/category1">分類1</router-link>
    <router-link to="/product/category2">分類2</router-link>

    <!-- 切換路由界面 -->
    <router-view/>

  </div>
</template>
```

> 範例程式碼：[JSFiddle](https://jsfiddle.net/yyx990803/L7hscd8h/)

---

## 動態路由

當有一種情況是很多頁面，不過版型都一樣，只有資料面不一樣的話，
我們就可以利用**動態 id**的方式，透過 AJAX 取得資料，置換掉版型的內容，
這樣就只需產出一個`Component`元件就好。

```javascript
// 載入頁面
import Index from '@/components/Index';
import categoryAll from '@/components/categoryAll';
import category from '@/components/category';

export default new Router({
  routes: [
    {
      path: '/product',
      // name: 'Product',         // 若子項目有設定default的話，改用default
      // component: Product,      // 若子項目有設定default的話，改用default
      children: [
        {
          path: '', // default設定
          name: '全部',
          component: categoryAll
        },
        {
          path: '/category/:id', // 動態路由設定，以冒號開頭
          name: '分類',
          component: category
        }
      ]
    }
  ]
});
```

而在 Vue 檔裡，可使用`$route.params.id`來取的 id，
到時就可利用此 id 透過 ajax 取得資料，並且更新版型的內谷。

**範例**

| pattern                       | matched path        | $route.params                      |
| ----------------------------- | ------------------- | ---------------------------------- |
| /user/:username               | /user/evan          | { username: 'evan' }               |
| /user/:username/post/:post_id | /user/evan/post/123 | { username: 'evan', post_id: 123 } |

> 範例程式碼：[JSFiddle](https://jsfiddle.net/yyx990803/4xfa2f19/)

---

## 同一路徑載入二個元件

可在一個頁面上同時載入多個元件的話，除了一個`<router-view>`是沒有命名，
其餘新增的 `<router-view>` 都需要額外新增 `name` 的屬性。

```html
<template>
  <div id="app">

    <!-- default -->
    <router-view></router-view>

    <!-- 指定名：list -->
    <router-view name="list"></router-view>
    <!-- 指定名：total -->
    <router-view name="total"></router-view>

  </div>
</template>
```

```javascript
// 載入頁面
import Product from '@/components/Product';
import TheHeader from '@/components/TheHeader';
import List from '@/components/List';
import Total from '@/components/Total';

export default new Router({
  routes: [
    {
      path: '/product',
      // name: 'Product',
      // component: Product,
      components: {
        // 設定多個 component
        default: TheHeader,
        list: List,
        total: Total
      }
    }
  ]
});
```

---

## 路由的 CSS

當我們使用`<router-link>`在切換路由時，而`<router-link>`實際會轉換成 HTML 的`<a>`連結標籤，
所以當我們想針對目前點擊的`<a>`連結標籤做 <font color="red">CSS 效果</font>時，可利用下列二個 CSS 的 class 名稱

- `linkActiveClass`：預設頁面，符合此條件的`<a>`標籤，會有此 class。

- `linkExactActiveClass`：實際點擊`<a>`標籤後，會變成此 class。

> 小坑：[vue.js 默认路由不加载 linkActiveClass 的问题](https://segmentfault.com/a/1190000012353669)

---

## 自定義切換路由方法

可利用 Vue 的 methods，在動作觸發時，使用程式來切換路由。

### router.push

這個方法會向`history`添加一個新的記錄，所以，當用戶點擊瀏覽器後退按鈕時，則回到之前的 URL。

```javascript
methods: {
  chagnePath() {
    // 字符串
    this.$router.push('home')

    // 对象
    this.$router.push({ path: 'home' })

    // 命名的路由
    this.$router.push({ name: 'user', params: { userId: 123 }})

    // 带查询参数，变成 /register?plan=private
    this.$router.push({ path: 'register', query: { plan: 'private' }})
  }
}
```

### router.replace

跟`router.push`很像，唯一的不同就是，它<font color="red">不會向 history 添加新記錄</font>，
而是跟它的方法名一樣，是 **替換掉當前的 history 記錄**。

### 其他 router

```javascript
// 在history記錄中向前或者後退多少步，類似window.history.go(n)。
this.$router.go(-1);
this.$router.go(2);

// 往前一頁
this.$router.back();

// 往後一頁
this.$router.forward();
```

<div class="note primary">注意：
在 Vue 實例内部，你可以通过 `$router` 访问路由实例，因此你可以调用 `this.$router.push`。</div>

<div class="note info">官方：[Router 实例方法](https://router.vuejs.org/zh/api/#router-%E5%AE%9E%E4%BE%8B%E6%96%B9%E6%B3%95)
[编程式的导航](https://router.vuejs.org/zh/guide/essentials/navigation.html)</div>

---

## Lazy Loading Routes

主要是設定**Compnent**時，採用 `import('@/pages/Comics')` 這樣的寫法的話，
會將各個頁面的 JS 拆分開來，而不是 build 專案時，只有一個一大包的 JS 檔，
如此一來的話，當 User 開啟某頁面時，才會載入當頁面所需的 JS 檔，
避免 User 初次進到首頁時，感覺到網站速度慢的狀態。

### LazyLoadingRoutes\_前

步驟：

1.  先在上方 import 元件
2.  在 routes 設定每個 Path，對應的元件

```javascript
import Vue from 'vue';
import Router from 'vue-router';

// 載入頁面元件
import Comics from '@/pages/Comics';
import ComicDetail from '@/pages/ComicDetail';
import ComicChapter from '@/pages/ComicChapter';

Vue.use(Router);

export default new Router({
  routes: [
    {
      path: '/comics',
      name: 'Comics',
      component: Comics // 設定頁面元件
    },
    {
      path: '/comics/:id',
      name: 'ComicDetail',
      component: ComicDetail // 設定頁面元件
    },
    {
      path: '/comics/:id/chapter/:cid',
      name: 'ComicChapter',
      component: ComicChapter // 設定頁面元件
    },
    {
      path: '*',
      redirect: '/comics'
    }
  ]
});
```

### LazyLoadingRoutes\_後

步驟：

1.  直接在 routes 設定每個 Path 時，import 頁面元件

```javascript
import Vue from 'vue';
import Router from 'vue-router';

Vue.use(Router);

export default new Router({
  routes: [
    {
      path: '/comics',
      name: 'Comics',
      component: () => import('@/pages/Comics') // import寫在這
    },
    {
      path: '/comics/:id',
      name: 'ComicDetail',
      component: () => import('@/pages/ComicDetail') // import寫在這
    },
    {
      path: '/comics/:id/chapter/:cid',
      name: 'ComicChapter',
      component: () => import('@/pages/ComicChapter') // import寫在這
    },
    {
      path: '*',
      redirect: '/comics'
    }
  ]
});
```

### 比較圖

由下圖可看出，有無使用**LazyLoadingRoutes**的差異

- LazyLoadingRoutes\_前 → js 檔案共用 3 支。
- LazyLoadingRoutes\_後 → js 檔案共用 6 支。

{% asset_img LazyLoadingRoutes.png %}

---

## 驗證

檢查 User 是否已登入狀態

### 範例

在程式碼第**22**行位置，可以看到 **about** 的 path，有新增 <font color="red">**meta**</font> 物件，透過檢查 `authorization` 變數

```javascript
/* eslint no-console:off */
import VueRouter from 'vue-router';
import Vue from 'vue';
import store from './store';

import Main from './component/Main.vue';

Vue.use(VueRouter);
const About = () => import('./component/About.vue');
const Login = () => import('./component/Login.vue');

const log = value =>
  console.log(
    `%c${value}`,
    'background: #bdc3c7; color: black; font-size:10px;'
  );

const router = new VueRouter({
  mode: 'history',
  routes: [
    { path: '/', component: Main },
    { path: '/about', component: About, meta: { authorization: true } },
    { path: '/login', component: Login }
  ]
});

// 導向頁面前的檢查機制
router.beforeEach((to, from, next) => {
  log(`Router beforeEach to: ${to.path} from: ${from.path}`);

  // 判斷此頁面是否需要 登入狀態
  if (to.matched.some(record => record.meta.authorization || false)) {
    const isLogin = store.state.isLogin; //取得 Vuex 的 isLogin 變數值
    if (isLogin) {
      // 已登入
      next();
    } else {
      // 未登入，導向 login 頁面
      next({ path: '/login', query: { redirect: to.fullPath } });
    }
  } else {
    next();
  }
});
export default router;
```

---

## 參考文章

<div class="note info">[網站路由 vue-router](https://dotblogs.com.tw/wasichris/2017/03/06/235449)
[路由超連結產生器 router-link](https://dotblogs.com.tw/wasichris/2017/03/07/001939)
</div>
