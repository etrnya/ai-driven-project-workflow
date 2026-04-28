# 🕰️ Git 與 GitHub Desktop 版本控制完整教學

> 適合對象：完全沒有用過 Git 的新手開發者

---

## 一、什麼是 Git？

**Git 是你的「程式碼時光機」。**

它幫你記錄每一次的改動，讓你可以隨時回到過去的任何版本，不怕改壞。

想像你在寫一份重要報告：
- 沒有 Git：你可能有 `報告.docx`、`報告_最終版.docx`、`報告_真的最終版.docx`...
- 有了 Git：只有一份 `報告.docx`，但你可以隨時穿越回任何一個「存檔點」

---

## 二、三個核心概念（先理解這個，其他就通了）

| 概念 | 比喻 | 說明 |
| :--- | :--- | :--- |
| **Repository (Repo)** | 有時光機功能的資料夾 | 你的專案資料夾，Git 會在裡面記錄所有歷史 |
| **Commit** | 按下「存檔點」 | 記錄這個時刻所有改動的快照 |
| **Push / Pull** | 上傳 / 下載 | 把存檔點上傳到 GitHub，或從 GitHub 下載回來 |

---

## 三、工具選擇

Git 的操作有兩種方式，**強烈建議新手使用 GitHub Desktop**：

| | 指令列 (CLI) | ✅ GitHub Desktop (推薦) |
| :--- | :--- | :--- |
| 學習難度 | 高，要記很多指令 | **低，按按鈕就好** |
| 查看改動 | 需輸入 `git status` | **自動視覺化顯示** |
| 存檔 | 需輸入 2 行指令 | **填一格文字 + 按一個按鈕** |
| 上傳 | 需輸入 `git push` | **按一個按鈕** |

---

## 四、安裝準備

### 4.1 安裝 Git（命令列核心，必裝）

打開 PowerShell，輸入：
```powershell
winget install --id Git.Git -e --source winget
```
安裝完成後，**關閉並重新開啟 PowerShell**。

驗證安裝成功：
```powershell
git --version
# 應看到類似：git version 2.54.0.windows.1
```

### 4.2 設定你的身份（只需做一次）

```powershell
git config --global user.name "你的GitHub帳號名稱"
git config --global user.email "你的GitHub信箱"
```

### 4.3 安裝 GitHub Desktop（視覺化工具，強烈推薦）

👉 前往官網下載：**https://desktop.github.com/**

下載後直接安裝，開啟後點 **Sign in to GitHub.com**，用瀏覽器登入你的 GitHub 帳號並授權即可。

---

## 五、兩種使用方式

---

### ✅ 方式一：GitHub Desktop（推薦新手）

#### 情境 A：連結一個已存在於電腦的本地專案

1. 打開 GitHub Desktop
2. 點選上方選單 **File → Add Local Repository**
3. 選擇你在電腦上的專案資料夾
4. 如果 GitHub Desktop 說「這個資料夾沒有 Git」，點 **create a repository** 即可初始化

#### 情境 B：從 GitHub 上的專案複製到電腦（Clone）

1. 打開 GitHub Desktop
2. 點選上方選單 **File → Clone Repository**
3. 選擇 **GitHub.com** 分頁，找到你的專案（例如 `ai-driven-project-workflow`）
4. 選擇你想存放的本地路徑，點 **Clone**

---

#### 📝 日常工作流程（3 個步驟）

> 每次你改完程式或文件，重複這個流程即可：

**步驟 1：查看改動**

改完檔案後，打開 GitHub Desktop，左側會自動列出所有你改動的檔案：
- 🟢 綠色 `+` = 新增的內容
- 🔴 紅色 `-` = 刪除的內容

**步驟 2：寫存檔點說明並 Commit**

在左下角的欄位填入這次改動的說明（Summary），例如：
- `fix: 修正 README 錯字`
- `feat: 新增 Phase 5 說明`
- `docs: 更新安全守則`

填完後按下 **「Commit to main」** 按鈕。

**步驟 3：上傳到 GitHub**

按右上角的 **「Push origin」** 按鈕，改動就會同步到 GitHub 上。

---

#### ⏪ 後悔了怎麼辦？用 GitHub Desktop 還原

1. 點上方選單 **History** 分頁
2. 找到你想回到的那個存檔點（Commit）
3. 對它按右鍵，選 **Revert Changes in Commit**

---

### 🖥️ 方式二：指令列（進階參考）

如果你想了解 GitHub Desktop 背後實際執行的指令：

#### 初始化連結（只做一次）

```powershell
# 切換到你的專案資料夾
cd "你的專案路徑"

# 初始化 Git 時光機
git init

# 連結到 GitHub 上的遠端專案
git remote add origin https://github.com/你的帳號/你的專案名稱.git

# 把 GitHub 上的內容同步下來
git fetch origin
git reset --hard origin/main

# 把本地分支改名為 main（與 GitHub 一致）
git branch -m master main
```

#### 日常工作流程（每次改完都跑這三行）

```powershell
# 步驟 1：把所有改動加入準備存檔區
git add .

# 步驟 2：按下存檔點
git commit -m "這裡填改動說明"

# 步驟 3：上傳到 GitHub
git push origin main
```

#### 其他常用指令

```powershell
# 查看目前狀態（哪些檔案改動了）
git status

# 查看所有存檔點的歷史紀錄
git log --oneline

# 從 GitHub 下載別人的更新
git pull origin main

# 還沒 commit 前，把所有改動還原
git checkout -- .
```

---

## 六、整體流程示意圖

```
你的電腦                                    GitHub
────────────────────────────────────────────────────
改動檔案（寫程式、編輯文件）
        │
        ▼
  [GitHub Desktop] 左側看到改動清單
        │
        ▼
  填寫 Summary 說明
        │
        ▼
  按下「Commit to main」   ← 存檔點建立
        │
        ▼
  按下「Push origin」  ──────────────────→  GitHub 更新 ✅
                       ←──────────────────
                          按「Fetch/Pull」   從 GitHub 下載最新版
```

---

## 七、好的 Commit 說明怎麼寫？

> 清楚的說明讓你未來翻閱歷史時一眼就知道改了什麼。

建議格式：`類型: 說明`

| 類型 | 使用時機 | 範例 |
| :--- | :--- | :--- |
| `feat` | 新增功能或內容 | `feat: 新增 Phase 7 部署教學` |
| `fix` | 修正錯誤 | `fix: 修正 README 連結失效` |
| `docs` | 更新文件 | `docs: 補充 Docker 安裝步驟` |
| `refactor` | 重構但不影響功能 | `refactor: 整理 skills 資料夾結構` |
| `chore` | 雜項維護 | `chore: 更新 .gitignore` |

---

## 八、常見問題 QA

**Q: `push` 失敗，出現 rejected 錯誤？**
> A: 通常是因為 GitHub 上有你本地沒有的更新。先執行 `git pull origin main`，解決衝突後再 `push`。

**Q: 不小心 commit 了不該 commit 的檔案？**
> A: 在 `.gitignore` 檔案裡加上該檔案的名稱（例如 `.env`），下次就不會被追蹤了。

**Q: GitHub Desktop 和指令列哪個比較好？**
> A: 功能完全一樣，GitHub Desktop 只是讓操作更直覺。你可以兩個都用，不互衝。

---

*文件由 etrnya & Antigravity (AI) 協作整理，版本：2026-04-28*
