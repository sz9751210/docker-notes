## 鏡像基本操作
### 拉取鏡像
```shell
docker pull ubuntu:14.04
```

### 列出本地鏡像
```shell
docker images

REPOSITORY      TAG      IMAGE ID       CREATED         SIZE
來自哪個倉庫      標記      ID(唯一)       (創建時間)       (鏡像大小)
```

### 創建鏡像
1. 修改已有鏡像
```shell
docker run -it ubuntu:14.04 bash

touch test

docker commit -m "add test" -a "myuser" container-id ubuntu:14.04-v1
```

2. 使用Dockerfile創建鏡像
```shell
# 使用-t添加tag
docker build -t="image-name:image-tag" .
```

### 上傳鏡像
```shell
docker push image-name
```

### 保存以及載入鏡像
```shell
# 保存鏡像
docker save -o ubuntu_14.04.tar ubuntu:14.04

# 載入鏡像
docker load --input ubuntu_14.04.tar or docker load < ubuntu_14.04.tar
```

### 本地系統導入鏡像
```shell
cat ubuntu_14.04.tar | docker import - ubuntu:14.04-v2
```

### 移除鏡像
```shell
docker rmi image
```
>必須確保沒有容器在使用此鏡像才能移除

### 實現原理
鏡像是由許多層(layer)構成，透過Union FS將不同的層結合到一個鏡像

Union FS兩大用途
1. 實現不借助LVM、RAID將多個disk掛到同一個目錄下
2. 將一個只讀分支和一個可寫分支聯合在一起

案例：Live CD使用此方式允許在鏡像不變的基礎上讓用戶可以在其上進行一些寫操作，而Docker則是在AUFS上使用類似原理構建容器