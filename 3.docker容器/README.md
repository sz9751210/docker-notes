## 容器基本操作
### 啟動容器
1.新建並啟動
```shell
docker run -it ubuntu:14.04 bash

## -i:讓容器的標準輸入保持打開
## -t:讓docker分配一個偽終端(pseudo-tty)，並綁定到容器的標準輸入上
```

docker run後台操作順序
- 檢查本地是否有此鏡像，沒有就從docker hub下載
- 利用鏡像創建並啟動一個容器
- 分配一個文件系統，並在只讀的鏡像層外面掛載一層可讀寫層
- 從宿主主機配置的bridge中橋接一個虛擬接口到容器中
- 從地址池配置一個ip地址給容器
- 執行用戶指定的應用程序
- 執行完畢後容器將被終止

2. 啟動已終止容器
```shell
docker start container-id
```

### 背景執行容器
```shell
docker run -d ubuntu:14.04 /bin/sh -c "while true; do echo hello world; sleep 1; done"

docker logs container-name 可查看log

## -d:讓docker容器在後台以守護進程(daemonized)形式運行
```
### 終止容器
```shell
docker stop 
```
### 重啟容器
```shell
docker restart
## 此指令會先將容器終止，再重新啟動
```
### 進入容器
```shell
docker attach

docker exec

```

>attach主要用於附加到正在運行的進程，而exec則是專門用於在已經啟動的容器中運行新進程
### 導入和導出容器
1. 導入容器快照
```shell
cat mysnapshot.tar | docker import - test/ubuntu:v1.0
```

2. 導出容器
```shell
docker export container-id > mysnapshot.tar
```

>docker load為導入image，會保存完整紀錄，size較大
>
>docker import為導入container，只保存容器當時的快照狀態，會丟棄歷史紀錄與元數據，所以可重新指定tag
### 刪除容器
```shell
docker rm
```
> 如果要刪除運行中的容器，可使用`-f`參數，docker會發送SIGKILL信號給容器