## docker網路
### 外部訪問容器
```shell
docker run -d -P nginx

使用`-P`會隨機分配49000-49900的port映射到內部容器開放的port，查看分配的port可透過`docker ps`
```

使用`-p`可自定義映射port號格式如下
ip:hostPort:containerPort | ip::containerPort | hostPort:containerPort
1. 映射所有port ip-address
```shell
docker run -d -p 81:80 nginx

CONTAINER ID   IMAGE     COMMAND                  CREATED         STATUS         PORTS                                 NAMES
e23a886fc786   nginx     "/docker-entrypoint.…"   3 seconds ago   Up 2 seconds   0.0.0.0:81->80/tcp, :::81->80/tcp   condescending_tesla
```
2. 映射到指定ip-address的指定port
```shell
docker run -d -p 127.0.0.1:81:80 nginx

CONTAINER ID   IMAGE     COMMAND                  CREATED         STATUS        PORTS                  NAMES
335f065c6c20   nginx     "/docker-entrypoint.…"   2 seconds ago   Up 1 second   127.0.0.1:81->80/tcp   admiring_shannon

## 指定udp port
docker run -d -p 127.0.0.1:80:80/udp nginx

CONTAINER ID   IMAGE     COMMAND                  CREATED         STATUS        PORTS                          NAMES
e3429e9b6b27   nginx     "/docker-entrypoint.…"   2 seconds ago   Up 1 second   80/tcp, 127.0.0.1:80->80/udp   gracious_ardinghelli

## 指定多個port
docker run -d -p 80:80 -p 443:443 nginx
```
3. 映射到指定ip-address的任意port
```shell
docker run -d -p 127.0.0.1::80 nginx

CONTAINER ID   IMAGE     COMMAND                  CREATED         STATUS        PORTS                     NAMES
6ad1104a139a   nginx     "/docker-entrypoint.…"   2 seconds ago   Up 1 second   127.0.0.1:49154->80/tcp   zen_engelbart
```
#### 查看映射port配置
```shell
## 查看當前映射的port配置，以及綁定的ip-address
docker port container-id
```
### 容器互聯
透過`--link`讓使用此選項的container添加link對象的host以及相關環境變量
```shell
## 創建db後再建立web服務
docker run -d --name db training/postgres

docker run -d -P --name web --link db:db training/webapp python app.py

## 檢視是否有成功link
docker exec -it web env 
docker exec -it web cat /etc/hosts
docker exec -it web ping db
```