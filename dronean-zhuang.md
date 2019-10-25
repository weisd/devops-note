## drone安装，基于docker

拉取docker镜像

```
docker pull drone/drone:1
```

启动 drone, 这里使用的是gogs作为代码仓库
```
docker run \
  --volume=/var/lib/drone:/data \
  --env=DRONE_AGENTS_ENABLED=true \
  --env=DRONE_GOGS_SERVER=https://wps.ktkt.com \
  --env=DRONE_RPC_SECRET=imgkrywfaisg \
  --env=DRONE_SERVER_HOST=dronet.ktkt.com \
  --env=DRONE_SERVER_PROTO=http \
  --publish=1080:80 \
  --publish=1443:443 \
  --restart=always \
  --detach=true \
  --name=drone \
  drone/drone:1
```

