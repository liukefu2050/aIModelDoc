
# 本地运行 coze-studio 快速指南


## 获取代码

```
git clone https://github.com/coze-dev/coze-studio.git
```

## 部署并启动服务

```
cd coze-studio
# start service
# for macOS or Linux
make web  
# for windows
cp ./docker/.env.example ./docker/.env
docker compose -f ./docker/docker-compose.yml up
```
报错：
unable to get image 'nsqio/nsq:v1.2.1': error during connect: Get "http://%2F%2F.%2Fpipe%2FdockerDesktopLinuxEngine/v1.51/images/nsqio/nsq:v1.2.1/json": open //./pipe/dockerDesktopLinuxEngine: The system cannot find the file specified.

解决：先启动Docker Desktop

