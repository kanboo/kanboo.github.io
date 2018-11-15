---
title: WebTracking
date: 2018-11-14 22:50:27
categories:
  - 網頁追蹤
tags:
  - 網頁追蹤
  - GA
  - FB
---

{% cq %}

{% asset_img webanalytics.jpg %}

<font style="font-size:20px;">整理 網頁追蹤與分析 筆記</font>

{% endcq %}

<!-- more -->

---

## FB UTM 工具

### 觸及的網址

網址範例：

```
https://kanboo.github.io/
?utm_source=facebook
&utm_medium=article
&utm_campaign=shareInfo
```

### 說明

- utm_source：從哪個網站過來，如：FB、Google、Blog…等
- utm_medium：透過什麼方式，如：banner、文章分享、關鍵字廣告…等
- utm_campaign：此次活動名稱

> [FB UTM 產生器](https://www.facebook.com/business/google-analytics/build-your-url)

---

## 轉換與行為

轉換(從行為 → 漏斗 → 轉換)

1. 希望使用者在網頁上「<font color="red">最終完成的行為</font>」
2. 以此訂定你的「漏斗流程」

### 範例說明

1. A 電商：完成「結帳」行為。
2. B 電商：
   2.1 有完成「加入購物車」行為，透過分析此行為達到一個量(1 千人)時
   2.2 再順勢推出「優惠活動」，將 User 推向結帳行為。
3. AWS：完成一個「注冊流程」。

{% asset_img webanalytice_1.png %}

> Faceboox：[FB 標準事件程式碼](https://www.facebook.com/business/help/402791146561655?helpref=faq_content)

---

## Mixpanel

{% asset_img webanalytice_2.png %}

> [測試網站](https://codepen.io/Kanboo/pen/pOWeOK) > [官方網站](https://mixpanel.com)

### 設計轉換漏斗

選擇「Funnels」→ 建立新 Funnel → 設定篩選條件

{% asset_img webanalytice_3.png %}

### 篩選條件

{% asset_img webanalytice_4.png %}

---

## FB 追蹤設定

將我們所有客戶的追蹤資料，依據不同的條件，分類出不同的類別出來，
籍此篩選出具有「<font color="red">更高潛在價值</font>」的客戶清單，不但能更有效的發揮廣告效益，
也讓每筆廣告費用花在刀口上，而不是亂槍打鳥。

{% asset_img webanalytice_5.png %}

### 建立及設定 FB Pixel

#### 1. 建立

{% asset_img webanalytice_6.png %}

#### 2. 選擇「手動安裝」

{% asset_img webanalytice_7.png %}

#### 3. 複製追蹤程式碼，嵌入在頁面

{% asset_img webanalytice_8.png %}

#### 4. 可新增額外「追蹤資訊」

{% asset_img webanalytice_9.png %}

- 不同的追蹤事件

{% asset_img webanalytice_10.png %}

- 可夾帶額外資訊

{% asset_img webanalytice_11.png %}

---

### 驗證是否綁定成功

在 `head` 處，嵌入 `facebook 追蹤碼` 後，
額外新增一事件，如： `fbq('track', 'ViewContent');` ，
再去 Facebook 像素 檢查是否有紀錄顯示。

{% label danger@註： %} 剛觸發追蹤程式碼時，Facebook 不會即時顯示紀錄，須稍待幾分鐘後，才會有最新紀錄顯示。

追蹤程式碼

{% asset_img webanalytice_12.png %}

Facebook 像素 紀錄顯示畫面

{% asset_img webanalytice_13.png %}

---

### 追蹤事件與參數設定

Facebook 像素追蹤碼，點擊「設定」後，再點「自行手動安裝像素程式碼」

{% asset_img webanalytice_14.png %}

再來直接點「繼續」至第二步

{% asset_img webanalytice_15.png %}

有一些基本的追蹤選項可選擇

{% asset_img webanalytice_16.png %}

有些追蹤項目，還可填入額外的參數進去，填完以後，點擊下面複製區一下後，即可將程式碼嵌入我們的頁面或動作裡。

{% asset_img webanalytice_17.png %}

另外也可參照 FB 標準事件程式碼 的範例，如下

{% asset_img webanalytice_18.png %}

直接依照 Facebook 標準的規則，自行撰寫程式碼，
完成後，可在結帳頁面或動作時，觸發此段程式碼

```javascript=
<script>
  // 購買追蹤事件
  fbq('track', 'Purchase', {
    value: 2000,
    currency: 'TWD',
  });
</script>
```

偵測結果，也可點擊「查看詳細資料」，查看帶入的參數值。

{% asset_img webanalytice_19.png %}

> Faceboox：[FB 標準事件程式碼](https://www.facebook.com/business/help/402791146561655?helpref=faq_content)

---

### 如何自訂事件

有時除了追蹤登入「頁面的事件」外，也希望能追蹤到 User 的一些「行為事件」，
如：下載產品 DM 的 PDF 動作，不過像此類事件，FB 標準事件程式碼裡並無提供，
所以我們就需要「自訂事件」來追蹤此行為，籍此來篩選出具有更高潛在客戶清單。

自行輸入預追蹤的字眼即可

```javascript=
// 自訂事件 - 下載PDF行為
fbq('track', 'downloadPDF');

// 自訂事件 - 註冊行為
fbq('track', 'registerStep');
```

> Faceboox：[FB 標準事件程式碼](https://www.facebook.com/business/help/402791146561655?helpref=faq_content)

---

### 建立廣告受眾

要篩選出具有「更高潛在價值」的客戶的話，我們就需下些條件來獲得更精確的資料。

{% asset_img webanalytice_20.png %}

建立「廣告受眾」

{% asset_img webanalytice_21.png %}

{% asset_img webanalytice_22.png %}

下列範例為「篩選出有進註冊頁面，但尚未完成註冊人員」

{% asset_img webanalytice_23.png %}

> [FB 廣告受眾服務](https://www.facebook.com/login.php?next=https%3A%2F%2Fwww.facebook.com%2Fads%2Fmanager%2Faudiences%2F)

---

### 建立類似廣告受眾

#### 1. 選擇哪個當基底

{% asset_img webanalytice_24.png %}

#### 2. 建立類似廣告受眾

{% asset_img webanalytice_25.png %}

#### 3. 設定條件

{% asset_img webanalytice_26.png %}

#### 4. 完成畫面

{% asset_img webanalytice_27.png %}

> [FB 廣告受眾服務](https://www.facebook.com/login.php?next=https%3A%2F%2Fwww.facebook.com%2Fads%2Fmanager%2Faudiences%2F)

---

### 廣告系統介紹

<span id="inline-blue">情境說明</span>

銷售產品為「手機」，而我們可將客戶區分為「男生、女生」二類型，
再根據這二種類型，設計不同的廣告內容，讓廣告能更觸動人力，提高客戶的購買欲望，
另外也可分析出不同的文案下，哪個廣告效益較好(埋 UTM)，用來當下次的參考資料。

{% asset_img webanalytice_28.png %}

<span id="inline-blue">廣告管理員</span>

{% asset_img webanalytice_29.png %}

在「行銷活動」，點擊「建立」

{% asset_img webanalytice_30.png %}

點擊「選擇引導式建立流程」

{% asset_img webanalytice_31.png %}

選擇「轉換次數」

{% asset_img webanalytice_32.png %}

輸入「行銷活動名稱」，點擊「設定廣告帳號」

{% asset_img webanalytice_33.png %}

設定相關資訊後，點擊「繼續」

{% asset_img webanalytice_34.png %}

輸入「廣告組合名稱」，選擇想要使用者最終想達成的「目標」

{% asset_img webanalytice_35.png %}

畫面往下滑動，挑選出主要的廣告受眾

{% asset_img webanalytice_36.png %}

設定預花費的廣告金額

{% asset_img webanalytice_37.png %}

> [FB 廣告平台服務](https://www.facebook.com/adsmanager/manage/campaigns?act=322712334954089#_=_)

---

### FB 廣告優化流程

先訂出想要<font color="red">追蹤的過程及最終目標</font>

{% asset_img webanalytice_38.png %}

優化過程是須「<font color="red">循序漸近</font>」的，用案例來說明的話

一個商品或網站一開始，一定沒有各種的追蹤事件紀錄，
如：加入購物車、結帳、註冊…等紀錄，
所以我們一開始只能先從「瀏覽商品資訊」的客戶下廣告，
當運行一段時間後，追蹤紀錄陸續會累積「註冊、結帳…」等，
籍此我們可以再利用這些數據，產生另一組的「類似廣告受眾」出來，
再針對這些「類似廣告受眾」下廣告，進而擴大我們的銷售範圍或客戶群。

{% asset_img webanalytice_39.png %}

利用既有的受眾，產生另一組「類似廣告受眾」

{% asset_img webanalytice_40.png %}

> [FB 轉換功能介紹](https://www.facebook.com/business/a/custom-conversions)

---

### 自訂轉換流程

可額外設定不在 Facebook 標準事件範圍裡的條件，選擇「設定新的自訂轉換」。

{% asset_img webanalytice_41.png %}

選擇「自訂事件」

{% asset_img webanalytice_42.png %}

填入「名稱」、選擇「類別」，至於「類別」就挑選一個比較符合的選項挑選，最後點擊「建立」。

{% asset_img webanalytice_43.png %}

成功建立一事件

{% asset_img webanalytice_44.png %}

這時再次選擇時，就會出現我們剛新增的「自訂事件」名稱

{% asset_img webanalytice_45.png %}

另外也可在「事件管理工具 → 像素 → 自訂轉換」，
看到我們新增的「自訂事件」

{% asset_img webanalytice_46.png %}

可測試看看，是否此追蹤事項是否有生效

{% asset_img webanalytice_47.png %}

若確定生效後，就可以在「廣告管理員」去針對「自訂轉換」建立廣告。

{% asset_img webanalytice_48.png %}

---

### FB 分析建立客製化漏斗

利用現有的數據來分析，是否哪個階段可優化或改善，將每個階段的客戶停留率再提高，進而達到最終目標行為(結帳、註冊)。

開啟「FB Analytics」介面

{% asset_img webanalytice_49.png %}

選擇「分析項目」

{% asset_img webanalytice_50.png %}

點選「建立漏斗」

{% asset_img webanalytice_51.png %}

選擇「新增漏斗步驟」，挑選事件

{% asset_img webanalytice_52.png %}

新增第二個項目，點擊後面的「+」號，就可再挑事件

{% asset_img webanalytice_53.png %}

也可設定時間的範圍

{% asset_img webanalytice_54.png %}

完成設定的結果

{% asset_img webanalytice_55.png %}

最後可將此設定「儲存」，可日後可再查看。

{% asset_img webanalytice_56.png %}

{% asset_img webanalytice_57.png %}

> FB：[分析連結](https://www.facebook.com/login.php?next=https%3A%2F%2Fwww.facebook.com%2Fanalytics)

### FB 分析-營業額與顧客終生價值

也可觀看「收入」的分析報告。

{% asset_img webanalytice_58.png %}

> FB：[分析連結](https://www.facebook.com/login.php?next=https%3A%2F%2Fwww.facebook.com%2Fanalytics)

---

## Google Analytics

學習如何收集資料，進而分析出有參考性的資訊

1. 自訂事件與轉換
2. 整合訂單數據
3. 拉出受眾，結合廣告系統

{% asset_img webanalytice_59.png %}

### 開啟電子商務增強版

{% asset_img webanalytice_60.png %}

{% asset_img webanalytice_61.png %}

確認有「購物行為、結帳行為」二個選項，才算有開啟成功。
{% asset_img webanalytice_62.png %}

---

### GA 事件

操作方式跟之前埋 FB 追蹤程式碼很像，此外一樣有分「內建」和「自建」的追蹤事件

1. 內建事件 (電子商務、增強型電子商務)
2. 自訂事件 (下載電子書、觀看影片)

```javascript=
// 格式
gtag('event', '<event_name>', {<optional_event_params>})

// 實際寫法
gtag('event', 'sign_up', {
  'method' : 'Google'
});
```

<div class="note info">官方文件：
[gtag.js 事件參考和參數參考](https://developers.google.com/gtagjs/reference/event?hl=zh-cn)
[gtag.js 收集增強型電子商務數據](https://developers.google.com/analytics/devguides/collection/gtagjs/enhanced-ecommerce?hl=zh-cn)
[GA Chrome 偵測工具](https://chrome.google.com/webstore/detail/tag-assistant-by-google/kejbdjndbnbjgmefkgdddjlbokphdefk?hl=zh-TW)
</div>

---

### 購物行為事件名稱

- 觀看產品：view_item
  - [跟踪商品詳情查看](https://developers.google.com/analytics/devguides/collection/gtagjs/enhanced-ecommerce?hl=zh-cn#track_a_product_details_view)
  - [必填欄位](https://developers.google.com/analytics/devguides/collection/gtagjs/enhanced-ecommerce?hl=zh-cn#impression_data)
- 加入購物：add_to_cart
  - [添加到購物車](https://developers.google.com/analytics/devguides/collection/gtagjs/enhanced-ecommerce?hl=zh-cn#track_additions_to_and_removals_from_shopping_cart)
- 開始結帳：begin_checkout
  - [結帳操作](https://developers.google.com/analytics/devguides/collection/gtagjs/enhanced-ecommerce?hl=zh-cn#track_checkout)
- 購買成功：purchase
  - [購買情況](https://developers.google.com/analytics/devguides/collection/gtagjs/enhanced-ecommerce?hl=zh-cn#track_purchases)

持續收集上述的使用者紀錄，當累積到一定的基數後，就可以分析出可靠性的參考數據，進而決策下一個走向或功能

{% asset_img webanalytice_63.png %}

<div class="note info">官方文件：
[gtag.js 事件參考和參數參考](https://developers.google.com/gtagjs/reference/event?hl=zh-cn)
[gtag.js 收集增強型電子商務數據](https://developers.google.com/analytics/devguides/collection/gtagjs/enhanced-ecommerce?hl=zh-cn)
</div>

---

### viewcontent 瀏覽商品設定

紀錄使用者在瀏覽商品項目時，當進某商品頁面時，
我們可以「詳細紀錄」使用者瀏覽哪個商品。

```javascript=
gtag('event', 'view_item', {
  items: [
    {
      id: 'P12345',
      name: 'Android Warhol T-Shirt',
      list_name: 'Search Results',
      brand: 'Google',
      category: 'Apparel/T-Shirts',
      variant: 'Black',
      list_position: 1,
      quantity: 2,
      price: '2.0'
    }
  ]
});
```

補充：關於「展示數據」，「id、name」參數為必填，其餘的為選填。

{% asset_img webanalytice_64.png %}

<div class="note info">官方文件
跟踪商品詳情查看：[view_item](https://developers.google.com/analytics/devguides/collection/gtagjs/enhanced-ecommerce?hl=zh-cn#track_a_product_details_view)
增強型電子商務：[展示數據](https://developers.google.com/analytics/devguides/collection/gtagjs/enhanced-ecommerce?hl=zh-cn#impression_data)
</div>

---

### add_to_cart 加入購物車設定

當使用者將產品加入購物車時，觸發 GA 去紀錄使用者的產品資訊。

{% asset_img webanalytice_65.png %}

```javascript=
// 監聽 加入購物車 按鈕
const btn_addCart = document.querySelector('#addCart');

btn_addCart.addEventListener('click', function(e) {
  gtag('event', 'add_to_cart', {
    items: [
      {
        id: 'P12345',
        name: 'Android Warhol T-Shirt',
        list_name: 'Search Results',
        brand: 'Google',
        category: 'Apparel/T-Shirts',
        variant: 'Black',
        list_position: 1,
        quantity: 2,
        price: '2.0'
      }
    ]
  });
});
```

補充：關於「商品數據」，「id、name」參數為必填，其餘的為選填。

{% asset_img webanalytice_66.png %}

<div class="note info">官方文件
[跟踪將商品添加到購物車或從購物車中移除商品的操作](https://developers.google.com/analytics/devguides/collection/gtagjs/enhanced-ecommerce?hl=zh-cn#track_additions_to_and_removals_from_shopping_cart)
[商品數據](https://developers.google.com/analytics/devguides/collection/gtagjs/enhanced-ecommerce?hl=zh-cn#product_data)
</div>

---

### begin_checkout 結帳行為設定

當使用者開始進行結帳時，可紀錄目前的資訊，
若結帳需經過「多次的操作」的話，也可紀錄使用者操作到哪個頁面，
而放棄最終結帳行為，進而分析優化結帳流程。

```javascript=
// 結帳頁面
gtag('event', 'begin_checkout', {
  items: [
    {
      id: 'P12345',
      name: 'Android Warhol T-Shirt',
      list_name: 'Search Results',
      brand: 'Google',
      category: 'Apparel/T-Shirts',
      variant: 'Black',
      list_position: 1,
      quantity: 2,
      price: '2.0'
    }
  ],
  coupon: ''
});
```

> [跟踪結帳操作](https://developers.google.com/analytics/devguides/collection/gtagjs/enhanced-ecommerce?hl=zh-cn#track_checkout)

---

### purchase 購買事件設定

紀錄最終的結帳資訊，以及購買產品清單。

```javascript=
gtag('event', 'purchase', {
  transaction_id: '24.031608523954162',
  affiliation: 'Google online store',
  value: 23.07,
  currency: 'USD',
  tax: 1.24,
  shipping: 0,
  items: [
    {
      id: 'P12345',
      name: 'Android Warhol T-Shirt',
      list_name: 'Search Results',
      brand: 'Google',
      category: 'Apparel/T-Shirts',
      variant: 'Black',
      list_position: 1,
      quantity: 2,
      price: '2.0'
    },
    {
      id: 'P67890',
      name: 'Flame challenge TShirt',
      list_name: 'Search Results',
      brand: 'MyBrand',
      category: 'Apparel/T-Shirts',
      variant: 'Red',
      list_position: 2,
      quantity: 1,
      price: '3.0'
    }
  ]
});
```

補充：關於「操作數據」，「transaction_id」參數為必填，其餘的為選填。

{% asset_img webanalytice_67.png %}

<div class="note info">官方文件
[跟踪購買情況](https://developers.google.com/analytics/devguides/collection/gtagjs/enhanced-ecommerce?hl=zh-cn#track_purchases)
[操作數據](https://developers.google.com/analytics/devguides/collection/gtagjs/enhanced-ecommerce?hl=zh-cn#action_data)
</div>

---

### 客制自訂事件

```javascript=
// 格式
gtag('event', <action>, {
  'event_category': <category>,
  'event_label': <label>,
  'value': <value>
});
```

```javascript=
// 實際範例：
gtag('event', '下載電子書', {
  event_category: '點擊連結',
  event_label: '下載行銷密技電子書',
  value: 10
});
```

- `<action>`是在 Google Analytics（分析）事件報告中顯示為事件操作的字符串。
- `<category>`是顯示為事件類別的字符串。
- `<label>`是顯示為事件標籤的字符串。
- `<value>`是一個顯示為事件價值的非負整數。

> 跟踪 Google Analytics（分析）事件：[GA 自訂事件](https://developers.google.com/analytics/devguides/collection/gtagjs/events?hl=zh-cn)

---

### 設定目標 - 程序視覺呈現

若想要額外分析出不同的資料的話，如：完成註冊、下載電子書…等，
其他的數據分析的話，就可使用「目標」來篩選出我們要的報表。

{% asset_img webanalytice_68.png %}

#### 操作流程

下列以分析「完成註冊之依頁面瀏覽順序」的案例，進行操作

1. 設定 → 選擇「目標」

{% asset_img webanalytice_69.png %}

2. 選擇「新增目標」後

{% asset_img webanalytice_70.png %}

3. 選擇「自訂」，點擊「繼續」

{% asset_img webanalytice_71.png %}

3. 輸入「名稱」、選擇「類型」後，按「繼續」

{% asset_img webanalytice_72.png %}

4. 目標詳情

實際連結目標 => 輸入最終目標的頁面網站

不過下列有額外打開「程序」此選項，原因為進行「註冊行為」時，
使用者不可直接到達最終的註冊完成頁面，
一定要完成前面的「填寫個人資料」這步驟後，
才會有可能達成「完成註冊」行為，
所以才需設定「程序」這層判斷，讓數據能更精確。

{% asset_img webanalytice_73.png %}

5. 完成目標設定

{% asset_img webanalytice_74.png %}

6. 查看「程序視覺呈現」

{% asset_img webanalytice_75.png %}

---

### 從事件設定轉換目標

1. 第一步選「自訂」，輸入「名稱」，選擇「事件」

{% asset_img webanalytice_76.png %}

2. 填寫「事件條件」

{% asset_img webanalytice_77.png %}

補充說明：

上述需填寫的事件條件，等同是我們埋 GA 事件的參數，下圖為「對應關係」

{% asset_img webanalytice_78.png %}

3. 完成目標設定

{% asset_img webanalytice_79.png %}

---

### 多管道程序

觀看使用者是透過何種方式到達我們網站，如：社群、Gogole 搜尋、Blog…等

{% asset_img webanalytice_80.png %}

---

### 從 UTM 觀看轉換成效

選擇「來源/媒介」，再搭配選擇「轉換」選項後，可以用來分析從哪個地方來的使用者，達成的轉換率(註冊、結帳)比較高，我們就可以嘗試增加廣告費用，來測試轉換率是否還會再提高。

{% asset_img webanalytice_81.png %}

<div class="note info">其他參考文章：
[Google analytics 事件追蹤、轉換目標設定教學](https://ianchuu.com/2018/04/25/gaevents/index.html)
[超詳細 GA 網站分析入門大全](https://pickydigest.com/digital-marketing/google-analytics-getting-started/)
[Google Tag Manager Micro Challenge](https://ithelp.ithome.com.tw/users/20107582/ironman/1471)
</div>

---

## 分流短網址服務

> 官方連結：https://lihi.io/

### 活動網址

<span id="inline-blue">情境說明</span>

假設我有一個活動公告，要發送給所有的客戶，
而第一次發佈(簡訊)時，發現不小心填錯活動網址，
這時要回收這些訊息，已無法達成，
所以我們可以透過「Lihi 短網址」功能，
發送「短網址」給客戶，所以更正為正確網址，
此時「短網址」也會自動導向正確的網址。

<span id="inline-yellow">範例</span>

可注意下列紅框的部份(短網址)，都是一樣的，
而藍框的部份(網址)，則長的不一樣。

- 第一次發佈網址

{% asset_img webanalytice_82.png %}

- 第二次更改後網址

{% asset_img webanalytice_83.png %}

### lihi 之參數功能

參數說明

{% asset_img webanalytice_84.png %}

設定結果

{% asset_img webanalytice_85.png %}

### A/B 測試

設定 A/B 入口網址

{% asset_img webanalytice_86.png %}

後續可再追蹤，由哪個入口進來的成效較優

{% asset_img webanalytice_87.png %}

---

## 四張前端必看的 GA 報表

前端常用報表

<span id="inline-toc">1.</span> 特定頁面在瀏覽器上的跳出率與停留時間

- 位置：行為 > 網站內容 > 內容深度分析
- 維度：瀏覽器
- 原因：觀察瀏覽器是否有影響客戶的跳出率
- 範例：
  - 購物車頁面：safari 的跳出率比其他瀏覽器高 20%，原來是某個 JS 沒寫好
  - 結帳頁面：為什麼 IE 的停留時間都不到 10 秒，原來有 BUG 導致跳轉

{% asset_img webanalytice_88.png %}

<span id="inline-toc">2.</span> 響應式網頁的使用體驗查詢

- 位置：目標對象 > 行動裝置 > 總覽
- 維度：螢幕解析度
- 原因：以說服客戶你所開發的前端介面，在平均工作階段時間有滿足絕大部分的需要
- 範例：
  - 查詢到在 iPad 的平均工作時段較低，原來是響應式沒有寫好
  - 觀察流量與轉換率的比重

注意轉換率：

- 100 個人來到你的網站
  - 大於 2% 可撐起一間公司
  - 大於 3% 公司存活率
  - 5~10% 狂

{% asset_img webanalytice_89.png %}

<span id="inline-toc">3.</span> 網頁效能查詢

- 位置：行為 > 網站速度 > 總覽
- 維度：無
- 原因：能使用速度說服客服你的網頁效能平均值
- 範例：
  - 當客戶和你要網站平均載入時，可以撈此表的資料

{% asset_img webanalytice_90.png %}

<span id="inline-toc">4.</span> 特定系統上的瀏覽器是否有問題

- 位置：目標對象 > 技術 > 瀏覽器與作業系統
- 維度：主要維度(作業系統) + 次要維度 (瀏覽器)
- 原因：以方便你觀察特定系統是否再各數據有明顯落差
- 範例：
  - 發現到 android 的 firefox 版跳出率很高
  - Linux 的 Chrome 平均工作階段時間很低

解讀數據四大面向

- 目標對象：顧客面貌
  - 分析出主要客群
- 客戶開發：來源從哪裡來的？
  - 直接連結
  - 廣告
  - 搜尋
  - 社群
  - 推薦連結(參照連結->如：Blog 上的分享網址、葉配文、Youtube…等)
- 行為：顧客都在上面做哪些事情？
- 轉換：是否有達成你訂定的目標？

行銷與營運人員想瞭解的報表

- 每個來源加入購物車、結帳的轉換率
- 每個產品的業績，與客戶特性
- 各載具的結帳、購買轉換率

<span id="inline-purple">重點字句</span>

- 100 個人來到你的網站，超過 3%的「轉換率」，可撐起一家公司
- 從 100% 的顧客中，尋找出 10% 願意付費的使用者

> [共筆文章](https://quip.com/hpjnAK1zuyvI)

---

## GTM

> 六角：[GTM](https://quip.com/NeY7AQadzWsQ)
> 搞搞就懂：[透過 GTM 對網站進行 GA 追蹤](https://dotblogs.com.tw/wasichris/2018/09/28/012643)

###### tags: `GA`、`FB`
