# shoppe_crawler_and_recommender_system
蝦皮爬蟲和使用Shopee - Price Match Guarantee(kaggle競賽)的模型做推薦系統


# 介紹  
先從蝦皮爬取資料和圖片(總共138000個商品)  
用之前在打kaggle競賽模型(蝦皮舉辦的)預測每個商品的相似商品

# 程式碼
* 爬蟲: https://github.com/h053473666/shoppe_crawler_and_recommender_system/blob/main/shoppe/shopee-web-crawler.ipynb  
* 下載圖片: https://github.com/h053473666/shoppe_crawler_and_recommender_system/blob/main/shoppe/shoppe-web-img-download-1.ipynb  
* 模型預測: https://github.com/h053473666/shoppe_crawler_and_recommender_system/blob/main/shoppe/shoppe-web-infer.ipynb  

# 競賽

競賽網址: https://www.kaggle.com/competitions/shopee-product-matching  
介紹: 為每個商品尋找一樣的商品  

我們這組最後的提交:  
https://www.kaggle.com/code/h053473666/2class-infer-batch-20  
# 模型
因為語言不一樣所以只使用圖片模型 ，以及為符合網站設計做部分修改。   
主要有5個模型:  
* efficientnet_b0 二元分類
* nfnet_l0 (all)  arcface loss
* nfnet_l0 (clothes) arcface loss
* nfnet_l0 (other) arcface loss
* efficientnet_b3 (all) arcface loss

先幫圖片分成兩類，之後用arcface loss得模型訓練，拿掉最後一層取出512個向量。  
模型融合部分是直接取平均:  
* nfnet_l0 (all) / 3  +  efficientnet_b3 (all) / 3  +  nfnet_l0 (clothes) / 3  
* nfnet_l0 (all) / 3  +  efficientnet_b3 (all) / 3  +  nfnet_l0 (other) / 3  
之後用knn尋找相似商品，距離取0.3，當沒有找滿5個商品距離+0.1，直到找滿5個。  

二元分類是我競賽時加的方法詳情可以看下面討論:  
https://www.kaggle.com/competitions/shopee-product-matching/discussion/238021  

arcface的實現部分可以參考這篇討論:  
https://www.kaggle.com/competitions/shopee-product-matching/discussion/226279  

# 架構圖
![架構](https://user-images.githubusercontent.com/37287974/169714533-010f1a30-a928-4f21-8129-3d739c8c73e4.png)



