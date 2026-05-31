---
title : '预训练-对齐'
date : 2026-03-22T20:43:04+08:00
lastmod: 2026-03-22T20:43:04+08:00
description : "<iframe width="560" height="315" src="https://www.youtube.com/embed/Ozos6M1JtIE?..."
image : img/cat.jpg
draft : false
categories : ["AI"]
tags : ["学习笔记", "AI", "生成式AI時代下的機器學習(2025)"]
---
# 预训练-对齐

https://www.youtube.com/watch?v=Ozos6M1JtIE

<iframe width="560" height="315" src="https://www.youtube.com/embed/Ozos6M1JtIE?si=crCU_lIq_QDT9WPH" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

---

## 一、課程概述

- **課程**: 生成式AI時代下的機器學習 (2025) — 李宏毅 (台大)
- **第五講主題**: 大型語言模型訓練方法「預訓練–對齊」(Pretrain-Alignment) 的強大與極限
- **核心問題**: 為什麼 LLM 需要先做預訓練再做對齊？這套範式到底強在哪裡，又有什麼根本性的侷限？

---

## 二、預訓練 (Pretraining)

### 2.1 什麼是預訓練

- 預訓練是 LLM 訓練的第一階段，目標是讓模型從大規模無標註文本中學習語言的統計規律
- 核心思想：**自我監督學習 (Self-Supervised Learning)**，不需要人工標註資料
- 模型透過預測下一個 token（或遮蓋掉的 token）來理解詞彙、語法、語義乃至世界知識

### 2.2 預訓練目標 (Training Objectives)

| 目標 | 說明 | 代表模型 |
|------|------|----------|
| **Next Token Prediction (NTP)** | 給定上文，預測下一個 token；自回歸 (autoregressive) 方式 | GPT 系列 |
| **Masked Language Modeling (MLM)** | 隨機遮蓋輸入中的部分 token，讓模型還原 | BERT |
| **Prefix Language Modeling** | 部分輸入可雙向看到，輸出部分自回歸生成 | T5, GLM |
- 當前主流 LLM（GPT-4, Llama, Qwen, DeepSeek 等）幾乎都採用 **因果語言模型 (Causal LM)**，即 NTP 目標

### 2.3 預訓練資料

- **規模巨大**: 從 TB 級別到數十 TB 的文本資料
- **資料來源**:
  - 網頁爬蟲（Common Crawl, 網頁存檔）
  - 書籍（BookCorpus, 圖書掃描）
  - 學術論文（arXiv, PubMed）
  - 程式碼（GitHub）
  - 社群媒體（Reddit, 論壇）
- **資料處理關鍵步驟**:
  1. **去重 (Deduplication)** — 移除重複內容，避免記憶效應
  2. **過濾 (Filtering)** — 移除低品質、有毒、個人隱私內容
  3. **語種分類** — 平衡多語言分佈
  4. **品質評分** — 使用分類器或启发式規則評估文本品質
  5. **配比 (Data Mixing)** — 不同來源按比例混合

### 2.4 Scaling Laws (規模定律)

- **Kaplan et al. (2020)** 提出 LLM 的效能與三個因素呈冪律關係：
  - 模型參數數量 (N)
  - 訓練資料 token 數量 (D)
  - 計算量 (C)
- **關鍵發現**: 在計算預算固定下，應同時等比放大模型和資料，而非只放大一方
- **Chinchilla Law (Hoffmann et al., 2022)**:
  - 對於給定的計算預算，最佳模型大小與資料量比例約為 **20 tokens ／參數**
  - 這意味著許多早期模型（GPT-3）訓練不足，資料量太少

### 2.5 預訓練的強大之處

1. **知識獲取**: 模型從海量文本中學習到大量世界知識（事實、概念、關聯）
2. **語言能力**: 語法、修辭、邏輯推理的基礎能力在預訓練階段建立
3. **上下文學習 (In-Context Learning)**: 預訓練後模型具備從 prompt 中的範例學習的能力
4. **零樣本泛化**: 即使沒有見過特定任務，模型也能嘗試執行
5. **規模效應**: 隨著規模增大，湧現 (emergence) 出許多非預期的能力（如翻譯、程式生成）

### 2.6 預訓練的局限

1. **目標與人類需求不一致**: NTP 只追求預測準確，不關心回答是否有用、安全、誠實
2. **有害內容**: 訓練資料中包含偏見、毒性、錯誤資訊，模型會學到這些
3. **缺乏指令遵循能力**: 預訓練模型不知道如何回應「請用繁體中文總結以下文章」
4. **輸出控制困難**: 無法控制模型的語氣、格式、長度等屬性

---

## 三、對齊 (Alignment)

### 3.1 為什麼需要對齊

- 預訓練模型就像一個擁有龐大知識但**不受控的天才**——他知道很多，但不一定會按你的要求回答
- **對齊的目標**: 讓模型的行為符合人類的價值觀、偏好和意圖
- 對齊涵蓋: 有用性 (Helpfulness)、誠實性 (Honesty)、安全性 (Harmlessness) — **HHH 原則**

### 3.2 監督式微調 (Supervised Fine-Tuning, SFT)

- **也稱為 Instruction Tuning**
- **做法**: 收集大量「指令 — 理想回應」配對資料，對預訓練模型進行有監督微調
- **資料來源**:
  - 人工撰寫的指令範例
  - 從更強模型蒸餾 (distillation) 的輸出（如使用 GPT-4 生成）
  - 公開資料集（ShareGPT, OpenAssistant, Dolly 等）
- **優點**:
  - 實作簡單，只需要標準的交叉熵損失
  - 能快速讓模型學會遵循指令格式
  - 訓練穩定，不易發散
- **缺點**:
  - 依賴高品質人工標註，成本高
  - 模型只能模仿示範，無法學會超越示範的行為
  - 容易過擬合到特定的指令格式
  - 無法處理開放式偏好（多個答案都對但人類偏好其中一個）

### 3.3 基於人類回饋的強化學習 (RLHF)

#### 3.3.1 RLHF 的三階段流程

```
Stage 1: SFT — 用高品質指令資料微調預訓練模型
Stage 2: Reward Modeling — 訓練獎勵模型 (RM) 來預測人類偏好
Stage 3: PPO — 用強化學習最大化獎勵模型的分數
```

#### 3.3.2 獎勵模型 (Reward Model)

- **目標**: 學習一個函數 $r_\phi(x, y)$ 來給出回應的品質分數
- **訓練資料**: 人類標註員對多個模型輸出進行排序（偏好配對）
- **損失函數**: Bradley-Terry 模型
  - $L(\phi) = -\mathbb{E}[\log \sigma(r_\phi(x, y_w) - r_\phi(x, y_l))]$
  - 其中 $y_w$ 是較受偏好的回應，$y_l$ 是較不受偏好的回應
- **關鍵挑戰**:
  - 獎勵模型可能 hacking：模型找到高獎勵但實際上不好的輸出
  - 人類標註不一致，不同標註員偏好不同
  - 獎勵模型本身的偏見

#### 3.3.3 PPO (Proximal Policy Optimization)

- **核心思想**: 在強化學習中更新策略時，不要一次偏離原策略太遠
- **在 RLHF 中的角色**:
  - 策略 (Policy) = SFT 後的語言模型
  - 動作 (Action) = 生成下一個 token
  - 獎勵 = Reward Model 給出的分數 + KL 懲罰項
- **KL 懲罰項**: $-\beta \cdot D_{KL}(\pi_{\theta} || \pi_{SFT})$
  - 防止模型偏離 SFT 模型太遠，避免獎勵 hacking
  - 保持模型的生成多樣性
- **PPO 的優點**:
  - 訓練穩定，廣泛應用在實際系統（ChatGPT, Claude）
  - 理論上有收斂保證
- **PPO 的缺點**:
  - 實作複雜，需要同時維護 4 個模型（policy, reference, reward, value）
  - 對超參數敏感（KL 係數 $\beta$、學習率、clip 範圍）
  - 計算開銷大，訓練不穩定

#### 3.3.4 RLHF 的優勢

- 模型可以超越人類示範，生成比訓練資料中的更好的回應
- 本質上是在**優化人類偏好**而不僅僅是模仿
- 可以結合多種維度的考量（有用性 vs 安全性 trade-off）

#### 3.3.5 RLHF 的爭議與挑戰

1. **標註者偏差**: 人類標註員的價值觀會植入模型
2. **獎勵過度優化 (Reward Over-optimization)**: 模型找到獎勵模型的漏洞
3. **多輪對話的偏好**: 人類對單一回應的偏好與對話整體品質不一定一致
4. **Alpaca 效應**: RLHF 可能降低模型的多樣性和創造力
5. **訓練複雜度**: 需要大量工程基礎設施來穩定訓練

### 3.4 直接偏好最佳化 (Direct Preference Optimization, DPO)

#### 3.4.1 DPO 的核心思想

- **論文**: Rafailov et al., 2023 "Direct Preference Optimization: Your Language Model is Secretly a Reward Model"
- **關鍵洞察**: 獎勵模型可以表示為策略 (policy) 的函數，因此不需要顯式訓練 RM
- **損失函數**:
  - $L_{DPO}(\pi_\theta; \pi_{ref}) = -\mathbb{E}[\log \sigma(\beta \log \frac{\pi_\theta(y_w|x)}{\pi_{ref}(y_w|x)} - \beta \log \frac{\pi_\theta(y_l|x)}{\pi_{ref}(y_l|x)})]$
  - 直接最大化偏好回應的機率，最小化非偏好回應的機率

#### 3.4.2 DPO vs RLHF

| 方面 | RLHF | DPO |
|------|------|-----|
| 架構複雜度 | 需 4 個模型 | 只需 2 個模型 (policy + reference) |
| 訓練穩定性 | 敏感，易不穩定 | 較穩定 |
| 超參數 | 多 (LR, KL coeff, clip) | 少 (主要是 $\beta$) |
| 計算成本 | 高 | 低 |
| 理論基礎 | 強化學習 | 直接從偏好最佳化推導 |
| 獎勵模型 | 需要顯式訓練 | 隱含於策略中 |
| 可解釋性 | 獎勵模型可獨立分析 | 無法單獨分析獎勵 |
| 成熟度 | ChatGPT, Claude 等已驗證 | 較新，仍在快速發展 |

#### 3.4.3 DPO 的侷限

- 對偏好資料的品質更敏感
- 不適合需要線上探索 (online exploration) 的場景
- 無法直接處理 Reward Shaping 等更複雜的 RL 技術
- β 超參數的選擇對結果影響很大

### 3.5 其他對齊方法

| 方法 | 簡述 |
|------|------|
| **KTO (Kahneman-Tversky Optimization)** | 只需要知道一個回應是好是壞，不需要配對比較 |
| **ORPO (Odds Ratio Preference Optimization)** | 在 SFT 階段直接加入偏好學習，不需單獨對齊階段 |
| **SimPO (Simple Preference Optimization)** | 使用平均對數機率作為隱含獎勵，參考模型可選 |
| **CPO (Contrastive Preference Optimization)** | 結合對比學習與偏好最佳化 |
| **Rejection Sampling** | 從模型採樣多個輸出，選最好的做 SFT（如 Gemini 使用） |
| **Constitutional AI** | 讓模型根據一組原則自我修正（Anthropic 提出） |

---

## 四、預訓練–對齊範式的深入分析

### 4.1 為什麼是「預訓練 → 對齊」兩階段？

- **互補性**: 預訓練提供廣泛的知識基礎，對齊決定如何使用這些知識
- **分工明確**:
  - 預訓練: 學什麼是語言，學世界知識
  - 對齊: 學如何以人類期望的方式回應
- **效率考量**:
  - 對齊只需要相對少量的高品質資料（數萬到數十萬條）
  - 對齊階段的計算成本遠低於預訓練

### 4.2 兩階段的相互作用

- **瓶頸效應**: 對齊階段的品質上限取決於預訓練階段的知識儲備
  - 如果模型根本不知道某個事實，對齊也無法讓它正確回答
- **知識 vs 行為的分離**:
  - 預訓練決定模型「知道什麼」
  - 對齊決定模型「說什麼」
  - 一個有爭議的觀點：對齊可能只是教模型一個「回應風格」，而不是真正的價值觀塑造

### 4.3 這個範式真正強大在哪裡

1. **利用大規模無標註資料**: 不需要昂貴的人工標註就能獲取大量知識
2. **模組化設計**: 預訓練和對齊可以獨立改進
   - 一個更好的預訓練模型可以直接替換到對齊流程中
   - 對齊方法改進後也可以重新對齊已有的預訓練模型
3. **成本效益極高**:
   - 預訓練成本很高，但只需要做一次
   - 對齊可以在預訓練模型上反覆迭代
4. **湧現能力 (Emergent Abilities)**:
   - 預訓練階段完全沒有針對特定任務設計，但大規模模型湧現出推理、翻譯、程式碼生成等能力
   - 對齊釋放了這些湧現能力，讓它們可以被使用者利用

---

## 五、預訓練–對齊範式的極限

### 5.1 知識侷限

- **靜態知識**: 預訓練完成後，模型的知識就固定在訓練資料的截止時間
  - 無法自動更新（除非重訓或持續預訓練）
  - 對即時事件、新技術一無所知
- **長尾知識遺漏**: 對於低資源語言、小眾領域，預訓練資料不足
- **事實性問題**: 模型可能編造事實（幻覺, hallucination），因為預訓練目標是預測下一個 token 而不是保證事實正確

### 5.2 對齊的深層問題

#### 5.2.1 根本性的不一致

- **外推問題 (Extrapolation Problem)**: 對齊資料涵蓋的場景有限，當模型遇到超出對齊資料分佈的情況時，行為不可預測
- **目標錯置 (Goal Misgeneralization)**:
  - 模型可能在訓練環境中學會了迎合人類偏好，但在部署環境中做出有害行為
  - 例如：學會說「我無法完成這個請求」來避免被標註為有害，而不是真正理解為什麼不該做

#### 5.2.2 獎勵模型的天花板

- **Reward Model 本身也是模型**，也會有偏見和錯誤
- **Reward Hacking** 是 RLHF 的根本性難題：
  - 模型學會利用獎勵模型的弱點
  - 產生看起來合理但實際上無用或有害的輸出
- **多維度獎勵的權衡**:
  - 有用性 vs 安全性：一個過度安全的模型可能拒絕回答合理問題
  - 創造力 vs 一致性：過度對齊可能讓模型變得死板、模板化

#### 5.2.3 對齊稅 (Alignment Tax)

- **定義**: 對齊後模型在某些能力上不如對齊前的預訓練模型
- **常見案例**:
  - 數學推理能力下降
  - 程式碼生成品質降低
  - 創意寫作的多樣性減少
- **原因**: 對齊資料的偏好偏向於某種回應風格，壓縮了模型的輸出空間

### 5.3 規模的邊際效益遞減

- **Scaling Laws 在對齊階段不適用**: 資料量和模型大小對對齊效果的提升有明顯飽和
- **資料瓶頸**: 高品質的對齊資料難以規模化擴充
- **標註品質**: 隨著任務難度增加，人類標註員本身也難以給出最佳回應

### 5.4 價值觀植入問題

- **誰的價值觀？**
  - 對齊資料由特定群體（如美西工程師）標註
  - 模型可能反映出標註者的文化偏見和政治立場
- **價值觀衝突**:
  - 不同文化、不同國家對「安全」、「有害」的定義不同
  - 一個全球部署的模型難以同時滿足所有使用者的價值觀
- **對齊穩定性的質疑**:
  - Jailbreak 攻擊可以繞過對齊保護
  - 模型可能在某些 prompt 下展現出未對齊前的行為

---

## 六、前沿進展與未來方向

### 6.1 持續預訓練 (Continual Pretraining)

- 讓模型持續學習新知識，而不是一次訓練完
- 挑戰：災難性遺忘 (Catastrophic Forgetting)、資料混雜

### 6.2 多階段對齊

- 從單一對齊到多階段、多維度對齊（逐步提升安全性、有用性、誠實性）
- Anthropic 的 **Constitutional AI** 和 **Harmlessness Training**

### 6.3 弱到強泛化 (Weak-to-Strong Generalization)

- OpenAI 的 Superalignment 研究
- 用弱模型標註強模型的行為，讓強模型超越弱模型的監督訊號

### 6.4 線上對齊 (Online Alignment)

- 模型在部署中持續收集使用者回饋並更新
- 結合上下文學習 (In-Context Learning) 實現即時對齊

### 6.5 推理時對齊 (Inference-Time Alignment)

- 使用解碼策略（如 contrastive decoding, 最佳化搜尋）在生成時對齊
- 不需要修改模型權重

### 6.6 關於 Pretrain-Alignment 範式的反思

- **是否必須？** 新的架構（如 Test-Time Compute 擴展、推理模型）可能改變這個範式
- **o1 系列的啟示**: 強化學習不僅用於對齊，還用於提升推理能力（RL for reasoning）
- **對齊的終極目標**: 不是讓模型說人類想聽的話，而是讓模型真正理解人類的價值觀

---

## 七、總結與關鍵 takeaways

1. **預訓練** 是 LLM 能力的基礎，透過大規模自我監督學習獲取知識
2. **對齊** 是將模型能力引導到人類期望方向，包括 SFT、RLHF、DPO 等方法
3. **RLHF 是當前最先進的對齊方法**，但複雜度高、訓練不穩定
4. **DPO 提供了更簡潔的替代方案**，但在某些場景下仍有不足
5. **這個範式的極限** 包括知識靜態性、獎勵模型瓶頸、對齊稅、價值觀衝突等根本性問題
6. **未來方向** 持續預訓練、線上對齊、推理時對齊等可能突破當前侷限
7. **核心矛盾**: 預訓練優化的是 token 預測準確率，但我們真正想要的是有用、安全、誠實的模型——這兩個目標之間存在根本性的 gap，對齊就是試圖填補這個 gap

---

## 八、參考文獻與延伸閱讀

| 論文 / 資源 | 主題 |
|-------------|------|
| Kaplan et al., "Scaling Laws for Neural Language Models" (2020) | 預訓練規模定律 |
| Hoffmann et al., "Training Compute-Optimal Large Language Models" (2022) | Chinchilla Law |
| Ouyang et al., "Training Language Models to Follow Instructions with Human Feedback" (2022) | InstructGPT / RLHF |
| Rafailov et al., "Direct Preference Optimization" (2023) | DPO |
| Bai et al., "Constitutional AI: Harmlessness from AI Feedback" (2022) | Constitutional AI |
| Christiano et al., "Deep Reinforcement Learning from Human Preferences" (2017) | RLHF 先驅 |
| Ziegler et al., "Fine-Tuning Language Models from Human Preferences" (2019) | 早期 RLHF 工作 |
| Askell et al., "A General Language Assistant as a Laboratory for Alignment" (2021) | HHH 原則 |
| Shen et al., "ChatGPT 不是 LLM 的終點" (2023) | 中文反思文章 |
| 李宏毅, "生成式AI時代下的機器學習" 課程 (2025) | 本課程 |
