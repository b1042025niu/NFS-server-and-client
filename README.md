# 組員
B1042002 林子期
B1042004 王博民
B1042025 麥俊雲
# 連結：https://www.youtube.com/watch?v=WP8MdkKlmbI
# 目錄
- [NFS 伺服器架設](#nfs-伺服器架設)
  - [NFS 簡單介紹](#nfs-簡單介紹)
  - [安裝伺服器端](#安裝伺服器端)
  - [NFS 伺服器服務指令](#nfs-伺服器服務指令)
  - [建立共享目錄](#建立共享目錄)
  - [分配客戶端權限](#分配客戶端權限)
  - [NFS 伺服器防火牆設定](#nfs-伺服器防火牆設定)
- [NFS 客戶端安裝](#nfs-客戶端安裝)
# NFS 伺服器架設
## NFS 簡單介紹
網路檔案系統（英語：Network File System，縮寫作NFS）是一種分散式檔案系統，力求客戶端主機可以存取伺服器端檔案，並且其過程與存取本地儲存時一樣。
## NFS 伺服器端安裝
### 安裝伺服器端
```
sudo apt install nfs-kernel-server
```
### NFS 伺服器服務指令
安裝好 NFS 伺服器套件之後，該服務會自動啟動，我們可以透過以下指令查看、啟動或停止 NFS 伺服器服務
- 查看 NFS 伺服器服務狀態
```
systemctl status nfs-server
```
![img](https://github.com/MaiMike666/NFS-server-and-client/blob/main/img/nfs_sever_status.png)
- 啟動 NFS 伺服器服務狀態
```
systemctl start nfs-server
```
- 停止 NFS 伺服器服務狀態
```
systemctl stop nfs-server
```
- 重新啟動 NFS 伺服器服務狀態
```
systemctl restart nfs-server
```
## 建立共享目錄
- 建立共享目錄
```
sudo mkdir -p /mnt/sharedfolder_server
```
我們希望所有客戶端都可以訪問該目錄，因此我們通過以下命令刪除限制的權限
- 讓該目錄可以被任何人訪問
```
sudo chown nobody:nogroup /mnt/sharedfolder_server
```
- 讓該目錄可以被讀取、執行、寫入
```
sudo chmod 777 /mnt/sharedfolder_server
```
## 分配客戶端權限
我們需要為客戶端提供訪問主機服務器的權限。此權限是通過位於系統 /etc 文件夾中的文件。請使用以下命令通過 Nano 編輯器打開此文件
- 使用 nano 編輯
```
sudo nano /etc/exports
```
- 在文件中添加以下設定
```
/mnt/sharedfolder_server clientIP(rw,sync,no_subtree_check)
```
```
# rw: 可以進行讀寫操作
# sync: 在儲存之前可以將任何更改寫入硬碟
# no_subtree_check: 防止子樹檢查
```
![img](https://github.com/MaiMike666/NFS-server-and-client/blob/main/img/nfs_etc.png)
## NFS 伺服器防火牆設定
- 允許客戶端通過防火牆
```
sudo ufw allow from [clientIP or clientSubnetIP] to any port nfs
```
![img](https://github.com/MaiMike666/NFS-server-and-client/blob/main/img/nfs_ufw.png)
- 檢查防火牆狀態
```
sudo ufw status
```
![img](https://github.com/MaiMike666/NFS-server-and-client/blob/main/img/nfs_ufw_status.png)
# NFS 客戶端安裝
## NFS 客戶端
- NFS 客戶端安裝
```
sudo apt-get install nfs-common
```
- 為 NFS 主機的共享文件夾創建掛載點
```
sudo mkdir -p /mnt/sharedfolder_client
```
- 在客戶端掛載共享目錄
```
sudo mount serverIP:/mnt/sharedfolder_server /mnt/sharedfolder_client
```
# 最終完成畫面
![img](https://github.com/MaiMike666/NFS-server-and-client/blob/main/img/nfs_f.png)
# 參考資料
[NFS WIKI](https://zh.wikipedia.org/zh-tw/%E7%BD%91%E7%BB%9C%E6%96%87%E4%BB%B6%E7%B3%BB%E7%BB%9F)

[Install NFS Server and Client on Ubuntu](https://vitux.com/install-nfs-server-and-client-on-ubuntu/)

[Linux 安裝與使用 NFS 伺服器、用戶端教學與範例](https://officeguide.cc/linux-nfs-server-client-installation-configuration-tutorial-examples/)
