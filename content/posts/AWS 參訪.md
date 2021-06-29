---
title: "AWS 參訪"
date: 2021-06-29
draft: true
categories: 
- 
tags:
- ML
- Python
---
### 心得：

這此參訪AWS 幫助我們更深入了解AWS的雲端服務及雲端運算的相關知識，對於正在準備Azure-900 考證的我們來說，提供很棒的觀念統整與觀念複習，更值得一提的是，透過講師的分享幫助我更深入的探索雲端雲算的世界，也幫助我在做知識整理的過程，更快速的將相關的資訊概念和雲端知識做連結，也認識到Docker和Kubernetes與Container之間的關係，以及Microservice 基於 Container 所發展出來的概念。

雖然Azure 做為微軟公司提供的雲端運算服務，與AWS 的雲端服務有異曲同工之處，但仍與AWS 的雲端平台有許多的不同，透過講師用心的分享也讓我更清楚了解AWS與Azure雲端服務各自的優劣。

其中，我認為非常有趣的是AWS在雲端服務上提供非常豐富的工具和產品，其透過本身的母公司Amazon作為產品試驗者，讓AWS不間斷地發產出創新且具價值的雲端服務給合作的夥伴們，這也使得AWS在公有雲的市場上居於主導的地位。

這次得參訪不僅讓我對於雲端的世界大開眼界，也讓我深刻的體會到AWS秉持的「IT IS STILL DAY ONE」的理念，很喜歡AWS 全球創新計劃主管 Dan Slater所說的一句話：' "Day 1" 心態是一種文化和營運模式，其將客戶置於 Amazon 工作的中心。將 "Day 1" 付諸實務依賴於長期關注、痴迷客戶和大膽創新。我想這也是AWS 持續創新的一個秘訣，在AWS的業師的身上也讓我看見業師們在面對工作一個很棒的心態，我想這也是很值得學習的一件事。

This visit to AWS helps us to understand AWS cloud services and cloud computing knowledge more deeply. For us who are preparing for Azure-900 research, it provides great concept integration and concept review. What is more worth mentioning is , Through the sharing of lecturers, it helped me to explore the world of cloud computing more deeply, and also helped me in the process of knowledge organization, to link related information concepts and cloud knowledge more quickly, and also realized that Docker, Kubernetes, and Container The relationship between and the concept developed by Microservice based on Container.

Although Azure, as a cloud computing service provided by Microsoft, is similar to AWS's cloud services, it still has many differences from AWS's cloud platform. Through the sharing of the instructors, I have a clearer understanding of AWS and Azure cloud services. Their pros and cons.

Among them, what I think is very interesting is that AWS provides a wealth of tools and products on cloud services. It uses its parent company Amazon as a product tester, allowing AWS to continuously produce innovative and valuable cloud services for cooperation. Partners, this also makes AWS occupy a dominant position in the public cloud market.

This visit not only opened my eyes to the cloud world, but also gave me a deep understanding of the concept of "IT IS STILL DAY ONE" that AWS upholds. I really like what Dan Slater, the director of AWS's global innovation program, said: ' The "Day 1" mentality is a culture and business model that puts customers at the center of Amazon's work. Putting "Day 1" into practice relies on long-term attention, obsession with customers, and bold innovation.

### 雲端服務介紹：

**Amazon** 

- 為零售業 省掉很多經銷商
- 像wearmarket 、Costco
- 主要收入來源為AWS

 ＊AWS 為B2B 而非科技公司沒有 CRM

**AWS**

- 連續10年 雲端服務leader
- AWS 作為雲端服務/作為創新平台
- 客戶導向-pull 先考慮客戶需要什麼功能 (有別於如微軟-push)
- 研發許多創新服務如推薦系統
- 將Amazon當白老鼠等技術成熟再放入AWS的服務給客戶（像樂高一樣）

**Amazon Web Services (AWS)** 是全球最全面和廣泛採納的雲端平台，透過全球資料中心提供超過 200 項功能完整的服務。數百萬個客戶 —包括成長最快的新創公司、最大型企業以及領先的政府機構—都使用 AWS 來降低成本、變得更靈活，且更迅速地創新。

**雲端運算** 是透過網際網路提供的 IT 資源隨需交付，採用按用量付費定價。您不必購買、擁有以及維護實體資料中心和伺服器，就能根據需要，從 Amazon Web Services (AWS) 這類雲端供應商存取技術服務，例如運算能力、儲存和資料庫。

**雲端運算**

- **優點**
    - 低價
    - 有彈性 敏捷
    - 開放 彈性
    - 安全
    - 全球研究
- **雲端服務考量**

    安全（可信度保證度）、運算速度⋯

- **雲端特色**
    - 24 個region
    - CDN（cloud front）205個點
    - 103 個 Direct Connect Locations
    - Availability zones 如何聰明達到availability 並不會花費太高成本

雲**端產品 (**參考：[https://aws.amazon.com/tw/products/?pg=WICC-N](https://aws.amazon.com/tw/products/?pg=WICC-N))

- 分析 ex
    - [Amazon EMR託管 Hadoop 框架](https://aws.amazon.com/tw/elasticmapreduce/?c=1&pt=4)
    - [Amazon Managed Streaming for Apache Kafka全受管的 Apache Kafka 服務](https://aws.amazon.com/tw/msk/?c=1&pt=6)
    - [AWS Data Pipeline適用於週期性資料驅動工作流程的協調流程服務](https://aws.amazon.com/tw/datapipeline/?c=1&pt=10)

- 運算 ex

    [Amazon EC2雲端的虛擬伺服器](https://aws.amazon.com/tw/ec2/?c=7&pt=1)

    [Amazon EC2 Auto Scaling擴展運算容量以滿足需求](https://aws.amazon.com/tw/ec2/autoscaling/?c=7&pt=2)

    [AWS Lambda執行程式碼以回應事件](https://aws.amazon.com/tw/lambda/?c=7&pt=10)

- 容器 ex

    [AWS Fargate容器的無伺服器運算](https://aws.amazon.com/tw/fargate/?c=7a&pt=4)

    [Amazon Elastic Container Service (ECS)高度安全、可靠且可擴展的容器執行方式](https://aws.amazon.com/tw/ecs/?c=7a&pt=2)

    [Amazon Elastic Kubernetes Service (EKS)最值得信賴的 Kubernetes 執行方式](https://aws.amazon.com/tw/eks/?c=7a&pt=3)

- 資料庫 ex

    [Amazon DynamoDB受管的 NoSQL 資料庫](https://aws.amazon.com/tw/dynamodb/?c=9&pt=2)

    [Amazon RDS適用於 MySQL、PostgreSQL、Oracle、SQL Server 和 MariaDB 的受管關聯式資料庫服務](https://aws.amazon.com/tw/rds/?c=9&pt=8)

    [Amazon Keyspaces (適用於 Apache Cassandra)受管 Cassandra 相容資料庫](https://aws.amazon.com/tw/keyspaces/?c=9&pt=5)

.....很多產品可參考連結

**產品介紹**

- 運算產品

    **Amazon EC2：**

    AMI: 是一種範本，其中包含軟體組態 (例如作業系統、應用程式伺服器和應用程式)。您可以從 AMI 啟動執行個體，執行個體是 AMI 的複本，在雲端中以虛擬伺服器的形式執行。您可以啟動 AMI 的多個執行個體

    **lambda：**

- 容器產品

    **ECS：**

    **EKS：**

    **Fargate：**

- ML 產品

    **AWS ML stack**

    - ML Frameworks & infrastructure
    - AWS SageMarker - CICD
    - AI services

    **應用案例**

    - INTUIT（客服
    - CONVOY（物流
    - Corner（醫療心電圖
    - NFA （美式足球 預測導播路徑球道

＊行業 AI

- Monitors .....