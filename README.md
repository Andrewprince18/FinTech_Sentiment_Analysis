# 📁 FinTech_Sentiment_Analysis Project

本專案旨在探索財經語料中的**情緒分析**與**語音特徵擷取**，並初步建構語音情緒分類的資料前處理流程。

---

## 📂 專案結構說明

### 1. `Crawling/`

此資料夾包含透過爬蟲抓取公司財報（10-K）的工具與範例程式。  
目前已成功抓取美國蘋果公司（Apple Inc.）的年報作為示範。

---

### 2. `Sentiment_Analysis/`

此資料夾專注於蘋果公司 2024 年第三季（Q3）**財報電話會議（Earnings Call）**的分析與音訊處理，包含以下資料與程式：

#### 🎧 音訊檔案

- `Apple_Q3_2024EarningsCall.mp3`：完整財報電話會議音檔  
- `Apple_Q3_2024EarningsCall_CEO_1.wav`：CEO 講話片段  
- `Apple_Q3_2024EarningsCall_CFO_1.wav`：CFO 講話片段  

---

#### 📓 Jupyter 筆記本說明

##### ✅ `Transforms_Russell.ipynb` — 音訊特徵擷取與處理流程

本筆記本展示如何將語音資料轉換為可用於機器學習的數值特徵向量。

###### 🎛️ 音訊特徵擷取流程說明

本段程式碼使用 `audioBasicIO` 與 `ShortTermFeatures` 兩個模組，從音訊中提取短期特徵，並計算其平均值作為代表特徵向量。

- **模組功能**：
  - `audioBasicIO`：載入音訊、轉單聲道、取得波形與採樣率  
  - `ShortTermFeatures`：提取能量、ZCR、頻譜特徵等  

###### 🪟 特徵提取設定（假設採樣率為 16,000Hz）

| 參數       | 值     |
|------------|--------|
| 窗口大小   | 0.05 秒 × 16,000 = 800 樣本 |
| 步幅長度   | 0.025 秒 × 16,000 = 400 樣本 |
| 窗口重疊   | 800 - 400 = 400 樣本 |

###### 📐 特徵矩陣與名稱

- `features`：2D 特徵矩陣，行為特徵種類，列為時間切片  
- `feature_names`：例如能量、ZCR、頻譜質心等  

###### ⚖️ 特徵正規化

$$
\text{normalized-feature} = \frac{\text{feature} - \min(\text{feature})}{\max(\text{feature}) - \min(\text{feature})}
$$

如遇 $\max = \min$，則保留原始值以避免除以零。

###### 📊 輸出結果

- 特徵名稱清單  
- 每個音檔的平均特徵向量  
- 可作為後續分類、情緒辨識、語者識別等任務的輸入資料  

---

##### ✅ `Circumplex_Models.ipynb` — 圓形情緒模型（Russell's Circumplex Model）視覺化

本筆記本根據 Russell 所提出的 **Circumplex Model of Affect**，將音訊資料中擷取出的愉悅度（valence）與激發度（arousal）特徵對應到一個圓形情感空間中。

###### 🎯 分析目標：

視覺化 CEO 與 CFO 的發言在情緒空間中的分佈與差異。

###### 🔁 分析步驟：

1. **特徵輸入**：從 `Transforms_Russell.ipynb` 擷取之特徵中，挑選愉悅度與激發度指標（如 Spectral Centroid、Energy entropy 等）  
2. **角度與座標計算**：
   - 將愉悅度設為 x 軸、激發度設為 y 軸
   - 根據座標位置推估情感方向  
3. **圓形模型視覺化**：
   - 劃分 8 個基本情緒象限：
     - 愉悅（Pleasant）、興奮（Excited）、激發（Aroused）、困擾（Distressed）、厭惡（Unpleasant）、抑鬱（Depressed）、困倦（Sleepy）、放鬆（Relaxed）  
   - CEO 與 CFO 分別用不同顏色標示  
   - 顯示其在情感空間中的相對位置與傾向  

###### 📈 視覺化結果

<div align="center">

<img src="Sentiment_Analysis/output.png" width="500"/>

</div>
該視覺化圖有助於：

- 理解發言者傳遞之潛在情緒類型  
- 比較 CEO 與 CFO 在語音表達上的差異  
- 後續延伸應用至多段語音、不同公司的語音比較  

---

## 🔮 未來展望

本專案未來將進一步：

- 結合語者辨識與情緒偵測技術  
- 探索聲學特徵與財報文字內容的聯動關係  
- 引入機器學習模型進行情緒分類與情感預測  


