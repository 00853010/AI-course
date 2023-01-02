---
layout: post
title: Shrimps YOLO
author: [chih yee]
category: [Lecture]
tags: [jekyll, ai]
---

介紹如何在Darknet框架下，使用YOLO模型辨識蝦子。

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

### Darknet訓練

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


### Darknet測試
* **安裝OpenCV函式庫** <br>
  安裝指令
  <td><img src="https://user-images.githubusercontent.com/80897253/210196429-e7ec095b-bf78-48a6-b90b-e06b8a2cd153.png"></td>
  修改Makfile設定
  <td><img src="https://user-images.githubusercontent.com/80897253/210196447-83b9aff2-cf5d-4e57-be69-833cd5963099.png"></td>








<td><img src=""></td>
<p align="center"><img src=" " width="50%" height="50%"></p>



* GPU Server
<p align="center"><img src="https://www.leadtek.com/images/news/20190527_1_en.jpg" width="50%" height="50%"></p>

* GPU Workstation
<p align="center"><img src="https://www.nvidia.com/content/dam/en-zz/Solutions/Data-Center/dgx-station-a100/nvidia-dgx-station-og.jpg" width="80%" height="80%"></p>

* GPU PC
<table>
<tr>
<td><img src="https://upload.wikimedia.org/wikipedia/commons/9/96/Quad-GeForce-GTX-Titan-Black-Ultimate-GPU-Gaming-Computer.png"></td>
<td><img src="https://media.zenfs.com/en/techradar_949/bc146508a6d089d94ed89057a96c60df"></td>
</tr>
</table>

* Embedded Systems
![](https://www.nvidia.com/content/dam/en-zz/Solutions/intelligent-machines/embedded-systems/jetson-commercial-journey-og.jpg)

---
## AI chips
### [Top 10 Gamechanger of AI Chips Industry to Know in 2022](https://industrywired.com/top-10-gamechanger-of-ai-chips-industry-to-know-in-2022/)
* **IBM** released its “neuromorphic chip” TrueNorth AI in 2014. TrueNorth includes 5 four billion transistors, 1 million neurons, and 256 million synapses.<br>
“What we’ve done with the Telum chip is we’ve completely re-architected how these caches work to keep a lot more data a lot closer to the processor core than we have done in the past,” said Christian Jacobi, IBM Fellow and chief technology officer of system architecture and design for IBM zSystems. “To do this, we’ve quadrupled the level two cache. We now have a 32-MB level-two cache.”<br>
* **Nvidia** H100 features fourth-generation Tensor Cores and the Transformer Engine with FP8 precision that provides up to 9X faster training over the prior generation for mixture-of-experts (MoE) models. The combination of fourth-generation NVlink, which offers 900 gigabytes per second (GB/s) of GPU-to-GPU interconnect
* **Intel** Gaudi2, designed by Intel's Israel-based Habana Labs, is twice as fast as its first-generation predecessor.
* **Google** Cloud TPU is the purpose-constructed device gaining knowledge of accelerator chip that powers Google merchandise like Translate, Photos, Search, Assistant, and Gmail. 
* **Advanced Micro Devices (AMD)** gives hardware and software program answers including EPYC CPUs and Radeon Instinct GPUs for device studying and deep studying

leading AI chip startups<br>
* **Cerebras Systems** WSE-2, which has 850,000 cores and 2.6 trillion transistors
* **SambaNova Systems** has advanced the SN10 processor chip and raised greater than $1.1 billion in funding. SambaNova Systems builds information facilities and rentals them to the businesses.
* **Graphcore** is a British agency based in 2016. The agency introduced its flagship AI chip as IPU-POD256. Graphcore has already been funded with around seven hundred million.
* **Groq** has been based through former Google employees. The startup has already raised around $350 million and produced its first fashions consisting of GroqChip Processor, GroqCard Accelerator, etc.

---
### Tesla D1 chip
[Enter Dojo: Tesla Reveals Design for Modular Supercomputer & D1 Chip](https://www.hpcwire.com/2021/08/20/enter-dojo-tesla-reveals-design-for-modular-supercomputer-d1-chip/)
<table>
  <tr>
    <td><img src="https://6lli539m39y3hpkelqsm3c2fg-wpengine.netdna-ssl.com/wp-content/uploads/2021/08/d1-chip-tesla-300x204.png"/></td>
    <td><img src="https://6lli539m39y3hpkelqsm3c2fg-wpengine.netdna-ssl.com/wp-content/uploads/2021/08/training-tile-tesla-300x206.png"/></td>
  </tr>
</table>
![](https://6lli539m39y3hpkelqsm3c2fg-wpengine.netdna-ssl.com/wp-content/uploads/2021/08/exapod-768x250.png)

<font size="3">
With each D1 chip providing 22.6 teraflops of FP32 performance, <br>
each training tile will provide 565 teraflops and each cabinet (containing 12 tiles) will provide 6.78 petaflops - <br>
meaning that one ExaPOD alone will deliver a maximum theoretical performance of 67.8 FP32 petaflops. <br>
</font>

[Tesla details Dojo supercomputer, reveals Dojo D1 chip and training tile module](https://www.datacenterdynamics.com/en/news/tesla-details-dojo-supercomputer-reveals-dojo-d1-chip-and-training-tile-module/)
<table>
  <tr>
  <td><img src="https://i0.wp.com/semianalysis.com/wp-content/uploads/2021/08/training-tile-2.png?resize=800%2C445&ssl=1" /></td>
  <td><img src="https://media.datacenterdynamics.com/media/images/training_tiles_III.original.png" /></td>
  </tr>
</table>

---
### Google TPU
* [TPU System Architecture](https://cloud.google.com/tpu/docs/system-architecture-tpu-vm)
* Cloud TPU VM architectures
![](https://cloud.google.com/tpu/docs/images/tpu-pod-architecture.png)
* Ref. [Hardware for Deep Learning Part4: ASIC](https://blog.inten.to/hardware-for-deep-learning-part-4-asic-96a542fe6a81)
![](https://miro.medium.com/max/2000/1*iOWhkTnD0uXnpQ2Y_viHdA.png)
* TPU v4 
![](https://1.bp.blogspot.com/-E_4XkiTpfik/YLInSqPOY6I/AAAAAAAAAYo/h9FR2niT-yMZJgLUcLi03C2w-4yFCUwFgCLcBGAsYHQ/s16000/Google-TPU-v4.png)
* TPU v3 Block Diagram
![](https://1.bp.blogspot.com/-eVdyyxSonCM/YLIp1D90koI/AAAAAAAAAZo/yn_8_Ku9ReglUf_-UKVxQh8Nidpi1iFJwCLcBGAsYHQ/w640-h350/TPU-v3-Block-Diagram.png)
* TPU Block Diagram
![](https://miro.medium.com/max/700/1*9uNlFIx5Uic2hoC4jIV6hg.png)

---
### Nvida GPUs
* **V100** (DataCenter GPU)
![](https://www.nvidia.com/content/dam/en-zz/es_em/es_em/Solutions/Data-Center/tesla-v100/data-center-tesla-v100-pcie-625-ud@2x.jpg)
* **A100** (DataCenter GPU)
![](https://s4.itho.me/sites/default/files/images/Nvidia%20A100%20PCIe.jpg)
* **H100**:
![](https://developer-blogs.nvidia.com/wp-content/uploads/2022/03/GTC2022_SXM5_01_v001_DL-1536x864.png)

* **DXG-H100** (Workstation)
![](https://sw.cool3c.com/user/29442/2022/3230118d-3972-4525-a8a7-bbf06bcac615.jpg)
* **RTX4090** (Graphics Card)
![](https://www.digitaltrends.com/wp-content/uploads/2022/09/rtx409001.jpg?fit=720%2C405&p=1)

---
### [NVIDIA Hopper Architecture In-Depth](https://developer.nvidia.com/blog/nvidia-hopper-architecture-in-depth/)<br>
![](https://developer-blogs.nvidia.com/wp-content/uploads/2022/03/Perf-Main-FINAL-768x325.jpg)
![](https://developer-blogs.nvidia.com/wp-content/uploads/2022/03/H100-Streaming-Multiprocessor-SM-625x869.png)
![](https://developer-blogs.nvidia.com/wp-content/uploads/2022/03/New-Hopper-FP8-Precisions-625x340.jpg)

---
### [NVIDIA DLSS3](https://www.nvidia.com/en-us/geforce/news/dlss3-ai-powered-neural-graphics-innovations/)
NVIDIA DLSS revolutionized graphics by using AI super resolution and Tensor Cores on GeForce RTX GPUs to boost frame rates while delivering crisp, high quality images that rival native resolution.
![](https://images.nvidia.com/aem-dam/Solutions/geforce/ada/news/dlss3-ai-powered-neural-graphics-innovations/how-nvidia-dlss-3-works.jpg)
![](https://images.nvidia.com/aem-dam/Solutions/geforce/ada/news/dlss3-ai-powered-neural-graphics-innovations/nvidia-dlss-3-motion-optical-flow-accelerator.jpg)
<iframe width="750" height="422" src="https://www.youtube.com/embed/cJlo2I7CiD0" title="Microsoft Flight Simulator | NVIDIA DLSS 3 - Exclusive First-Look" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

---
### CUDA & CuDNN
* [CUDA Toolkit](https://developer.nvidia.com/cuda-toolkit) 
* [CuDNN](https://developer.nvidia.com/cudnn)
![](https://developer.nvidia.com/sites/default/files/akamai/cudnn/cudnn_chart.png)

---
### Nivida Jetson 
![](https://www.fastcompression.com/img/jetson/nvidia-jetson-modules2.png)
![](https://github.com/rkuo2000/AI-course/blob/gh-pages/images/Jetson_modules_Hardware_comparison.png?raw=true)
* [Jetson AGX Orin 32GB module](https://www.nvidia.com/zh-tw/autonomous-machines/embedded-systems/jetson-orin/) 275 TOPS
![](https://www.nvidia.com/content/dam/en-zz/Solutions/intelligent-machines/jetson-orin/jetson-orin-modules-2c50-d.jpg)
* [Jetson Benchmarks](https://developer.nvidia.com/embedded/jetson-benchmarks)
* [Getting the Best Performance on MLPerf Inference 2.0](https://developer.nvidia.com/blog/getting-the-best-performance-on-mlperf-inference-2-0/)

---
### Jetson Orin Nano
It has up to eight streaming multiprocessors (SMs) composed of 1024 CUDA cores and up to 32 Tensor Cores for AI processing.
![](https://developer-blogs.nvidia.com/wp-content/uploads/2022/09/Block-diagram-of-Jetson-Orin-Nano-625x508.png)
![](https://github.com/rkuo2000/AI-course/blob/gh-pages/images/Jetson_Orin_Nano.png?raw=true)

---
### Kneron 耐能智慧
* KL530 AI SoC
![](https://www.kneron.com/tw/_upload/image/solution/large/938617699868711f.jpg)
  - 基於ARM Cortex M4 CPU内核的低功耗性能和高能效設計。
  - 算力達1 TOPS INT 4，在同等硬件條件下比INT 8的處理效率提升高達70%。
  - 支持CNN,Transformer，RNN Hybrid等多種AI模型。
  - 智能ISP可基於AI優化圖像質量，強力Codec實現高效率多媒體壓縮。
  - 冷啟動時間低於500ms，平均功耗低於500mW。
<br>
<br>
* KL720 AI SoC (算力可達0.9 TOPS/W)
![](https://www.kneron.com/tw/_upload/image/solution/large/95f4758c9cfd08.png)
  - 基於ARM Cortex M4 CPU内核的低功耗性能和高能效設計
  - 可適配高端IP攝像頭，智能電視，AI眼鏡、耳機以及AIoT網絡的終端設備。 
  - 可處理高達4K圖像，全高清影音和3D感應，實現精準的臉部識別以及手勢控制。 
  - 可為翻譯機和AI助手等產品提供自然語言處理。 
  - 以上各種功能以及其它邊緣AI — 例如感熱 — 均可實時處理。 
<br>

---
### Realtek AmebaPro2
[AMB82-MINI](https://www.amebaiot.com/en/amebapro2/#rtk_amb82_mini)<br>
![](https://www.amebaiot.com/wp-content/uploads/2022/06/AMB82-MINI-2048x1489.jpg)
* MCU
  - Part Number: RTL8735B
  - 32-bit Arm v8M, up to 500MHz
* MEMORY
  - 768KB ROM
  - 512KB RAM
  - Supports MCM embedded DDR2/DDR3L memory up to 128MB
  - External Flash up to 64MB
* KEY FEATURES
  - Integrated 802.11 a/b/g/n Wi-Fi, 2.4GHz/5GHz
  - Bluetooth Low Energy (BLE) 4.2
  - Integrated Intelligent Engine @ 0.4 TOPS
  - Ethernet Interface
  - USB Host/Device
  - SD Host
  - ISP
  - Audio Codec
  - H.264/H.265
  - Secure Boot
  - Crypto Engine
* OTHER FEATURES
  - 2 SPI interfaces
  - 1 I2C interface
  - 8 PWM interfaces
  - 3 UART interfaces
  - 3 ADC interfaces
  - 2 GDMA interfaces
  - Max 23 GPIO
<iframe width="580" height="327" src="https://www.youtube.com/embed/_Kzqh6JXndo" title="AIoT: AmebaPro2 vs ESP32" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

---
## [mlplatform.org](https://www.mlplatform.org/)
The machine learning platform is part of the Linaro Artificial Intelligence Initiative and is the home for Arm NN and Compute Library – open-source software libraries that optimise the execution of machine learning (ML) workloads on Arm-based processors.
![](https://www.mlplatform.org/assets/images/assets/images/content/NN-frameworks20190814-800-1d11a6.webp)
<table>
  <tr><td>Project</td><td>Repository</td></tr>
  <tr><td>Arm NN</td><td>[https://github.com/ARM-software/armnn](https://github.com/ARM-software/armnn)</td></tr>
  <tr><td>Compute Library</td><td>[https://review.mlplatform.org/#/admin/projects/ml/ComputeLibrary](https://review.mlplatform.org/#/admin/projects/ml/ComputeLibrary)</td></tr>
  <tr><td>Arm Android NN Driver</td><td>https://github.com/ARM-software/android-nn-driver</td></tr>
</table>

---
### [ARM NN SDK](https://www.arm.com/zh-TW/products/silicon-ip-cpu/ethos/arm-nn)
免費提供的 Arm NN (類神經網路) SDK，是一組開放原始碼的 Linux 軟體工具，可在節能裝置上實現機器學習工作負載。這項推論引擎可做為橋樑，連接現有神經網路框架與節能的 Arm Cortex-A CPU、Arm Mali 繪圖處理器及 Ethos NPU。<br>

**[ARM NN](https://github.com/ARM-software/armnn)**<br>
Arm NN is the most performant machine learning (ML) inference engine for Android and Linux, accelerating ML on Arm Cortex-A CPUs and Arm Mali GPUs.

---
## [AI on Chip Taiwan Alliance (台灣人工智慧晶片聯盟)](https://www.aita.org.tw/)
![](https://www.aita.org.tw/images/sig2-1.png)
<br>
![](https://www.aita.org.tw/images/sig2-2.png)

---
## ML Benchmark: 
<p align="center"><img src="https://www.servethehome.com/wp-content/uploads/2021/06/Google-TPU-v4-MLPerf-v1.0-Tope-Line-Results-1536x1536.png" width="70%" height="70%"></p>

### [MLPerf](https://mlcommons.org/en/)
* MLPerf Inference v1.1 Results<br>
&emsp;[inference-datacenter v1.1 results](https://mlcommons.org/en/inference-datacenter-11/)<br>
&emsp;[inference-edge v1.1 results](https://www.mlcommons.org/en/inference-edge-11/)<br>
* [MLPerf Training v1.0 Results](https://mlcommons.org/en/training-normal-10/)<br>
* [MLPerf Tiny Inference Benchmark](https://mlcommons.org/en/inference-tiny-05)<br>

---
## Framework
### [PyTorch](https://pytorch.org)
![](https://miro.medium.com/max/1400/1*agu5YjWbY1RXhWHZJLZwbw.png)

### [Tensorflow](https://www.tensorflow.org)

### [Tensorflow Lite](https://www.tensorflow.org/lite)

### [Tensorflow Lite for Microcontroller](https://www.tensorflow.org/lite/microcontrollers)

### TinyML
[EloquentTinyML](https://github.com/eloquentarduino/EloquentTinyML)

### [Tensorflow.js](https://www.tensorflow.org/js/demos)

### [MediaPipe](https://google.github.io/mediapipe/)

---
## Open Platforms
<table>
  <tr>
    <td><a href="https://kaggle.com"><img src="https://www.kaggle.com/static/images/site-logo.png"></a></td>
    <td><a href="https://gym.openai.com/"><img src="https://i.pinimg.com/474x/a8/5e/11/a85e111c643e20543f1a5283a2de835c.jpg"></a></td>
  </tr>
</table>
<br>
<br>

*This site was last updated {{ site.time | date: "%B %d, %Y" }}.*

