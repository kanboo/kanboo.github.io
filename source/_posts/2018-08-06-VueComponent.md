---
title: VueComponent
date: 2018-08-06 13:54:17
categories:
- VueJS
tags:
- VueJS
- Component
---

{% cq %}

{% asset_img components.png %}

<font style="font-size:20px;">整理 Vue.js Component 筆記</font>

{% endcq %}

<!-- more -->

---

## 檔案命名規則

紀錄一些命名方式，詳細說明可看官方說明

### 緊密耦合的 Component

- 和父組件緊密耦合的子組件，應該以 **父組件名稱** 作為前綴。

{% asset_img componentName_01.png %}

> 官方：[紧密耦合的组件名](https://cn.vuejs.org/v2/style-guide/index.html#%E7%B4%A7%E5%AF%86%E8%80%A6%E5%90%88%E7%9A%84%E7%BB%84%E4%BB%B6%E5%90%8D-%E5%BC%BA%E7%83%88%E6%8E%A8%E8%8D%90)

### 組件命名的順序

- 以概念上較普遍的單詞作為開頭，以描述性的修飾詞結尾。

{% asset_img componentName_02.png %}

> 官方：[组件名中的单词顺序](https://cn.vuejs.org/v2/style-guide/index.html#%E7%BB%84%E4%BB%B6%E5%90%8D%E4%B8%AD%E7%9A%84%E5%8D%95%E8%AF%8D%E9%A1%BA%E5%BA%8F-%E5%BC%BA%E7%83%88%E6%8E%A8%E8%8D%90)

### Component 命名與引用

使用單一檔案組件 （.vue）的話，不用多想，全部 <font color="red">PascalCase</font> 就對了

需要使用 <font color="red">kebab-case</font> 的情況只在你把 template 寫在 <font color="red">.html</font> 裡面，
如此一來它會經 browser 解析，而 browser 是<font color="red">不分大小寫</font>的，
所以才需要加 - 讓 Vue 知道大寫的轉換規則以找到組件。

{% asset_img componentName_03.png %}

> FB 討論文：[Vue 的元件命名建議](https://www.facebook.com/groups/vuejs.tw/permalink/1774411302638594/)

### prop 命名

- 在 HTML 中，使用 kebab-case
- 在 JavaScript 中，使用 camelCase

{% asset_img componentName_04.png %}

> 官方：[Prop-名大小写](https://cn.vuejs.org/v2/style-guide/index.html#Prop-%E5%90%8D%E5%A4%A7%E5%B0%8F%E5%86%99-%E5%BC%BA%E7%83%88%E6%8E%A8%E8%8D%90)

<div class="note info">官方：[优先级 B 的规则：强烈推荐 (增强可读性)](https://cn.vuejs.org/v2/style-guide/index.html#%E4%BC%98%E5%85%88%E7%BA%A7-B-%E7%9A%84%E8%A7%84%E5%88%99%EF%BC%9A%E5%BC%BA%E7%83%88%E6%8E%A8%E8%8D%90-%E5%A2%9E%E5%BC%BA%E5%8F%AF%E8%AF%BB%E6%80%A7)</div>

---

## props

### 資料傳遞過程

用下圖來表示外部資料傳入`Vue.component`的整個過程。

{% asset_img components_01.png %}

> 圖片來源：JSNWORK-[Vue.component 組件教學](http://jsnwork.kiiuo.com/archives/2611/vue-vue-component-%E7%B5%84%E4%BB%B6%E6%95%99%E5%AD%B8-%E4%BD%BF%E7%94%A8-v-bind%E3%80%81v-for%E3%80%81x-template/)

### 單向數據流

盡可能保持單向數據流的概念，就是讓資料是從 <font color="red">**外層 → 內層**</font> 單方向傳遞。

### AJAX 與 Component 的時間差

有時我們的資料可能是由 **AJAX** 的方式取得，如下例的 Vue 的 **created** ，
而當 **component** 要讀取**父層的資料**時，可能這時還**沒有資料**，
所以這時 **component** 直接讀取父層的 **某個變數** 資料時，但為空值而錯誤。

#### 錯誤範例

開啟錯誤範例，看 console 出現的錯誤訊息。

```html
<div id="app">
  <!-- Component -->
  <card :user-data="user"></card>
</div>

<script>
var app = new Vue({
  el: '#app',
  components: {
    card
  },
  data: {
    user: {},
  },
  created: function () {
    var vm = this;
    $.ajax({
      url: 'https://randomuser.me/api/',
      dataType: 'json',
      success: function (data) {
        vm.user = data.results[0];
      }
    });
  }
});
</script>
```

> 錯誤範例：[JSBin](http://jsbin.com/tulejanupu/1/edit?html,js,output)

#### 修正版本

這時可以利用 `v-if` 來判斷，透過 `AJAX` 取得的資料是否已取得，
避免在父層資料還為空值時，就被讀取而錯誤。

```html
<div id="app">
  <!-- Component -->
  <card :user-data="user" v-if="user.picture"></card>
</div>
```

> 正確範例：[JSBin](http://jsbin.com/bolinuxidi/1/edit?html,js,output)

<div class="note info">參考文章：
[JSNWORK-Vue.component 組件教學](http://jsnwork.kiiuo.com/archives/2611/vue-vue-component-%E7%B5%84%E4%BB%B6%E6%95%99%E5%AD%B8-%E4%BD%BF%E7%94%A8-v-bind%E3%80%81v-for%E3%80%81x-template/)
[CHRIS-父子組件溝通 pass props / emit event](https://dotblogs.com.tw/wasichris/2017/03/04/021726)
[jeremy-父子組件溝通 - Props](https://jeremysu0131.github.io/Vue-js-%E7%88%B6%E5%AD%90%E7%B5%84%E4%BB%B6%E6%BA%9D%E9%80%9A-Props/)</div>

### 定義盡可能詳細

養成好習慣，避免以後踩雷。

#### 一般寫法

```html
<div id="app">
  <!-- 傳入參數值 -->
  <prompt-component :user-name="name"></prompt-component>
</div>
```

```javascript
Vue.component('PromptComponent', {
  template: '<button @click="sayHi(userName)">Say Hi!</button>',
  props: ['user-name'], //使用`props`聲明它所獲得的資料
  methods: {
    sayHi: function(name) {
      alert('Hi ' + name);
    }
  }
});

var vm = new Vue({
  el: '#app',
  data: {
    name: 'Kanboo'
  }
});
```

> 程式範例：[JSBin](http://jsbin.com/damirubalo/1/edit?html,js,output)

#### 嚴謹寫法

```html
<div id="app">
  <!-- 傳入參數值 -->
  <my-component
    :message="message"
    :name="name">
  </my-component>
</div>
```

```javascript
Vue.component('MyComponent', {
  template: '<div class="component">{{message}} {{name}} </div>',
  props: {
    message: {
      type: String, // 型態
      default: 'Hello' // 預設值
    },
    name: {
      type: String, // 型態
      required: true // 是否必填
    }
  }
});

var vm = new Vue({
  el: '#app',
  data: {
    message: 'Hi',
    name: 'Kanboo'
  }
});
```

> 程式範例：[JSBin](http://jsbin.com/jogacazayo/1/edit?html,js,output)

<div class="note info">官方：[Prop](https://cn.vuejs.org/v2/guide/components-props.html#Prop-%E7%B1%BB%E5%9E%8B)</div>

### props 變數命名

當我們在 HTML 在 Component 裡，設定`greeting-text`屬性，
這樣在`props`要如何正確對應到正確的變數呢！

範例：

```html
<!-- HTML使用 kebab-case -->
<todo-item greeting-text="hello"></todo-item>
```

可看到下面有二種方式，

1.  用雙引號包起來，這樣就跟在 HTML Component 的屬性名稱一樣
2.  使用駝峰式 camelCase，來對應屬性名稱。

```javascript
Vue.component('TodoItem', {
  template: '<h1>{{ greetingText }}</h1>',
  props: {
    // 第一種方式
    'greeting-text': String,
    // 第二種方式(建議)
    greetingText: String
  }
});
```

<div class="note info">官方：[组件注册](https://cn.vuejs.org/v2/guide/components-registration.html)</div>

---

## emit 向外層傳送事件

雖然我們要盡可能保持單向數據流的概念，就是讓資料是從 外層 → 內層 單方向傳遞，
不過有特殊情況的話，需要由 **內層** 去變動 **外層** 的值的話，
這時就需透過 `$emit` 傳遞事件方式，去觸發外層事件來更新變數值。

{% asset_img components_02.png %}

> 範例程式：[透過 emit 向外傳遞資訊](http://jsbin.com/joleliwefa/1/edit?html,js,output)

---

## Slot 插槽替換

簡單來說就是將我們的 component，修改為一個可客制化的 component。

以 **DailogBox** 為例，
我們每次使用都有可能因不同情況，而輸出不同的「**標題、內文、按鈕**」，
但我們的主體結構是長一樣的，這時就可以使用`Slot`來替換局部 HTML。

### 範例

當有**多個插槽**時，我們就必須給予**插槽名稱**，才能正確替換局部 HTML。

原則上遵照下列幾點即可：

1.  外層是在 `Tag標籤`，新增 `slot` 屬性，如：`<header slot="header">替換的 Header</header>`
2.  內層是在 `slot標籤`，新增 `name` 屬性，如：`<slot name="header">這段是預設的文字</slot>`
3.  若無設定 `slot名稱` 的話，就會自己找洞插，不過一個洞就還好，多個的話，怕插錯洞。

```html
<div id="app">
  <h2>具名插槽(使用 預設值)</h2>
  <named-slot-component>
  </named-slot-component>

  <h2>具名插槽(替換 預設值)</h2>
  <named-slot-component>
    <header slot="header">替換的 Header</header>
    <template slot="footer">替換的 Footer</template>
    <template slot="btnString">按鈕內容</template>
    <p slot>其餘的內容</p>
  </named-slot-component>
</div>


<script type="text/x-template" id="namedSlotComponent">
  <div class="card my-3">
    <div class="card-header">
      <slot name="header">這段是預設的文字</slot>
    </div>
    <div class="card-body">
      <slot>
        <h5 class="card-title">預設內文表頭</h5>
        <p class="card-text">預設內文文字</p>
      </slot>
      <a href="#" class="btn btn-primary">
        <slot name="btnString">預設動作內容</slot>
      </a>
    </div>
    <div class="card-footer">
      <slot name="footer">這是預設的 Footer</slot>
    </div>
  </div>
</script>
```

加上顏色標籤，方便對應程式碼

{% asset_img components_03.png %}

---

## 使用 is 動態切換 Component

原先我們是使用 `is` 來決定要掛載哪一個 Component

```html
<!-- is 沒有冒號 -->
<div is="primary-component" :data="item">
```

不過有時可能要因應不同條件下，顯示不同的 Component，我們可能用 `v-if` 這樣寫去切換

```html
<!-- A情況 -->
<primary-component :data="item" v-if="current === 'primary-component'"></primary-component>
<!-- B情況 -->
<danger-component :data="item" v-if="current === 'danger-component'"></danger-component>
```

但可能有超過二種以上條件的話，程式碼就顯得雜亂，這時我們可以改寫用 `變數` 方式，並配合 `is` 的特性去做切換。

### is 動態切換範例

注意行數

- 第 2 行：需使用`:is`，而不是`is`，差在有沒有<font color="red">冒號</font>。
- 第 23 行：用`current`變數，來紀錄目前要使用哪個 Component

遵守上述二點，這時我們就可以在不同條件下，變更`current`的值，來切換不同 component。

```html
<!-- is 有冒號 -->
<div class="mt-3" :is="current" :data="item"></div>

<script>
Vue.component('primary-component', {
  props: ['data'],
  template: '#primaryComponent',
});
Vue.component('danger-component', {
  props: ['data'],
  template: '#dangerComponent',
});


var app = new Vue({
  el: '#app',
  data: {
    item: {
      header: '這裡是 header',
      title: '這裡是 title',
      text: '這是 context'
    },
    current: 'primary-component'
  }
});
</script>
```

---

## Component 為何要用 return data

主要要說明 `new Vue` 的 `data` 及 `Component` 的 `data`，
一個是使用 `{}`(物件)，一個卻要使用 `return {}`(物件)，
這二者差異點是在哪裡！

### 修改前

當有重覆性的 Component 且會運用 **變數** 來紀錄的話，
有時很有可能會導致共用到同一個 **變數** ，
而互相影響彼此的結果，就像下面的例子一樣，可以開啟`CodePen`，
分別 **點擊** 裡面的按鈕，看會出現什麼結果。

```html
<div id="app">
  <div>
    1.你已經點擊
    <button class="btn btn-outline-secondary btn-sm" @click="counter += 1">{{ counter }}</button> 下。
  </div>
  <div>
    2.你已經點擊
    <button class="btn btn-outline-secondary btn-sm" @click="counter += 1">{{ counter }}</button> 下。
  </div>
</div>

<script>
  var app = new Vue({
    el: '#app',
    data: {
      counter: 0
    },
  });
</script>
```

> 非 Component：[CodePen](https://codepen.io/Kanboo/pen/bKVmQw)

### 修改後

跟修改前，最大的差異就在於額外新增一段程式碼 `Vue.component`，
利用此功能達成 Component 化。

```html
<div id="app">
  修改後：
  <div>
    3.你已經點擊 <counter-component></counter-component> 下。
  </div>
  <div>
    4.你已經點擊 <counter-component></counter-component> 下。
  </div>
</div>

<script>
  // 新增Vue-component組件
  Vue.component('counter-component', {
    data: function () {
      return {
        counter: 0
      }
    },
    template: `
      <button class="btn btn-outline-secondary btn-sm" @click="counter += 1">{{ counter }}</button>
    `
  })

  var app = new Vue({
    el: '#app',
    data: {
      counter: 0
    },
  });
</script>
```

> Component：[CodePen](https://codepen.io/Kanboo/pen/wXKQKJ)

### 注意事項

不過使用 `Vue.component` 要特別注意一點，就是在 `data` 的宣告部份，

原本是使用 `{}`(物件) 的方式宣告，而在`Vue.component`的`data`一定要使用`函式`宣告，

並且 `return {}`(物件)，否則無法正常執行。

```javascript
Vue.component('counter-component', {
  // 宣告為function，並且 return 物件
  data: function() {
    return {
      counter: 0
    };
  },
  template: `
    <button class="btn btn-outline-secondary btn-sm" @click="counter += 1">{{ counter }}</button>
  `
});
```

### 差異說明

- Component 會採用 **函式 return 值** 的原因，是因為 Component 會一直被重覆建立，而每個 Component 的 data 必須有所區隔，利用此方式來建立<font color="red">不同記憶體位址</font>的 `{}`(物件)

- new Vue 的 data 直接使用`{}`(物件)，也是因為只會只有一個，此 Vue 並不會被別人複製，所以就不會發生共用到同一個記憶體位址，而產生互改別人的資料的問題。

---
