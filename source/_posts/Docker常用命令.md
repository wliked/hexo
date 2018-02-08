---
title: Docker常用命令
date: 2018-02-08 14:20:00
tags: docker
category: docker
---

本文介绍常用的Docker命令和一个最简单的Dockerfile示例.

<!--more-->

# 常用命令

    ``` bash
    docker ps                           # 显示所有running docker container, 可以看到容器的名称和id
    docker ps -a                        # 显示所有docker container，包含已停止运行的
    docker stop {container_name/id}     # 停止某个running 的容器
    docker rm {container_name/id}       # 删除一个停止的容器
    docker start {container_name/id}    # 重新启动某个stop的容器
    docker attach {container_name/id}   # attach 到一个已运行的容器中进程的stdin, 如果从这个stdin 中exit, 会导致容器的停止

    docker run -d -p 10022:22 herong/centos7-ssh:latest /usr/sbin/sshd -D  # detach 模式运行容器，并将宿主机端口10022映射到容器端口22
    docker port 4966d35fe0a3                #查看容器的端口映射情况, 4966d35fe0a3目标容器的id

    docker images                                   # 显示所有本地的image
    docker rim {image_name/id}                      # 删除一个image, 如果有容器引用了该镜像, 则需先删除容器
    docker run docker/whalesay cowsay boo-boo       # run一个image: docker/whalesay
    docker run -it docker/whalesay                  # 交互式方式 run 一个容器
    docker exec -it {container_name/id} /bin/bash   # 在已启动的容器中启动一个交互式命令行, 这个命令行进程与容器中已运行的进程无相互影响, 不会像attach方式因为退出导致整个容器退出

    docker login            # 输入dockerhub 用户名密码登陆dockerhub
    docker search python    # 在dockerhub 上搜索镜像
    docker pull python      # 从dockerhub pull 一个镜像
    docker run yourusername/docker-whale        #从docker hub下载image并run
    docker commit -a wliked -m "comments" {container_name/id}  wliked/vacation_system:v1.0   # 从正在running的容器commit 一个镜像
    docker tag vacation_system wliked/vacation_system:v1.0    #更改镜像名称并将其加入一个repo (wliked) , 这个repo一般是dockerhub用户名,或私有镜像仓库中的用户名
    docker push wliked/vacation_system:v1.0                   # 打上repo名后,可以向dockerhub push 镜像

    ```

# 简单Dockerfile示例

   ``` bash
   mkdir dockerbuild
   cd dockerbuild
   touch Dockerfile  # 新建文件并输入如下Doerkfile内容:
       FROM docker/whalesay:latest
       RUN apt-get -y update && apt-get install -y fortunes  #可以有多个RUN命令 # RUN 命令会基于已有的镜像层run一个容器起来, 在里面执行命令, 然后docker commit一个镜像, 每个RUN命令都对应这样一个动作; 所以镜像是一层层build出来的
       CMD /usr/games/fortune -a | cowsay
   ```

# 根据Dockerfile build一个镜像

    ``` bash
    docker build -t docker-whale .  # build 一个叫 docker-whale 的image, 注意后面的 . 号, 表示 Dockerfile的路径
    docker run docker-whale         # 运行自己刚build 出来的image
    docker tag 7d9495d03763 maryatdocker/docker-whale:latest   # 为image打tag
    ```
