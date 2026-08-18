# 安裝與部署

本篇記錄在 Linux 環境安裝 **MetaDefender Core** 的完整流程，適用於單機部署與 LAB 測試環境。

## 系統需求

正式部署前請確認主機資源，引擎數量越多、記憶體需求越高：

| 項目 | 最低需求 | 建議值 |
|------|----------|--------|
| CPU | 4 核心 | 8 核心以上 |
| 記憶體 | 8 GB | 16 GB 以上 |
| 磁碟空間 | 40 GB | 100 GB（含引擎更新） |
| 作業系統 | Ubuntu 20.04 / 22.04、RHEL 8 / 9 | — |

## 安裝步驟

依你的作業系統選擇對應指令：

=== "Ubuntu / Debian"

    ```bash
    # 更新套件清單並安裝下載工具
    sudo apt update && sudo apt install -y wget

    # 下載並安裝 MetaDefender Core
    wget "https://software.opswat.com/ometascan.deb"
    sudo dpkg -i ometascan.deb
    ```

=== "RHEL / Rocky Linux"

    ```bash
    # 安裝下載工具
    sudo dnf install -y wget

    # 下載並安裝 MetaDefender Core
    wget "https://software.opswat.com/ometascan.rpm"
    sudo rpm -i ometascan.rpm
    ```

!!! warning "先開好防火牆"
    首次啟動前，請確認防火牆已開放 `8008` 連接埠（管理主控台），否則將無法從瀏覽器連入設定頁面。

## 啟動與授權

安裝完成後，確認服務狀態並開啟管理主控台：

```bash
# 確認服務是否正常運作
sudo systemctl status ometascan

# 瀏覽器開啟以下位址進行初始設定與授權
# http://<主機 IP>:8008
```

依畫面指示輸入授權金鑰即可完成啟用。

!!! tip "LAB 環境小技巧"
    建議搭配 Nginx Proxy Manager 反向代理，綁一個好記的內部網域，就不用每次都記 IP 加連接埠。這也方便之後掛憑證走 HTTPS。

## 常見雷區

??? note "服務起不來、連不進 8008？"
    依序檢查：

    1. `systemctl status ometascan` 看服務是否真的在跑
    2. `ss -tlnp | grep 8008` 確認連接埠有在監聽
    3. 防火牆（`ufw` / `firewalld`）是否放行
    4. 若在雲端主機（如 Azure），別忘了**安全性群組 / NSG** 也要開 8008

*最後更新：2026-08-17*
