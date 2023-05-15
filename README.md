# devops学习

## 配置template虚拟机

下载vmware，百度自己找个例子

配置虚拟机 

https://blog.csdn.net/weixin_44098426/article/details/128404337

https://www.runoob.com/w3cnote/vmware-install-centos7.html

## 安装Mobaxterm终端链接工具

https://mobaxterm.mobatek.net/

## 为虚拟机添加外网可以访问的端口映射
用花生壳做内网穿透https://hsk.oray.com/
获得虚拟机内网地址
![](pic/2023-05-12-11-21-44.png)


## 1. Docker

### 1.1 安装docker

安装依赖 
```
yum -y install yum-utils device-mapper-persistent-data lvm2yy
```

设置下载docker的镜像源为阿里云
```
yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
```

安装docker
```
yum -y install docker-ce
```

启动docker服务
```
systemctl start docker
```

设置开机自动启动
```
systemctl enable docker
```
### 1.2 安装docker compose
去github上搜索docker/compose
![](pic/2023-05-12-10-20-23.png)

安装tag上不带点的，稳定的，下载docker-compose-linux-x86_64
![](pic/2023-05-12-10-21-57.png)

用Mobaxterm的SFTP连接把下载的文件上传到服务器。
重命名文件，加执行权限，并将其放入$PATH目录里。
```
mv docker-compose-linux-x86_64.64 docker-compose
chmod +x docker-compose
echo $PATH
mv docker-compose /usr/bin/
docker-compose version
```

## 1. GITLAB

### 1.1 Docker安装GITLAB

* docker pull gitlab/gitlab-ce

* 新建docker-compose.yml
```
version: '3.1'
services:
  gitlab:
    image: 'gitlab/gitlab-ce'
    container_name: gitlab
    restart: always
    environment:
      GITLAB_OMNIBUS_CONFIG: |
        external_url 'http://127.0.0.1:8929'
        gitlab_rails['gitlab_shell_ssh_port'] = 2224
    ports:
      - '8929:8929'
      - '2224:2224'
    volumes:
      - './config:/etc/gitlab'
      - './logs:/var/log/gitlab'
      - './data:/var/opt/gitlab'
```
docker-compose up -d

* 多等一会儿，然后浏览器输入127.0.0.1:8929检验

### 1.2 GITLAB的root密码

docker exec -it gitlab bash   
cat /etc/gitlab/initial_root_password
![](pic/2023-05-08-11-10-58.png)


