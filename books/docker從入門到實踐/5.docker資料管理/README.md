## docker volume
### data volume
#### 簡介
可提供一個或多個容器使用的特殊目錄，繞過UFS，有以下幾點特性
- 可在容器之間共享
- 對data volume的修改立即生效
- 對data volume的更新不影響image
- 生命週期為直到沒有容器使用

#### 創建data volume
```shell
docker run -it -v /myvolume centos:centos7 /bin/bash

## 儲存的位置可透過docker inspect container-id查看
docker inspect container-id

## 如果要查看指定volume則可以透過docker volume指令下去做查看
docker volume inspect volume-name
```
>因為沒有mapping，所以儲存的位置會在/var/lib/docker/volumes/下
#### 掛載本機目錄作為data volume
```shell
docker run -it -v /host-path:/container-path centos:centos7 /bin/bash
```
>如果要讓data volume權限為只讀，可透過-v /host-path:/container-path:ro的方式下去做設定

#### 掛載單檔作為data volume
```shell
## 紀錄在容器內輸入過的命令
docker run --rm -it -v ~/.bash_history:/.bash_history ubuntu /bin/bash
```
### data volume container
#### 簡介
適用於持續更新的數據需要在容器之間共享，data volume container本質上就是一個容器，專門用來提供數據卷給其他容器掛載，生命週期為此data volume container的data volume已無任何容器掛載
#### 創建data volume container並讓容器掛載
```shell
docker run -d -v /dbdata --name dbdata training/postgres echo Data-only container for postgres

docker run -d --volumes-from dbdata --name db1 training/postgres
docker run -d --volumes-from dbdata --name db2 training/postgres
docker run -d --volumes-from db1 --name db3 training/postgres
```
> `--volume-from`可掛載多個data volume container，或是從其他已經掛載data volume container的容器掛載，使用此參數掛載data volume container的容器自己並不需要保持在運行狀態


### 備份、恢復、搬遷data volume
#### 備份
使用`volumes-from`創建一個加載dbdata的volume container的容器，並從本地掛載當前目錄到容器的/backup目錄
```shell
docker run --volumes-from dbdata -v $(pwd):/backup ubuntu tar cvf /backup/backup.tar /dbdata
```
> 容器啟動後，使用`tar`將dbdata卷備份為本地的`/backup/backup.tar`

#### 恢復
先創建一個新的data volume container叫dbdata2
```shell
docker run -v /dbdata --name dbdata2 ubuntu /bin/bash
```
接著創建另一個容器，掛載dbdata2的volume container，並使用untar解壓備份文件到掛載的容器卷中
```shell
docker run --volumes-from dbdata2 -v $(pwd):/backup busybox tar xvf /backup/backup.tar
```
