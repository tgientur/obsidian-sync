信息源：财联社、金十数据、东方财富、雅虎
定时推送 8：00 & 17：00
AI总结分析
推送平台  钉钉 个人微信 公众号

安装的主要问题就是***代理问题*** ，网络连接不上，一个是云服务器没有连接上，导致git clone的时出错，一个是docker也要配置代理，问题是docker compose pull 和docker compose build -p的时候出错


# 重启
sudo docker compose restart
# 查看容器状态
sudo docker compose ps

# 查看最新日志
sudo docker logs --tail 30 trendradar

#手动执行一次并推送 
sudo docker exec -it trendradar python manage.py run


测试：
1. curl -X POST -H "Content-Type: application/json" -d '{"msg_type":"text","content":{"text":"Hello! TrendRadar 测试消息"}}' "https://open.feishu.cn/open-apis/bot/v2/hook/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"

curl -x http://172.29.48.212:7890 https://www.google.com -I --connect-timeout 3




阿里云服务器里可以直接用？？
那我之前用宝塔都是啥，还不让复制

安装nano
sudo yum install -y nano

安装git
sudo yum install -y git

配置阿里云镜像加速
sudo mkdir -p /etc/docker
sudo tee /etc/docker/daemon.json <<-'EOF'
{
  "registry-mirrors": ["https://8aikki2b.mirror.aliyuncs.com"]
}
EOF
sudo systemctl daemon-reload
sudo systemctl restart docker
查看地址
sudo docker info | grep -A 5 "Registry Mirrors"

修改TrendRadar的配置文件
# 修改配置文件
vim config/config.yaml
vim config/frequency_words.txt


代理
ssh -D 1080 -C -N root@<115.29.196.17>

云服务器代理ssh -NfR 7890:localhost:7890 -p 22 root@115.29.196.17
alyun 47.80.12.193
tencent 150.158.3.58
ssh -NfR 7890:localhost:7890 -p 22 root@150.158.3.58

ssh -C2qTnN -D 1080 root@150.158.3.58 -p 22
ssh -NfR 7890:localhost:7890 -p 22 root@47.80.12.193
然后挂着poweshell窗口就好了

云服务器自检
netstat -tlnp | grep 7890
有下面这条就是成功
tcp  0  0 127.0.0.1:7890  0.0.0.0:*  LISTEN  12345/sshd: root
netstat -tlnp | grep 7890


设置
export http_proxy=http://127.0.0.1:7890
export https_proxy=http://127.0.0.1:7890

清除
unset http_proxy https_proxy



# 创建 Docker 服务配置目录
sudo mkdir -p /etc/systemd/system/docker.service.d

# 创建代理配置文件
sudo tee /etc/systemd/system/docker.service.d/proxy.conf <<-'EOF'
[Service]
Environment="HTTP_PROXY=http://127.0.0.1:7890"
Environment="HTTPS_PROXY=http://127.0.0.1:7890"
Environment="NO_PROXY=localhost,127.0.0.1,.local"
EOF
# 重新加载 systemd 配置
sudo systemctl daemon-reload

# 重启 Docker 服务（这一步会让 Docker 守护进程应用新配置）
sudo systemctl restart docker





