---
title: "How to embed Google Tag Manager?"
description: ""
date: 2021-09-26
draft: false
categories: 
- Marketing Analytics
tags:
- GTM
cover:
    image: "photo/cover2.jpeg"
    relative: true
---



## 一、前言

上篇我們介紹了如何講 GTM 的數據連接至 GA，這次讓我們來實作代碼的設定，來追蹤我們網站上想要追蹤的事件吧。在開始新增網頁上的 GTM 追蹤代碼之前，我們先來了解構成 GTM 追蹤代碼的三大元件：代碼、觸發條件、變數。

<!--more-->

### 1. 代碼
代碼是網頁或行動應用程式上執行追蹤事件的一段程式碼，在代碼管理工具中，大多是用來將追蹤資訊從的網站傳送到第三方，像是 Google Analytics 代碼或 Google Ads 轉換追蹤。

### 2.觸發條件
觸發條件定義了代碼如何被觸發，可使用內建的觸發條件，也可新增自己需要的觸發條件，進而設定該代碼在使用者與網站互動的特定情況與行為下被觸發。

### 3. 變數
變數代表了觸發的條件，定義了觸發條件的執行細節。也就是追蹤事件觸發程式代碼執行的時機，主要由變數（Variables）、運算符號（Operators）與值（Values）所組成。


## 二、設定追蹤代碼

首先，代碼會因「事件」而啟動，事件可以是載入網頁、點擊按鈕、捲動頁面等狀況。我們可以在 GTM 定義「觸發條件」來監聽事件，並指定代碼的啟動時機。

也就是說 GTM 會經由我們所定義的「觸發條件」來監聽網頁上的「事件」，當事件被完成時就會執行該「代碼」並將追蹤資訊傳送到我們的 GA 或其他第三方工具。設定步驟如下：

先點擊左側的代碼區塊，我們也可直接點選新增代碼，若直接點選新增代碼即可開始設定。

{{<figure src="photo/11.png" title="圖十一" align="center">}}

若是從左側的代碼區進入設定代碼的話，可以透過「新增」來我們追蹤的事件：

{{<figure src="photo/12.png" title="圖十二" align="center">}}

接著填入我們要追蹤的事件名稱，也就是該代碼名稱，並點選代碼設定：

{{<figure src="photo/13.png" title="圖十三" align="center">}}

這邊我們選擇代碼類型為「 Google Analytics：GA4 事件」，好讓我們想要追蹤的事件數據紀錄在 GA 中。

{{<figure src="photo/14.png" title="圖十四" align="center">}}

接著設定代碼為我們一開始設置的 GA4 設定，將我們事件的數據傳送到我們的 GA 帳戶中。

{{<figure src="photo/15.png" title="圖十五" align="center">}}

再輸入該事件名稱，若有需要可以在新增事件參數、使用者屬性、進階設定等。而該事件名稱會被記錄在我們的 GA 報表的數據中，可以參考圖十七的 GA 畫面。

{{<figure src="photo/24.png" title="圖十六" align="center">}}
{{<figure src="photo/25.png" title="圖十七" align="center">}}

## 三、觸發條件

觸發條件定義了代碼如何被觸發，接著來設定該代碼要觸發的條件。我們可以選擇內建的觸發條件，或是選擇我們之前已經建立的觸發條件(每次新建立的觸發條件都會被記錄在這個清單)，也可按「+」來新增觸發條件的設定。

{{<figure src="photo/16.png" title="圖十八" align="center">}}

但目前鮮有的觸發條件內容不是我們要的，因此我們新增觸發條件，透過點選觸發條件設定來選擇想要觸發條件的類型，也就是我們想要監聽網站上的事件內容，那這邊我選擇「點擊」區域的「所有元素」。

{{<figure src="photo/18.png" title="圖十九" align="center">}}

實際上，這些觸發條件類型又分為網頁瀏覽、點擊、使用者參與、其他等四大類型，下面列出比較常見的類型給大家參考：

**1. 網頁瀏覽**

- 網頁瀏覽
- DOM 就緒
- 視窗已載入

**2. 點擊**

- 僅連結
- 所有元素

**3. 使用者參與**

- Youtube 影片
- 元素可見度
- 捲動頁數
- 表單提交

**4. 其他**

- JavaScript 錯誤
- 自訂事件
- 計時器
- 記錄變更

選擇「點擊」區域的「所有元素」後，接著設定觸發條件名稱叫「點擊 Post」，觸發條件的名稱可以根據所設定的條件來命名。那我這邊設置啟動時機為部分點擊。


## 四、變數

變數代表了觸發的條件，定義了觸發條件的執行細節。根據變數（Variables）、運算符號（Operators）與值（Values）所組成。

為了記錄點擊 Post 按鈕的次數，我將變數設為內建變數的 Click Element。

{{<figure src="photo/19.png" title="圖二十" align="center">}}

{{<figure src="photo/20.png" title="圖二一" align="center">}}

並設定運算符號為 Click Element 符合 CSS 選取器，也就是我們目標元素的 class name 或是 id name 時觸發，目標元素的 class name 或是 id name 就是我們的值。

{{<figure src="photo/21.png" title="圖二二" align="center">}}

除了內建變數外，我們也可以設定其他自訂變數，只要根據我們的需求來設定變數、運算符號、值就可以。我們也可以設定多個變數的內容，只要有事件發生且這些條件全部都符合時，就會啟用這項觸發條件。

{{<figure src="photo/23.png" title="圖二三" align="center">}}


儲存完成後就完成我們代碼設定溜！
最後再透過 Debug model 來測試我們的代碼是否能夠成功在我們的網站被成功觸發就 ok 完成了！
