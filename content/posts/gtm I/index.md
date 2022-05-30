---
title: "Connect Google Tag Manager to Google Analytics"
description: ""
date: 2021-09-25
draft: false
categories: 
- Marketing Analytics
tags:
- GTM
cover:
    image: "photo/cover1.jpeg"
    relative: true
---


## 一、認識 GTM

GTM 是一套代碼管理的平台，主要是用來追蹤網站上各個元素所發生的事件，並管所有追蹤的程式碼之平台，簡單來說，透過 GTM，我們更可以更容易的追蹤網站上的特定內容並設定追蹤的程式碼，或是置入第三方應用工具來輔助分析網站，並幫助我們能妥善管理所有在網站上設置的程式碼，再透過分析工具如 Google Analytics，使我們能夠更了解網站上的使用者行為，進而改善與優化網站上的內容，對使用者進行再行銷。
<!--more-->

## 二、帳戶及容器建立

### 1. **建立一個 GTM 帳號**

在設定 GTM 追蹤代碼之前，我們要先建立一個 GTM 帳號。

{{<figure src="photo/1.png" title="圖一" align="center">}}


### 2. **在帳號內設定容器**

接著，在我們的帳號內設定容器，分別填入帳戶名稱和容器名稱，並選擇目標廣告平台。
一個帳號可以設定一至多個容器，而每個容器對應至一個網域。

{{<figure src="photo/2.png" title="圖二" align="center">}}



### 3.**確認使用 GTM 條款**

按下建立即可建立我們的容器。

{{<figure src="photo/3.png" title="圖三" align="center">}}


### 4.**設定追蹤網站**

通常第一次建立容器即會跳出「安裝 Google 代碼管理工具」的視窗(圖四)，或著我們也可以在管理標籤找到「安裝 Google 代碼管理工具」(圖五)。
接著將安裝 Google 代碼管理工具，分別將兩段程式貼至我們的 ```<head>``` 與 ``` <body>```。

{{<figure src="photo/4.png" title="圖四" align="center">}}

{{<figure src="photo/5.png" title="圖五" align="center">}}



## 三、連結 GA

完成上述安裝之後，我們就可以回到工作區來設置追蹤代碼。我們首先需要設定我們的 GTM 與 GA 帳連通，這樣我們即可在 GA 查看根據我們設定的代碼的數據。


### 1. 先點擊新增代碼

點選新增代碼就可以開始設定我們的代碼。

{{<figure src="photo/6.png" title="圖六" align="center">}}

### 2. 輸入代碼名稱

接著以輸入代碼名稱，這邊我輸入 GA4 設定，並接續設定代碼設定與觸發條件。

{{<figure src="photo/7.png" title="圖七" align="center">}}



### 3. 新增代碼設定

接著在代碼設定的區塊，我們可以選擇代碼的類型，為了能與 GA 帳戶聯通，我們可以選擇「通用 Analytics (分析)」或是「GA4 設定」。
這邊說明一下「通用版 Analytics」及新版「 GA4」 設定主要的差異：

#### 1) 通用 GA 

事件(通用 Analytics) 預設有 4 個欄位，分別為事件類別、事件動作、活動標籤與價值，並可以依照需求新增自訂維度或自訂指標欄位，儲存額外資訊。


#### 2) 新版 GA4

預設只有 1 個欄位(事件名稱)，其餘所有欄位(包含維度、指標、自訂維度與自訂指標)全部合併為事件參數欄位。

{{<figure src="photo/8.png" title="圖八" align="center">}}


我們的實作及介紹以新版的 GA4 設定為主。接著我們將GA 的 ID 填入至評估 ID 。我們可以選擇性的新增進階設定，這邊我將代碼觸發優先順序設定為999，優先級越高的代碼會越先啟動。如果沒有指定優先級，則預設值為 0。

{{<figure src="photo/9.png" title="圖九" align="center">}}

### 4.新增觸發條件

完成上面的代碼設後，接著是設定 GTM 的觸發條件。觸發條件代表網站在何種狀況底下，會觸發 GTM 也執行我們所設定的代碼。

{{<figure src="photo/10.png" title="圖十" align="center">}}

這邊因為我們需將所有的網站行為導入至 Google Analytics，因此這邊設定 All Pages，也就是瀏覽任一頁面能都觸發使用 GA 代碼紀錄，這樣就完成 GTM 的前置作業了！


因為篇幅的關係，還想瞭解更多的朋友可以接著看實作二喲！