# dcoker_base


启动镜像 
dc run -d -i -t -p 9100:3306 centos /bin/bash


https://o9ppsvj8.mirror.aliyuncs.com

https://cr.console.aliyun.com/cn-hangzhou/instances/mirrors

sudo mkdir -p /etc/docker
sudo tee /etc/docker/daemon.json <<-'EOF'
{
  "registry-mirrors": ["https://o9ppsvj8.mirror.aliyuncs.com"]
}
EOF
sudo systemctl daemon-reload
sudo systemctl restart docker