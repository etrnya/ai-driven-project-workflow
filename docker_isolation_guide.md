# Docker 開發環境隔絕教學：防止 AI 誤刪事故

## 1. 為什麼需要環境隔絕？
本次事故證明，當 AI Agent 在本機環境（尤其是 Windows PowerShell）執行毀滅性指令（如 `Remove-Item -Recurse`）時，若路徑包含特殊字元（如 Next.js 的 `[id]`），極易觸發路徑解析錯誤，導致非預期的大規模檔案遺失。

**使用 Docker 的好處：**
- **路徑標準化**：使用 Linux 路徑格式，避免 Windows 萬用字元解析 Bug。
- **權限控制**：AI 只能存取掛載進容器的資料夾，無法觸碰 C 槽其他資料。
- **易於復原**：即使容器內的系統壞了，也只要重啟即可，不影響主機。

---

## 2. 安裝與準備
1. **安裝 Docker Desktop**：從 [官網](https://www.docker.com/products/docker-desktop/) 下載並安裝。
2. **安裝 Node.js**：確保本機有基本環境以進行初始化。
3. **檢查狀態**：
   ```bash
   docker --version
   docker run hello-world
   ```

---

## 3. Next.js 專案隔離配置模板
在您的專案根目錄下建立以下兩個檔案：

### A. `Dockerfile` (定義環境)
```dockerfile
# 使用輕量級 Node.js 鏡像
FROM node:20-alpine

# 設定容器內的工作路徑
WORKDIR /app

# 先複製 package.json 以利用快取安裝套件
COPY package*.json ./
RUN npm install

# 複製其餘原始碼
COPY . .

# 預設 Next.js 埠號
EXPOSE 3000

# 啟動開發伺服器
CMD ["npm", "run", "dev"]
```

### B. `docker-compose.yml` (啟動與掛載)
```yaml
version: '3.8'
services:
  app:
    build: .
    ports:
      - "3000:3000"
    volumes:
      - .:/app              # 將本機代碼掛載到容器內
      - /app/node_modules    # 避免本機 node_modules 衝突
    environment:
      - NODE_ENV=development
```

---

## 4. 如何啟動並使用
1. **啟動容器**：
   ```bash
   docker-compose up --build -d
   ```
2. **進入開發**：
   - 瀏覽器打開 `http://localhost:3000`。
   - 建議搭配 VS Code 的 **Dev Containers** 擴充功能，直接在容器內寫程式。

---

## 5. 安全開發黃金準則 (防止意外)
1. **永遠使用 `-LiteralPath`**：若必須在 Windows 本機刪除，請勿使用 `-Path`。
2. **禁止 Root 操作**：在 Dockerfile 中可以設定非 root 使用者執行指令。
3. **Git 版控**：每天結束或進行重構前，執行 `git add .` 與 `git commit`。
4. **沙盒意識**：將 AI 的工作範圍限制在單一目錄，不給予整個磁碟的存取權。

---
*這份文件是由 2026-04-26 技術事故總結而成，旨在守護開發者的資料安全。*