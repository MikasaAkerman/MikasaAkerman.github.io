###	Jenkins

- docker-compose.yml

  ```yaml
  version: "3"
  
  services:
      jenkins:
          image: jenkins/jenkins:alpine
          restart: always
          container_name: jenkins
          user: root
          volumes:
           - /root/jenkins/data:/var/jenkins_home
           - /var/run/docker.sock:/var/run/docker.sock
           - /usr/bin/docker:/usr/bin/docker
          ports:
           - "12000:8080"
           - "12001:50000"
          networks:
           - devops_network
  
  networks:
    devops_network:
  ```

- 部署

  - build

    ```shell
    #!/bin/bash
    
    cd $WORKSPACE
    
    echo "go version"
    go version
    echo "docker version"
    docker --version
    
    # docker login -u username -p password
    cd cmd
    export GOPROXY=https://goproxy.cn,direct
    CGO_ENABLED=0 GOOS=linux GOARCH=amd64 go build -o launchapp
    
    cd ..
    docker build -t lastdaysinmemory/kms:alpine .
    rm cmd/launchapp
    
    # docker push lastdaysinmemory/kms:alpine
    ```

  - deploy

    ```shell
    #!/bin/bash
    
    cd /root/proj/kms
    docker-compose config
    docker rm -f kms
    docker-compose up -d
    ```

- 安装慢
[https://mirrors.tuna.tsinghua.edu.cn/jenkins/updates/update-center.json](https://mirrors.tuna.tsinghua.edu.cn/jenkins/updates/update-center.json)

- 错误处理

  - Jenkins执行docker和go命令报错

    _alpine识别不了Cent-OS的可执行文件，换成默认镜像即可解决_

  - 汉化
  
    1. 将Default Language设置为zh_US
    2. 调用 JENKINS/restart
    3. 将Default Language设置为zh_CN
  