## docker倉庫
### docker hub
```shell
docker search centos

## 可直接搜尋docker hub的image，可在搜尋時添加-s N指定星星數

docker login

docker push

## 註冊docker hub可將image推到docker hub上
```
### 私有倉庫
#### 安裝
1. docker image
```shell
docker run -d -p 5000:5000 registry

## 指定官方registry鏡像運行registry
```

2. ubuntu
```shell
apt-get install -y build-essential python-dev libevent-dev python-pip liblzma-dev
pip install docker-registry
```

3. ubuntu
```shell
yum install -y python-devel libevent-devel python-pip gcc xz-devel 
python-pip install docker-registry
```

4. 源碼安裝
```shell
apt-get install build-essential python-dev libevent-dev python-pip libssl-dev liblzma-dev libf 
git clone https://github.com/docker/docker-registry.git
cd docker-registry
python setup.py install
cp config/config_sample.yml config/config.yml
gunicorn -c contrib/gunicorn.py docker_registry.wsgi:application

```

#### 操作
假設registry ip為192.168.0.1，則可以透過tag的方式做一個標記
```
docker tag myimage 192.168.0.1:5000/test 
```
[自動上傳到本地registry腳本](https://github.com/yeasy/docker_practice/blob/zh-Hant/_local/push_images.sh)

### config檔
```shell
common:
    loglevel: info
    search_backend: "_env:SEARCH_BACKEND:" sqlalchemy_index_database:"_env:SQLALCHEMY_INDEX_DATABASE:sqlite:////tmp/docker-registry.db"
prod:
    loglevel: warn
    storage: s3
    s3_access_key: _env:AWS_S3_ACCESS_KEY s3_secret_key: _env:AWS_S3_SECRET_KEY s3_bucket: _env:AWS_S3_BUCKET boto_bucket: _env:AWS_S3_BUCKET storage_path: /srv/docker
    smtp_host: localhost
    from_addr: docker@myself.com to_addr: my@myself.com
dev:
    loglevel: debug
    storage: local
    storage_path: /home/myself/docker
test:
    storage: local
    storage_path: /tmp/tmpdockertmp
```
[官方config檔參考](https://docs.docker.com/registry/configuration/)