# devops学习

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


