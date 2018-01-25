---
title: ES6-樣板字串 (Template String)
date: 2018-01-25 09:31:37
categories: 
- JS
- ES6
tags:
- JS
- ES6
- 樣板字串
- Template String
---

{% cq %}

{% asset_img ES6.jpg %}

<font style="font-size:20px;">Template String 樣板字串</font>

{% endcq %}

<!-- more -->
***

## 基本用法

``` js 新舊寫法比較
const people = ['老大','老二','老么'];


//======================過去寫法======================
let oldUl = '<ul>' +
            '<li>我叫做 ' + people[0] + '</li>' +
            '<li>我叫做 ' + people[1] + '</li>' +
            '<li>我叫做 ' + people[2] + '</li>' +
            '</ul>';
  
console.log(oldUl);
//"<ul><li>我叫做 老大</li><li>我叫做 老二</li><li>我叫做 老么</li></ul>"

//======================好好寫法======================
let newUl = `
  <ul>
    <li>我叫做 ${people[0]}</li>
    <li>我叫做 ${people[1]}</li>
    <li>我叫做 ${people[2]}</li>
  </ul>
`
console.log(newUl);
//  <ul>
//    <li>我叫做 老大</li>
//    <li>我叫做 老二</li>
//    <li>我叫做 老么</li>
//  </ul>
```

甚至可以在 <font color="red">${}</font> 內使用<font color="red">函式</font> (${}裡再包${})。

``` js 使用.map組字串(結果與上例一樣)
let newUl = `
  <ul>
    ${people.map(person => `<li>我叫做 ${person}</li>`).join('')}
  </ul>
`
```

也可以在函式內增添<font color="red">更多的判斷式</font>。

``` js 新增if判斷式
const people = [
  {
    name: '老大',
    friends: 2
  },
  {
    name: '老二',
    friends: 16
  },
  {
    name: '老么',
    friends: 0
  }
]

let newUl = `
  <ul>
    ${people.map((person) => {
        if (person.friends) {
          return `<li>${person.name} 有 ${person.friends} 朋友</li>`
        } else {
          return `<li>${person.name} 沒朋友</li>`
        }
      }).join('')
    }
  </ul>
`

console.log(newUl);
//  <ul>
//    <li>老大 有 2 朋友</li><li>老二 有 999 朋友</li><li>老么 邊緣人</li>
//  </ul>
```

***
## 巢狀 String Template
如同上述的方法 <font color="red">${}</font> 內可以加入<font color="red">函式</font>及其更內層的 Template String，
所以也可以在 <font color="red">${}</font> 插入<font color="red">另一組的函式</font>的 Template String。

``` js 在${}裡呼叫function
const travelers = {
  leader: "爸爸",
  partner: [
    {
      name: "老大",
      friends: 2
    },
    {
      name: "老二",
      friends: 16
    },
    {
      name: "老么",
      friends: 0
    }
  ]
};


function renderList(people) {
  return `
    <div>上車名單</div>
    <ul>
      ${people.map(person => `<li>${person.name}</li>`).join('')}
    </ul>
  `
}

let template = `
  <div class="template">
  <h2>開車：${travelers.leader}</h2>
  ${renderList(travelers.partner)}
  </div>
`

console.log(template);
// <div class=\"template\">
// <h2>開車：爸爸</h2>
// 
//   <div>上車名單</div>
//   <ul>
//     <li>老大</li><li>老二</li><li>老么</li>
//   </ul>
// 
// </div>
```

***
## 跳脫字元

如果有需要插入<font color="red">特殊字元</font>，一樣可以使用 <font color="red">\</font> 反斜線來插入：

``` js
console.log(`\\`); // "\"
```

如果要計算字元數，或是需要將字串做額外處理，<font color="red">跳脫字元是不佔字符數</font>的：

``` js
console.log(`\\`.length); // 1
```

要取得<font color="red">含</font>特殊字元的字串可用 String.raw()：

``` js
console.log(String.raw`\\`.length) // 2
```

***
## 參考來源
<div class="note info">[邁向 JavaScript 勇者之路](https://ithelp.ithome.com.tw/users/20083608/ironman/1354)</div>