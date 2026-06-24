#  3D Single-Hand Particle Interaction System

![Three.js](https://img.shields.io/badge/Three.js-r128-black?style=for-the-badge&logo=three.js&logoColor=white)
![MediaPipe](https://img.shields.io/badge/MediaPipe-Hands-00C7B7?style=for-the-badge&logo=google&logoColor=white)
![JavaScript](https://img.shields.io/badge/JavaScript-ES6%2B-F7DF1E?style=for-the-badge&logo=javascript&logoColor=black)
![WebGL](https://img.shields.io/badge/WebGL-2.0-990000?style=for-the-badge&logo=webgl&logoColor=white)

一個結合 **Three.js 3D 粒子流體動畫** 與 **Google MediaPipe 機器學習手勢辨識** 的網頁端交互系統。使用者可以透過視訊鏡頭，以「隔空手勢」即時控制 35,000 個粒子的空間旋轉、動態縮放，並在多個複雜的 3D 幾何模型間進行流暢的無縫線性變形（Morphing）。

---

##  核心技術與優化亮點

* **分子級 DNA 雙螺旋 (Cinematic DNA)**：透過數學公式精確分配粒子權重（主骨架、橫向鹼基對、背景散射粒子），還原具有真實景深感與立體結構的 3D DNA 鏈。
* **動態性能平衝 (Performance Balancing)**：將粒子基數精算優化至 35,000 點，在保持極致視覺密度的同時，大幅降低 GPU/CPU 負載，確保多數中低階電腦皆能跑滿 60 FPS。
* **彈性線性插值變形 (Lerp Morphing)**：切換模型時，所有粒子均透過增量插值（`positions[i] += (target - current) * 0.08`）進行動態位移，實現如流水般自然的形體轉換效果。
* **多維度手勢解耦演算法**：
  * **手掌中心 2D 矩陣投影**：將手掌在相機畫面中的 X/Y 軸偏移，映射為 3D 空間中的旋轉矩陣角度（`targetRotation`），實現隔空旋轉。
  * **深度歐氏距離映射**：當感應到「**握拳 (Fist)**」狀態時，系統會計算手掌物理骨架大小，將其等比映射為空間縮放係數（`Scale`），前後移動即可放大縮小粒子體。

---

## 控制指南與手勢操作

### 隔空手勢互動 (Webcam Required)
| 手勢狀態 | 畫面表現 | 互動反饋 |
| :---: | :--- | :--- |
| **張開手掌 (Open Hand)** | **空間旋轉模式** | 移動手掌位置可控制 3D 模型的 X/Y 軸旋轉。 |
| **握拳狀態 (Fist)** | **深度縮放模式** | 保持握拳，**靠近鏡頭**模型變大；**遠離鏡頭**模型變小（支援範圍 $0.1 \times \sim 2.4 \times$）。 |

### 🎛️ UI 控制面板 & 快捷鍵
* **3D Particle Shape**：支援五種高精細度模型切換：
  * `Brilliant Christmas Tree` (聖誕樹)
  * `Mysterious Saturn` (土星與星環)
  * `Romantic Heart` (立體心形)
  * `Geometric Cube` (正方體)
  * `DNA Double Helix` (電影感雙螺旋)
* **鍵盤快捷鍵**：在網頁任何地方按下 **鍵盤上鍵 (ArrowUp)** 或 **下鍵 (ArrowDown)**，可直接循環切換 3D 模型。
* **介面最小化**：點擊控制中心右上角的 `−` / `+` 按鈕，可收折 UI，獲取全螢幕沉浸式視覺體驗。

---

## 本地開發與環境架設

由於本系統需要調用瀏覽器的視訊鏡頭（Webcam），且外部模型庫與 MediaPipe 的 `.wasm` / `.tflite` 動態資源受瀏覽器跨來源資源共享（CORS）安全性限制，**必須在本地伺服器環境下執行**。

### 步驟說明：
1. **下載專案**：將專案的 `index.html` 原始碼儲存至本地資料夾。
2. **啟動本地伺服器**：
   * **方法 A (推薦)**：使用 VS Code 編輯器，並安裝 **Live Server** 擴充套件，點擊右下角 `Go Live` 啟動。
   * **方法 B (Python)**：在該專案目錄下開啟終端機（Terminal）並輸入：
     ```bash
     python -m http.server 8000
     ```
     接著在瀏覽器打開 `http://localhost:8000`。
3. **允許權限**：瀏覽器會彈出詢問「要求使用您的相機」，請點擊**允許**。
4. **開始互動**：等待 Loading 畫面消失後，將手掌置於鏡頭前方即可開始體驗！

---

## 外部依賴庫

本專案無需透過 `npm install` 安裝繁重的依賴，完全基於以下高效 CDN 靜態庫構建：
* **Three.js (r128)** — 3D 場景、渲染器與粒子系統核心。
* **OrbitControls.js** — 支援滑鼠右鍵拖拽、滾輪縮放的 3D 視角控制器。
* **MediaPipe Hands (v0.4)** — Google 機器學習手勢追蹤與 21 個手部關節節點即時計算流。
* **MediaPipe Camera Utils** — 高幀率相機視訊流擷取與輸入控制。

---

## 授權條款

本專案基於 **MIT License** 開源。歡迎在此基礎上自由擴充更多手勢型態（如比出剪刀切換模型、雙手互動等）或整合 Shader 實現更炫麗的粒子特效！
