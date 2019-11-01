## envoy

更新yum

```
yum install -y yum-utils
```

添加源
```
yum-config-manager --add-repo https://getenvoy.io/linux/centos/tetrate-getenvoy.repo
```

安装
```
yum install -y getenvoy-envoy
```


查看版本
```
envoy --version
```