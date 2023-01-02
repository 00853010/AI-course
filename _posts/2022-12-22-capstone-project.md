---
layout: post
title: Shrimps YOLO
author: [chih yee]
category: [Lecture]
tags: [jekyll, ai]
---

介紹如何在Darknet框架下，使用YOLO模型辨識蝦子。

### 組員
* **鄭濬緯** 00853048
* **胡誌宜** 00853010
* **張哲維** 


---

## 操作流程

* **下載蝦子辨識資料** <br>
  [下載蝦子辨識資料](https://drive.google.com/file/d/1C6Tus2PqsEpgVPhhVqb5LIFOMQbv9q8S/view?usp=sharing)，並且放置到桌面、解壓縮

* **使用LabelImg** <br>
  (1)首先到Github[下載LabelImg專案](https://github.com/tzutalin/labelImg)
  <td><img src="https://user-images.githubusercontent.com/80897253/210181194-97931f41-5587-4a75-91a8-1b54229c2064.png"></td>
  (2)接著按照README的安裝步驟執行程式<br>
  <td><img src="https://user-images.githubusercontent.com/80897253/210181243-76979885-9067-46a4-b704-4798db1ade55.png"></td>

* **LabelImg設定** <br>
  (1)點擊開啟目錄，選擇蝦子圖片資料集的目錄位置。 <br>
  /home/pi/Desktop/shrimp_yolo/requirements/shrimp_data/JPEGImages
  <td><img src="https://user-images.githubusercontent.com/80897253/210181776-c502a840-5cac-409d-9001-0a1d260c6fcb.png"></td>
  (2)點擊左方改變存放目錄，選擇Annotations資料夾，然後雙擊右下角的檔案。 <br>
  /home/pi/Desktop/shrimp_yolo/requirements/shrimp_data/labels
  <td><img src="https://user-images.githubusercontent.com/80897253/210181947-32f4ad26-b921-4b99-bd2e-223776ef958c.png"></td>
  (3)看到已被標籤好的圖片，右邊也顯示蝦子類別。另外左方儲存按鈕下方的按鈕，可以變更儲存格式，因為要訓練YOLO模型，在這裡必須顯示為YOLO。 <br>
  <td><img src="https://user-images.githubusercontent.com/80897253/210182062-b854b225-ae6b-4d69-b20b-92f4477cafe7.png"></td>

* **LabelImg標籤圖片** <br>
  點擊左方創建區塊，即可在圖片中圈選蝦子的範圍。圈選後會彈出視窗，可以選擇該範圍的類別名稱。 <br>
  <td><img src="https://user-images.githubusercontent.com/80897253/210182103-b75e3fd8-2e53-46af-9367-88256ffdf87c.png"></td>

---

## Darknet訓練

* **Darknet介紹** <br>
  Darknet是一個用C和CUDA撰寫的開源深度學習框架，支持CPU和GPU訓練模型，如同Tensorflow、Pytorch，雖然功能並沒有上述兩種多，但是相對的簡單又輕量。 <br>
  [Github連結](https://github.com/AlexeyAB/darknet)
  <td><img src="https://user-images.githubusercontent.com/80897253/210182205-1c9be2e3-0ba1-4462-8cf0-a1dad8166949.png"></td>

* **Makefile** <br>
  此檔案為整個Darknet專案的配置，包含GPU、cuDNN、OPENCV等功能的啟用與否。每次調整完，都需要重新編譯專案。請將配置調整如下圖所示。
  <td><img src="https://user-images.githubusercontent.com/80897253/210182328-73c1eaff-4e76-4fb6-9bd8-594a6e5d73bd.png"></td>
  
* **安裝OpenCV函式庫** <br>
  安裝指令
  <td><img src="https://user-images.githubusercontent.com/80897253/210182365-90d0002f-3cae-45b9-a0f3-4ae628839f4a.png"></td>

* **Makefile指令** <br>
  清除指令
  <td><img src="https://user-images.githubusercontent.com/80897253/210182420-525ab672-f4b6-48f8-9dba-61ca439e7cdf.png"></td>
  編譯指令
  <td><img src="https://user-images.githubusercontent.com/80897253/210182421-ba0b4adf-2ec3-4772-8049-72c8e118d122.png"></td>

* **project.data** <br>
  此檔案定義YOLO在訓練、預測時，會使用到的類別數量、列表以及標籤、權重備份資料夾
  <td><img src="https://user-images.githubusercontent.com/80897253/210182595-645fa0d0-7cab-4473-b4b2-594f6b085911.png"></td>
  
* **project.names** <br>
  此檔案包含YOLO訓練、預測時，會讀取的類別名稱列表。在此只需要辨識蝦子一種類別。
  <td><img src="https://user-images.githubusercontent.com/80897253/210182599-1d1e7480-12d8-49ba-b042-70d9d6a4bc8f.png"></td>
  
* **train.txt** <br>
  訓練集，此檔案包含80%的圖片位置，YOLO在訓練時會讀取的列表
  <td><img src="https://user-images.githubusercontent.com/80897253/210182604-9f22d0cd-625d-426d-901a-10071e9fd705.png"></td>

* **test.txt** <br>
  測試集，此檔案包含20%的圖片位置，YOLO在測試時會讀取的位置列表
  <td><img src="https://user-images.githubusercontent.com/80897253/210182601-fe19dc04-9697-4227-be34-92c010b51b52.png"></td>

* **val.txt** <br>
  驗證集，此檔案包含20%的圖片位置，YOLO在驗證時會讀取的位置列表
  <td><img src="https://user-images.githubusercontent.com/80897253/210182606-f9d87284-99c7-4908-ae02-5a618d6f324e.png"></td>

* **yolov3-tiny.cfg** <br>
  可以調整yolov3-tiny模型參數的相關設定，測試時batch,subdivisions必須設為1，而在訓練時，不得皆設為1。另外根據偵測類別的數量不同filters參數也必須更改filters = (classes+5)*3
  <td><img src="https://user-images.githubusercontent.com/80897253/210196184-5ad09e87-4a85-46aa-a90f-42abac73a9fb.png"></td>
  <td><img src="https://user-images.githubusercontent.com/80897253/210196241-4db3fc79-1b10-431d-8355-35f13333e779.png"></td>

* **Yolov3-tiny模型訓練** <br>
  訓練指令 ./darknet detector train ../requirements/project.data ../requirements/yolov3-tiny.cfg  ./darknet53.conv.74 
  <td><img src="https://user-images.githubusercontent.com/80897253/210196311-1041dd80-cadf-46ae-b47b-50eca32a767f.png"></td>
  過程會顯示訓練過程的日誌紀錄，且每100次訓練會備份一次模型權重。
  <td><img src="https://user-images.githubusercontent.com/80897253/210196343-398cff7e-4f1a-41d6-85e8-b55f293ca2fb.png"></td>

---

## Darknet測試
* **安裝OpenCV函式庫** <br>
  安裝指令<br>
  <td><img src="https://user-images.githubusercontent.com/80897253/210196429-e7ec095b-bf78-48a6-b90b-e06b8a2cd153.png"></td>
  修改Makfile設定<br>
  <td><img src="https://user-images.githubusercontent.com/80897253/210196447-83b9aff2-cf5d-4e57-be69-833cd5963099.png"></td>

* **Makefile指令** <br>
  清除指令 <br>
  <td><img src="https://user-images.githubusercontent.com/80897253/210196564-a5783960-86d1-46db-aa30-915fc7648841.png"></td>
  編譯指令 <br>
  <td><img src="https://user-images.githubusercontent.com/80897253/210196567-8edc8af0-1382-4d91-91e9-eefa8aa269de.png"></td>

---

## Darknet訓練_模型測試
* **模型測試 1** <br>
  影像辨識指令 <br>
  ./darknet detector test ../requirements/project.data ../requirements/yolov3-tiny.cfg  ../requirements/yolov3-tiny_final.weights ../shrimp.jpg <br>
  <td><img src="https://user-images.githubusercontent.com/80897253/210196861-fbc0654f-42ed-4c0a-8f64-f2f5e71513c5.png"></td>
  <td><img src="https://user-images.githubusercontent.com/80897253/210196866-cc4b6e1d-4150-4ab5-bcfe-398517d41126.png"></td> <br>
  自動生成一個prediction.jpg圖檔 <br>
  <td><img src="https://user-images.githubusercontent.com/80897253/210196871-9d3a9d34-f4d8-48c5-84fa-9cc65249fd72.png"></td>
  
* **模型測試 2** <br>
  影片辨識指令 <br>
  ./darknet detector demo ../requirements/project.data ../requirements/yolov3-tiny.cfg  ../requirements/yolov3-tiny_final.weights ../shrimp.avi <br>
  <td><img src="https://user-images.githubusercontent.com/80897253/210196945-02dc1b56-669c-424b-b27c-15097a0bfb93.png"></td>
  
* **影片辨識成果** <br>
  <td><img src="https://user-images.githubusercontent.com/80897253/210196962-84b7233b-b31f-48a3-bdd1-6c01fe7b5079.png"></td>

* **模型測試 3** <br>
  Webcam辨識指令 <br>
  ./darknet detector demo ../requirements/project.data ../requirements/yolov3-tiny.cfg  ../requirements/yolov3-tiny_final.weights <br>
  <td><img src="https://user-images.githubusercontent.com/80897253/210197003-9e982b41-f81f-47fb-8228-df3e44a27707.png"></td>

* **使用攝像頭對準影片上的魚蝦** <br>
  <td><img src="https://user-images.githubusercontent.com/80897253/210197027-5266a323-7a3b-4d93-9d6a-d615d729a6a4.png"></td>

---

## 影片展示
<iframe width="840" height="472" src="https://www.youtube.com/embed/8SJQaGuLIX4" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

---

## 工作分配
* **鄭濬緯** <br>
  尋找主題、程式撰寫、報告流程製作
* **張哲維** <br>
  尋找主題、樹梅派執行
* **胡誌宜** <br>
  尋找主題、網頁製作、YOLO模型輔助






