---
title: JS30-04-Array-Cardio-Day-1
date: 2018-02-16 22:11:47
categories: 
- JS
- JS30
tags:
- JS
- JS30
---

{% cq %}

{% asset_img arraymethod.jpg %}

<font style="font-size:20px;">練習運用 <font color="red">Array</font> 的各種函式</font>

{% endcq %}

<!-- more -->
***

## 目標

- 共提供四組資料
    - inventors：first(名)、last(姓) 、year(出生日期)、passed(死亡日期)
    - wiki網站的html ：[網址連結](https://en.wikipedia.org/wiki/Category:Boulevards_in_Paris)
    - people：逗點分隔的姓名(firstName, lastName)
    - data：提供的一組包含重覆資料的陣列
- 根據不同需求條件篩選出正確的資料

## 練習題目

inventors 的資料：

1. 篩選出於 1500~1599 年間出生的 inventor (year in 1500-1599)
2. 將 inventors 的 firstname 與 lastname 組合成一個陣列
3. 將 inventors 依據「生日」由大至小排序
4. 加總所有 inventor 的年齡
5. 將 inventors 依據「年齡」由大至小排序

wiki網站的html：[網址連結](https://en.wikipedia.org/wiki/Category:Boulevards_in_Paris)

6. 列出wiki中巴黎所有包含’de’的路名

people 的資料：

7. 依據lastName排序所有people的資料

data 的資料：

8. 分別計算data內每個種類的數量

## 成品

>[[DEMO]](https://kanboo.github.io/JavaScript30/04%20-%20Array%20Cardio%20Day%201/) | [[GitHub]](https://github.com/kanboo/JavaScript30/blob/master/04%20-%20Array%20Cardio%20Day%201/index.html)

***
## inventors 題目

``` js inventors資料
const inventors = [
    { first: 'Albert', last: 'Einstein', year: 1879, passed: 1955 },
    { first: 'Isaac', last: 'Newton', year: 1643, passed: 1727 },
    { first: 'Galileo', last: 'Galilei', year: 1564, passed: 1642 },
    { first: 'Marie', last: 'Curie', year: 1867, passed: 1934 },
    { first: 'Johannes', last: 'Kepler', year: 1571, passed: 1630 },
    { first: 'Nicolaus', last: 'Copernicus', year: 1473, passed: 1543 },
    { first: 'Max', last: 'Planck', year: 1858, passed: 1947 },
    { first: 'Katherine', last: 'Blodgett', year: 1898, passed: 1979 },
    { first: 'Ada', last: 'Lovelace', year: 1815, passed: 1852 },
    { first: 'Sarah E.', last: 'Goode', year: 1855, passed: 1905 },
    { first: 'Lise', last: 'Meitner', year: 1878, passed: 1968 },
    { first: 'Hanna', last: 'Hammarström', year: 1829, passed: 1909 }
];
```

<span id="inline-toc">Q：</span>篩選出於 1500~1599 年間出生的 inventor (year in 1500-1599)

<span id="inline-toc">A：</span>透過 filter 篩選出符合條件的資料，最終回傳一個新陣列

``` js filter
// Array.prototype.filter()
// 1. Filter the list of inventors for those who were born in the 1500's

/* 解法 */
const bornArr = inventors.filter(function(inventor){
  if (inventor.year >= 1500 && inventor.year < 1600){
    return true;
  }
})

/* 簡化語法 */
const bornArr = inventors.filter( inventor => inventor.year >= 1500 && inventor.year < 1600)

console.table(bornArr);
```

<span id="inline-toc">Q：</span>將 inventors 的 firstname 與 lastname 組合成一個陣列

<span id="inline-toc">A：</span>透過 map 將原始資料加工後，最終回傳一個新陣列

``` js map
// Array.prototype.map()
// 2. Give us an array of the inventors' first and last names

/* 解法 */
const newInventors = inventors.map(function(inventor){
  return inventor.first + ' ' + inventor.last
})

/* 簡化語法 */
const newInventors = inventors.map( inventor => inventor.first + ' ' + inventor.last);

/* 簡化語法(Template String) */
const newInventors = inventors.map( inventor => `${inventor.first} ${inventor.last}`); //Template String

console.log(newInventors);
```

<span id="inline-toc">Q：</span>將 inventors 依據「生日」由大至小排序

<span id="inline-toc">A：</span> 依照「生日」大小重新排序(return值： -1 排前面 , 1 排後面)

``` js sort
// Array.prototype.sort()
// 3. Sort the inventors by birthdate, oldest to youngest

/* 解法 */
const sortInventors = inventors.sort(function(a, b){
  if ( a.year > b.year){
    return 1
  }else{
    return -1
  }
})

/* 簡化語法 */
const sortInventors = inventors.sort( (a, b) => a.year > b.year ? 1 : -1 )

console.table(sortInventors)
```



<span id="inline-toc">Q：</span>加總所有 inventor 的年齡

<span id="inline-toc">A：</span> 籍由 reduce 加總所有的年齡

``` js reduce
// Array.prototype.reduce()
// 4. How many years did all the inventors live?

/* 解法 */
const totalYears = inventors.reduce(function(total, inventor){
  return total + (inventor.passed - inventor.year)
}, 0) 

/* 簡化語法 */
const totalYears = inventors.reduce( (total, inventor) => total + (inventor.passed - inventor.year) , 0)

console.log(totalYears);
```

<span id="inline-toc">Q：</span>將 inventors 依據「年齡」由大至小排序

<span id="inline-toc">A：</span>依照「年齡」大小重新排序(return值： -1 排前面 , 1 排後面)

``` js
// 5. Sort the inventors by years lived

/* 解法 */
const sortYearsLived = inventors.sort(function(a, b){
  const aYearCount = a.passed - a.year;
  const bYearCount = b.passed - b.year;

  if ( aYearCount > bYearCount) {
    return -1
  }else{
    return 1
  }
})


/* 簡化語法 */
const sortYearsLived = inventors.sort( (a, b) => {
    const aYearCount = a.passed - a.year;
    const bYearCount = b.passed - b.year;

    return aYearCount > bYearCount ? -1 : 1
})

console.table(sortYearsLived);
```

***
## wiki-html 題目

<span id="inline-toc">Q：</span>列出wiki中巴黎所有包含’de’的路名

<span id="inline-toc">A：</span>練習 展開運算子(Spread Operator)、map()、filter()、includes() 搭配使用

``` js method組合運用
// 6. create a list of Boulevards in Paris that contain 'de' anywhere in the name
// https://en.wikipedia.org/wiki/Category:Boulevards_in_Paris

/* 解法 */
const category = document.querySelector('.mw-category');

//將 nodeList 轉為 Array型態
const Links = [...category.querySelectorAll('a')]; //展開運算子(Spread Operator)

//可以執行完 map 後，緊接著執行 filter
const de = Links
            .map( link => link.textContent)
            .filter( tName => tName.includes('de'))

console.log(de);
```

***
## people 題目

<span id="inline-toc">Q：</span>依據 lastName 排序所有people的資料

<span id="inline-toc">A：</span>練習 解構賦值(Destructuring Assignment)、sort() 運用


``` js
// 7. sort Exercise
// Sort the people alphabetically by last name

const people = ['Beck, Glenn', 'Becker, Carl', 'Beckett, Samuel', 'Beddoes, Mick', 'Beecher, Henry', 'Beethoven, Ludwig', 'Begin, Menachem', 'Belloc, Hilaire', 'Bellow, Saul', 'Benchley, Robert', 'Benenson, Peter', 'Ben-Gurion, David', 'Benjamin, Walter', 'Benn, Tony', 'Bennington, Chester', 'Benson, Leana', 'Bent, Silas', 'Bentsen, Lloyd', 'Berger, Ric', 'Bergman, Ingmar', 'Berio, Luciano', 'Berle, Milton', 'Berlin, Irving', 'Berne, Eric', 'Bernhard, Sandra', 'Berra, Yogi', 'Berry, Halle', 'Berry, Wendell', 'Bethea, Erin', 'Bevan, Aneurin', 'Bevel, Ken', 'Biden, Joseph', 'Bierce, Ambrose', 'Biko, Steve', 'Billings, Josh', 'Biondo, Frank', 'Birrell, Augustine', 'Black, Elk', 'Blair, Robert', 'Blair, Tony', 'Blake, William'];


/* 解法 */
const sortPeople = people.sort(function(a, b){
  const [aLast, aFirst] = a.split(', '); //解構賦值(Destructuring Assignment)
  const [bLast, bFirst] = b.split(', '); //解構賦值(Destructuring Assignment)

  if(aLast > bLast){
    return 1
  }else{
    return -1
  }
})

/* 簡化語法 */
const sortPeople = people.sort( (a, b) => {
    const [aLast, aFirst] = a.split(', '); //解構賦值(Destructuring Assignment)
    const [bLast, bFirst] = b.split(', '); //解構賦值(Destructuring Assignment)

    return aLast > bLast ? 1 : -1
})

console.log(sortPeople);
```

***
## data 題目

<span id="inline-toc">Q：</span>分別計算data內每個種類的數量

<span id="inline-toc">A：</span>練習 reduce() 運用

``` js 
// 8. Reduce Exercise
// Sum up the instances of each of these
const data = ['car', 'car', 'truck', 'truck', 'bike', 'walk', 'car', 'van', 'bike', 'walk', 'car', 'van', 'car', 'truck' ];

/* 解法 */
const dataCount = data.reduce( (counts, item) => {
    if(!counts[item]){
        counts[item] = 0;
    }

    counts[item] += 1;

    return counts;
}, {})

console.log(dataCount)
```