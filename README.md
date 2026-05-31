# 🦠 SEIR + RPA 傳染病擴散模擬

[![NetLogo](https://img.shields.io/badge/NetLogo-7.0-blue)](https://netlogoweb.org)
[![License](https://img.shields.io/badge/License-MIT-green)](LICENSE)
[![GitHub Pages](https://img.shields.io/badge/Demo-GitHub%20Pages-orange)](https://Tai-Ju.github.io/epiDEM-SEIR-RPA)

> 基於 Agent-Based Modeling（ABM）的 SEIR 傳染病模擬，整合 RPA 自動化防疫流程  
> 以 NetLogo Web 在瀏覽器中即時執行，無須安裝任何軟體

🔗 **[▶ 開啟模擬](https://Tai-Ju.github.io/epiDEM-SEIR-RPA)**

---

## 模型說明

本模型基於 Northwestern University 的 epiDEM Travel and Control 模型（Yang & Wilensky, 2011），升級為 **SEIR 四狀態架構**，並整合 **RPA（Robotic Process Automation）自動化防疫模組**，模擬跨大陸傳染病擴散與公衛介入策略。

### SEIR 狀態機

```
S（易感）→ E（潛伏）→ I（感染）→ R（康復）
```

| 狀態 | 顏色 | 說明 |
|------|------|------|
| S 易感者 | ⬜ 白 | 尚未接觸病毒的健康個體 |
| E 潛伏者 | 🟠 橘 | 已接觸但尚未具傳染力 |
| I 感染者 | 🔴 紅 | 具傳染力，可傳播給周圍 S |
| R 康復者 | 🟢 綠 | 已痊癒，取得免疫 |
| 已接種 | 🔵 藍 | 預防接種，不受感染 |
| RPA Bot | 🟡 黃 | 自動化醫療調度人員 |

---

## RPA 自動化防疫模組

| 功能 | 觸發條件 | 動作 |
|------|----------|------|
| 📊 資料蒐集 | 每隔 rpa-scan-interval ticks | Bot 掃描周圍感染人數並記錄 |
| 🔒 自動隔離 | 感染率 > quarantine-threshold | 強制隔離所有現存感染者 |
| 🚑 救護車調度 | 住院人數 > dispatch-threshold | 自動增派 RPA Bot |
| 🛂 邊境封鎖 | 雙大陸同時出現感染者 | 關閉跨大陸旅行 |
| 💉 疫苗加強 | 感染持續上升 N 個 tick | 提升接種率並對易感者補種 |

---

## Y = f(X) 分析框架

### 輸出指標 Y
- **R₀**（基本傳染數）：衡量單一感染者平均傳染人數
- **累積感染率**：最終感染人口比例
- **族群動態**：S / E / I / R 各時間點人數
- **感染速率 β、康復速率 γ、潛伏轉換率 σ**

### 輸入參數 X

**結構參數（文獻校正）**

| 參數 | 預設值 | 文獻依據 |
|------|--------|----------|
| infection-chance | 20 | 校正至 R₀ = 2–3（He et al., 2020） |
| average-recovery-time | 120 | 潛伏期 5.1 天（Lauer et al., 2020） |
| recovery-chance | 12 | CDC COVID 康復率參考 |

**行為參數（機率性）**

| 參數 | 說明 |
|------|------|
| average-isolation-tendency | 個體自主隔離傾向（常態分布） |
| average-hospital-going-tendency | 就醫傾向 |
| travel-tendency | 跨大陸移動機率 |

---

## 模擬情境

| 情境 | 設定 | 預期效果 |
|------|------|----------|
| 無介入 | 所有 RPA 關閉 | 累積感染率 ~100% |
| 自動隔離 | rpa-auto-quarantine ON，閾值 10% | 曲線延平，峰值延後 |
| 疫苗加強 | rpa-auto-vaccine ON | 累積感染率降至 ~46% |

---

## 使用方式

### 方法一：直接瀏覽器執行（推薦）
點擊 **[▶ 開啟模擬](https://Tai-Ju.github.io/epiDEM-SEIR-RPA)**

### 方法二：本機 NetLogo Web
1. 開啟 [https://netlogoweb.org](https://netlogoweb.org)
2. 點擊右上角「Upload」
3. 選擇 `models/epiDEM_SEIR_RPA.nlogox`

---

## 檔案結構

```
epiDEM-SEIR-RPA/
├── index.html                    # GitHub Pages 展示頁
├── models/
│   └── epiDEM_SEIR_RPA.nlogox   # NetLogo 模型檔案
└── README.md
```

---

## 參考文獻

1. Kermack, W. O., & McKendrick, A. G. (1927). A contribution to the mathematical theory of epidemics. *Proceedings of the Royal Society of London*, 115(772), 700–721.
2. Hethcote, H. W. (2000). The mathematics of infectious diseases. *SIAM Review*, 42(4), 599–653.
3. He, S., Peng, Y., & Sun, K. (2020). SEIR modeling of the COVID-19 and its dynamics. *Nonlinear Dynamics*, 101, 1667–1680.
4. Lauer, S. A., et al. (2020). The incubation period of COVID-19. *Annals of Internal Medicine*, 172(9), 577–582.
5. Fan, Q., et al. (2024). Modeling COVID-19 spread using multi-agent simulation with NetLogo. *BMC Public Health*, 24.
6. Yang, C., & Wilensky, U. (2011). NetLogo epiDEM Travel and Control model. Northwestern University, Evanston, IL.
