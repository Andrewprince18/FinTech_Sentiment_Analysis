# FinTech_Sentiment_Analysis

# 📁 FinTech Sentiment Analysis Project

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

#### 📓 Jupyter 筆記本

- `Circumplex_Models.ipynb`：
  - 簡介 **Russell 所提出的情緒環狀模型（Circumplex Model of Affect）**
  - 展示如何將文字資訊轉換為語音音檔，做為後續分析的資料來源

- `Transforms_Russell.ipynb`：
  - 包含**音訊前處理與特徵擷取流程**
  - 將音訊轉換為統一格式的特徵向量，供機器學習模型後續使用

---

## 🎛️ 音訊特徵擷取流程說明（Transforms_Russell.ipynb）

本段程式碼使用 `audioBasicIO` 與 `ShortTermFeatures` 兩個模組，從音訊中提取短期特徵，並計算其平均值作為代表特徵向量。

---

### 🛠️ 定義處理模組

- **`audioBasicIO`**：載入音訊、轉單聲道、取得波形與採樣率  
- **`ShortTermFeatures`**：提取短期特徵（如能量、過零率、頻譜特性等）

---

### 🪟 特徵提取設定

假設採樣率為 **16,000Hz**，則設定如下：

| 參數       | 值     |
|------------|--------|
| 窗口大小   | 0.05 秒 × 16,000 = 800 樣本 |
| 步幅長度   | 0.025 秒 × 16,000 = 400 樣本 |
| 窗口重疊   | 800 - 400 = 400 樣本 |

---

### 📐 特徵矩陣與名稱

- `features`：2D 特徵矩陣，行為特徵種類，列為時間切片
- `feature_names`：例如：
  - 能量（Energy）
  - 過零率（Zero Crossing Rate）
  - 頻譜質心（Spectral Centroid）等

---

### ⚖️ 特徵正規化（Normalization）

$$
\text{normalized-feature} = \frac{\text{feature} - \min(\text{feature})}{\max(\text{feature}) - \min(\text{feature})}
$$

如遇 $\max = \min$，則保留原始特徵以避免除以零。

---

### 📊 平均特徵向量

對每種正規化特徵取平均，作為整段音訊的代表特徵向量。

---

### 📦 輸出結果

- ✅ 特徵名稱清單
- ✅ 每個音檔的平均特徵向量

這些輸出可用於後續的分類、情緒辨識、語者識別等任務。

