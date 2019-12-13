# Prometheus（普罗米修斯）部署

## 安装Prometheus

### 下载

wget https://github.com/prometheus/prometheus/releases/download/v2.14.0/prometheus-2.14.0.linux-amd64.tar.gz

### 解压

tar xvfz prometheus-2.14.0.linux-amd64.tar.gz

### 重命名
mv prometheus-2.14.0.linux-amd64 prometheus

### 进入目录

cd prometheus

### 查看版本

./prometheus --version

### 配置

cat prometheus.yml

### 运行服务

./prometheus --config.file=prometheus.yml

### 创建一个Systemd服务

vi /etc/systemd/system/prometheus.service

### 将以下内容复制到文件内，保存并退出

```
[Unit]
Description=Prometheus Monitoring
Wants=network-online.target
After=network-online.target

[Service]
ExecStart=/data/prometheus/prometheus \
--config.file /data/prometheus/prometheus.yml
WorkingDirectory=/data/prometheus

[Install]
WantedBy=multi-user.target
```

### 重启Systemd

systemctl daemon-reload

### 启动Prometheus

systemctl start prometheus

### 查看状态

systemctl status prometheus

### 开启Prometheus的开机自启动

systemctl enable prometheus

## 安装Node Exporter

### 下载

wget https://github.com/prometheus/node_exporter/releases/download/v0.18.1/node_exporter-0.18.1.linux-amd64.tar.gz

### 解压

tar xvfz node_exporter-0.18.1.linux-amd64.tar.gz

### 重命名
mv node_exporter-0.18.1.linux-amd64 node_exporter

### 进入目录

cd node_exporter-0.18.1.linux-amd64

### 监控采集服务

./node_exporter

### 查看是否成功

curl http://localhost:9100/metrics

### 成功后，prometheus/prometheus.yml加上target

```
scrape_configs:
  - job_name: 'prometheus'
    static_configs:
      - targets: ['localhost:9090']
  - job_name: 'server'
    static_configs:
      - targets: ['localhost:9100']
```

### 创建一个Systemd服务

vi /etc/systemd/system/node_exporter.service

### 将以下内容复制到文件内，保存并退出

```
[Unit]
Description=Node Exporter
Wants=network-online.target
After=network-online.target

[Service]
ExecStart=/data/node_exporter
WorkingDirectory=/data/node_exporter/node_exporter

[Install]
WantedBy=multi-user.target
```

### 重启Systemd

systemctl daemon-reload

### 启动Node Exporter

systemctl start node_exporter

### 查看状态

systemctl status node_exporter

### 开启Node Exporter的开机自启动

systemctl enable node_exporter

## 安装Grafana

### 下载

wget https://dl.grafana.com/oss/release/grafana-6.4.4.linux-amd64.tar.gz 

### 解压

tar -zxvf grafana-6.4.4.linux-amd64.tar.gz

### 重命名
mv grafana-6.4.4 grafana

### 进入目录

cd grafana

### 启动

./bin/grafana-server web

### 修改端口号

默认端口号为3000，通过配置文件进行修改：创建名为custom.ini的配置文件，添加到conf文件夹，复制conf/defaults.ini中定义的所有设置，然后修改自己想要修改的

### 登录

IP:3000，默认账号密码：admin/admin，首次登录需修改密码：GfNewPass4!

### 添加Prometheus的数据源

* 点击Grafana Logo打开侧边栏
* 在侧边栏内，点击设置图标中的Data Sources
* 点击Add data source，选择Prometheus作为数据源
* 设置Prometheus服务的URL（Prometheus Server设置的ip+端口号，如果没改过且在本机运行的话，那就是localhost:9090）
* 点击Save&Test即可测试连接并保存为新的数据源

### 添加dashboard（监控面板）

在刚配好的Prometheus Data Source的设置中有一个标签就是dashboard，导入Prometheus 2.0 Stats这个面板

### 添加node_exporter

* 点击Grafana Logo打开侧边栏
* 在侧边栏内，点击添加图标中的Import
* 在[Grafana的官方面板页面](https://grafana.com/grafana/dashboards)找到想要的面板
* 复制右侧面板ID，在Import界面输入ID，Load后配置好数据源为我们的Prometheus
* 需要安装饼图插件grafana-piechart-panel

### 安装插件

// 进入Grafana/bin目录
./grafana-cli plugins install [插件名]
// 安装成功后重启Grafana

### 创建一个Systemd服务

vi /etc/systemd/system/grafana.service

### 将以下内容复制到文件内，保存并退出

```
[Unit]
Description=Grafana
Wants=network-online.target
After=network-online.target

[Service]
ExecStart=/data/grafana/bin/grafana-server web
WorkingDirectory=/data/grafana/bin

[Install]
WantedBy=multi-user.target
```

### 重启Systemd

systemctl daemon-reload

### 启动Grafana

systemctl start grafana

### 查看状态

systemctl status grafana

### 开启Grafana的开机自启动

systemctl enable grafana


