---
title: "How to Build a Web Crawler in Python - PTT as an example"
date: 2021-01-12
draft: false
categories: 
- Python Note
tags:
- Web Crawler
cover:
    image: "photo/ptt3.jpeg"
    relative: true
---

### 了解網路爬蟲的概念後，那我們就接著來實作看看吧！

<!--more-->


其實網路爬蟲的原理就是模擬我們操作網頁的行為，只是這過程許多行為都是透過瀏覽器幫我們完成的，因此我們才需要先了解瀏覽器運作的原理。
可以參考我上一篇的觀念介紹。

我們透過閱讀網頁的內容，尋找我們需要的資訊，爬蟲則是使用 requests 向網頁發送請求拿到網頁的所有資訊。我們再透過 BeautifulSoup 這個套件就可以針對網頁內容進行分析，以及抓取 HTML 的元素，將我們所需要的資訊從網路上抓取下來。

先前有介紹過使用這些套件的方法以及 requests 的用法，先來複習一下。

```python
# 先引入需要的套件
import requests
from bs4 import BeautifulSoup

# 向該網址發送 HTTP GET 請求並接收回應
# 取得該網頁所有資訊，並存入 resp 變數裡
base_url = "<https://www.ptt.cc>"
job_page = "/bbs/job/index.html" 
resp = requests.get(base_url + job_page)
```

當我們透過 requests.get() 拿到該網頁上的所有資訊時，可以先印出來確認伺服器是不是成功處理了請求：

```python
print(resp.status_code)
```

我們可以看到會印出了 HTTP 的狀態碼 200 ，這就表示請求成功，執行結果如下：

![](photo/ptt4.png)



### Python BeautifulSoup 語法小教室

當請求成功後，我們接著可以透過 BeautifulSoup 套件來解析網頁內容，要特別注意的是我們 requests.get() 的時候是拿到網頁的所有資訊，其中也包含 HTTP 的狀態碼，當我們在使用 BeautifulSoup 的時候則是針對 HTML 的內容，也就是 resp.text 進行解析。

```python
# 用 html.parser 分析 resp.text 並存入 soup 中
soup = BeautifulSoup(resp.text,'html.parser')
```

我們使用 html.perser 分析 resp.text 並存入 soup 變數，而 soup 就是解析完的結構樹物件。經過 BeautifulSoup 之後，也能夠幫助我們更好讀懂resp 的HTML ，可以嘗試列印 soup ，可以看到解析過後的 resp.text 如下：

![](photo/ptt5.png)

之後我們就可以透用解析後的 soup 來尋找我們的目標節點，並取得該節點的文字內容，用法如下：

```python
# 找到所有符合搜尋規則的目標節點
soup.findAll('標籤名稱','class名稱','id名稱')
# 找到單個符合搜尋規則的目標節點
soup.find('標籤名稱','class名稱','id名稱')
```

首先，我們先找出文章中所有工作的列表 rents：

```python
＃ 搜尋所有 html 中 class 為 r-ent 的目標
rents = soup.find_all(class_='r-ent')
```

接著，我們透過 for loop 將每一個工作列表 rent 的資訊與連結取出來：

```python
for rent in rents:                             
    job_title = rent.find(class_='title').a.string # 找到該列表的 title
    link = base_url + rent.find('a')["href"]       # 找到該列表裡的標籤 a 的分頁連結
    print(f"{job_title}: {base_url + link}")       # 善用字串格式化列印所有 title 與 link
```

job_title 即為每個 rent 的工作標題。可以參考下圖更容易理解，我們從每個 r-ent 中尋找 class 為 title 的標籤，在進一步找到 title 裡 a 標籤的字串，所找到的 job_title 也就是每個列表的工作標題。

![](photo/ptt6.png)

link 則是我們要點擊的工作分頁。 base_url 是 ptt 前段網址中固定的網域名稱，再加上每個 rent 中， a 標籤的連結即可連結到該工作的分頁。

因此我們即可印出所有的工作標題及該工作的分頁連結溜！

### 小補充

有時候會遇到 ptt 的某個文章被刪除或無法點閱，就會遇到程式 bug，這時候只要加個判斷式就可以解決上述問題。

```python
for rent in rents:
    title_href = rent.find(class_='title').a
    
    if title_href == None:
        continue
        
    job_title = rent.find(class_='title').a.string
    link = base_url + rent.find('a')["href"]
    print(f"{job_title}: {base_url + link}")
```

我們先確認被刪掉或無法點閱的文章是沒有連接的原因導致程式無法執行，如圖：

![](photo/ptt7.png)

接著將所有 title 的 a 存入 title_href ，若 title_href 為 None 時，我們使用 continue 跳過迴圈內 continue 後面的剩餘敘述，接著繼續執行下一次的迴圈運作，將所有可以點閱的工作標題及文章連結列印出來。

**完成的程式碼：**

```python
import requests
from bs4 import BeautifulSoup

base_url = "https://www.ptt.cc"
job_page = "/bbs/job/index.html"
resp = requests.get(base_url + job_page)

soup = BeautifulSoup(resp.text , 'html.parser')
rents = soup.find_all(class_='r-ent')

for rent in rents:
    title_href = rent.find(class_='title').a
    
    if title_href == None:
        continue
        
    job_title = rent.find(class_='title').a.string
    a = rent.find('a').get('href')
    link = base_url + rent.find('a')["href"]
    print(f"{job_title}: {base_url + link}")
```

執行結果：

![](photo/ptt8.png)
成功取出我們的目標內容之後，我們就可以透過檔案處理的方式將所要爬取的資料抓取下來。下次我們再來介紹關於 Python 檔案處理的部分吧！

## 結語

希望這篇教學能幫助正在入門爬蟲的你，也希望能幫助想將程式變成第二外語的你們(´・ω・`)/ 喜歡的話給我小小的掌聲，我會很開心的噢！