# OpenWrt x86_64 Auto Build (原版 OpenWrt 自動編譯)

[![Build Custom OpenWrt (All Formats)](https://github.com/YOUR_USERNAME/YOUR_REPO_NAME/actions/workflows/build-openwrt.yml/badge.svg)](https://github.com/YOUR_USERNAME/YOUR_REPO_NAME/actions/workflows/build-openwrt.yml)

這是一個基於 **GitHub Actions** 的自動化編譯項目，專為 **x86_64 (一般 PC / 軟路由 / 虛擬機)** 平台設計。
它會自動偵測並下載 **OpenWrt 官方最新穩定版 (Stable Release)** 源碼進行編譯，確保系統純淨、穩定且安全。

---

## 🚀 項目特色

*   **自動追蹤最新版**：每次編譯時，自動抓取官方最新的穩定版本號 (例如 v24.10.x 或 v25.xx)，拒絕測試版 (RC)。
*   **高度客製化**：透過圖形化介面，自定義 **路由器 IP** 與 **磁碟空間大小**。
*   **全格式支援**：同時生成 **EXT4** 與 **SquashFS** 格式，並自動產出 **.img** (實體機) 與 **.vmdk** (虛擬機) 映像檔。
*   **Docker 支援**：可選配 Docker CE 核心與 Dockerman 圖形化管理介面。
*   **實用外掛整合**：可選配 UPnP、Samba (網路共享)、TTYD (網頁終端)、WOL (網路喚醒)、DDNS (動態域名)。
*   **一鍵線上更新**：內建 `Attended Sysupgrade`，未來可直接在網頁後台升級系統，無需重新刷機。
*   **繁體中文介面**：所有內建功能均已包含 `luci-i18n-*-zh-tw` 語言包。

---

## ⚙️ 如何開始編譯 (How to Build)

這是一個 **手動觸發 (Workflow Dispatch)** 的流程，你需要簡單操作幾步：

1. 點擊上方的 **[Actions]** 標籤頁。
2. 在左側列表中選擇 **"Build Custom OpenWrt (All Formats)"**。
3. 點擊右側的 **[Run workflow]** 按鈕。
4. **填寫表單內容** (如下表)，確認後點擊綠色的 **Run workflow** 開始編譯。

### 📝 表單參數說明

| 選項 (Input) | 預設值 | 說明 |
| :--- | :--- | :--- |
| **Router IP Address** | `192.168.10.1` | 設定路由器的 LAN 口管理 IP，避免與數據機衝突。 |
| **Firmware Size (MB)** | `2048` | 設定系統分區大小 (建議至少 1024MB，若要跑 Docker 建議 2048MB 以上)。 |
| **Include Docker?** | `yes` | **yes**: 安裝 Docker + Dockerman。<br>**no**: 僅編譯純淨系統。 |
| **Include Essential Apps?** | `yes` | **yes**: 安裝 UPnP, Samba, TTYD, WOL, DDNS。<br>**no**: 不預裝這些外掛。 |

⏳ **編譯時間**：大約需要 **2 ~ 3 小時**，請耐心等待。編譯完成後，該次任務會顯示綠色勾勾 ✅。

---

## 📥 下載與安裝指南

編譯完成後，請進入該次 Action 的詳細頁面，在底部的 **Artifacts** 或 **Releases** 區域下載韌體。

### ⚠️ 重要提示：關於壓縮檔
為了突破 GitHub 單檔案 2GB 的上傳限制，所有映像檔均已壓縮為 **`.gz`** 格式。
**下載後請務必先解壓縮** (Windows 推薦 7-Zip，Mac/Linux 直接點擊解壓)，還原成 `.img` 或 `.vmdk` 才能使用。

### 📦 檔案格式選擇

| 檔案名稱特徵 | 適用場景 (推薦度) | 說明 |
| :--- | :--- | :--- |
| **squashfs-combined-efi** | ⭐⭐⭐⭐⭐ (首選) | **支援 UEFI 啟動**。SquashFS 格式支援 **「恢復原廠設定」** 功能，系統玩壞了可一鍵重置，最適合日常使用。 |
| **squashfs-combined** | ⭐⭐⭐⭐ | **傳統 BIOS (Legacy) 啟動**。適用於較舊的電腦主機。同樣支援恢復原廠設定。 |
| **ext4-combined** | ⭐⭐⭐ | 類似一般 Linux 系統格式，方便調整分區大小，但**不支援** OpenWrt 的恢復原廠設定功能 (玩壞需重刷)。 |
| **.vmdk** | 🖥️ 虛擬機專用 | 適用於 **VMware Workstation / ESXi / VirtualBox**。無需轉檔，解壓後直接掛載即可開機。 |
| **.img** | 💻 實體機 / PVE | 適用於寫入隨身碟、SSD，或是在 PVE (Proxmox VE) 中轉換使用。 |

---

## 🔐 預設登入資訊

刷機並開機後，請使用瀏覽器訪問你設定的 IP (預設 `192.168.10.1`)。

*   **帳號 (Username)**: `root`
*   **密碼 (Password)**: `無` (空密碼)

> ⚠️ **安全提醒**：首次登入後，請務必立即前往「系統」->「管理權限」設定一組新的密碼，並開啟 SSH 登入限制。

---

## 🛠️ 已整合套件清單 (視選項而定)

*   **核心系統**: Official OpenWrt Stable (Ext4 + SquashFS)
*   **管理介面**: LuCI (含 SSL 支援) + 繁體中文
*   **系統工具**:
    *   `luci-app-attendedsysupgrade`: 系統線上更新
    *   `qemu-ga`: QEMU Guest Agent (虛擬機增強支援)
*   **Docker (可選)**:
    *   `docker`, `dockerd`: 容器核心
    *   `luci-app-dockerman`: Docker 網頁管理介面
*   **實用外掛 (可選)**:
    *   `luci-app-upnp`: 自動端口映射 (遊戲機/BT必備)
    *   `luci-app-samba4`: 網路芳鄰檔案共享
    *   `luci-app-ttyd`: 網頁版終端機
    *   `luci-app-wol`: 網路喚醒 (Wake on LAN)
    *   `luci-app-ddns`: 動態域名解析

---

## 📄 免責聲明
本項目僅提供自動化編譯腳本，所有源碼均來自 OpenWrt 官方倉庫。使用者需自行承擔刷機風險。
