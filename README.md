# 05常用命令
```shell
# 构建镜像
docker build -t 镜像名 .
# 查看进程
docker ps
docker stop 容器id
# 进入容器进行终端输入
docker exec -it 容器id bash

```

# share image
```bash
docker login
docker tag image username/repository:tag
	# for example:
	docker tag friendlyhello john/get-started:part1
docker images
docker push username/repository:tag
```

# docker tool = 在win10之前的Windows上使用

# cmd/docker pull wnameless/oracle-xe-11g = docker下载安装images
# cmd/docker stop 镜像号码 = 停止运行images
# cmd/docker container ls = 列出容器
# cmd/docker pull = 获取镜像
# cmd/docker build = 创建镜像
-t 设置标签名 指定镜像名字
>	docker build -t friendlyhello .

# cmd/docker images = 列出镜像
docker images = 查看镜像
# cmd/docker run = 运行容器
-d = 后台运行
```shell
docker run imageName = 运行镜像
docker run -p 外部端口:容器内端口 repository名 = 运行容器
docker run -d -p 外部端口:容器内端口 repository名 = 后台运行
# examples
docker run -d -p 8080:80 hub.c.163.com/library/nginx
# 映射全部端口到主机的随机端口
docker run -d -P hub.c.163.com/library/nginx
docker ps #查看所有端口
```
# cmd/docker ps = 查看运行的images
# cmd/docker ps -a = 显示所有containers
# cmd/docker rm = 删除容器
# cmd/docker rmi = 删除镜像
# cmd/docker cp = 在host和container之间拷贝文件
# cmd/docker commit = 保存改动为新image
# cmd/docker exec = 进入镜像执行命令
`docker exec -it 镜像号 命令`
	-i 保证输入有效
	-t 分配终端输入命令
使用`docker exec -it <CONTAINER> <COMMAND>`：在容器里执行命令，并输出结果
下面是进入容器的bash命令行
> docker exec -it 0deb75d61474  /bin/bash
exit 退出容器

# cmd/docker restart 容器id = 重启容器

# docker network 网络
网络类型
	bridge = 单独一个namespace来分配独有的网卡等资源
	host = 共用主机上的网络配置
	none = 没有网络连接
端口映射
# 容器网络连接 localhost不能用 要使用主机的ip地址


# dockerfile/from = base image
# dockerfile/run = 执行命令
# dockerfile/add = 添加文件
# dockerfile/copy = 拷贝文件
# dockerfile/cmd = 执行命令
# dockerfile/expose = 暴露端口
# dockerfile/workdir = 指定路径
# dockerfile/maintainer = 维护者
# dockerfile/env = 设定环境变量
# dockerfile/entrypoint = 容器入口
# dockerfile/user = 指定用户
# dockerfile/volume = mount point
# dockerfile/nginx
dockerfile
```bash
# 从 Ubuntu 版本开始
FROM ubuntu
# 维护者是 xbf
MAINTAINER xbf
# 运行命令
RUN sed -i 's/archive.ubuntu.com/mirrros.ustc.edu.cn/g' /etc/apt/sources.list
RUN apt-get update
RUN apt-get install -y nginx
# 把本地文件拷贝到容器里
COPY index.html /var/www/html
# 容器入口 []里最后会用空格隔开执行 意思是让nginx在前台执行
ENTRYPOINT ["/usr/sbin/nginx","-g","daemon off;"]
# 暴露80端口
EXPOSE 80
```
构建镜像
`docker build -t xbf/hello-nginx .`
运行镜像
	-p 把端口80映射到容器80
	xbf/hello-nginx 是容器名字
`docker run -d -p 80:80 xbf/hello-nginx`

# daemon = 守护程序
# compose = 多个容器一起运作
# volume 外置映射存储
```bash
docker run -v /usr/share/ngnix/html nginx

docker run -d --name Nginx -v /usr/share/Nginx/html Nginx

# 检查镜像文件
docker inspect Nginx
# 从配置文件中找到 mount下的source
# 得到如 /var/lib/...
# 打开文件
screen ~/Library/Container/com.docker.docker/Data/com.docker.amd64-linux/tty

# 当前位置下的code文件夹映射到镜像中dhtml文件夹 运行 Nginx镜像;
docker run -v $PWD/code:/var/www/html Nginx

# docker run -volumes-from ...
docker create -v $PWD/data:/var/mydata --name data_container
docker create -v $PWD/data:/var/mydata --name data_container Ubuntu
```
# first & simplest dockerfile
```makefile
FROM alpine:latest
MAINTAINER xbf
CMD echo 'hello docker'
```

# dockerfile/jpress
```makefile
from hub.c.163.com/library/tomcat
maintainer xxx xxx@163.com
copy jpress.war /usr/local/tomcat/webapps
```

# dockfile/explain 解说
```makefile
# 设置参数
ARG cuda_version=8.0
ARG cudnn_version=6
# 继承的镜像
from hub.c.163.com/library/Tomcat
# 维护者 填写个人信息
maintainer xxx xxx@163.com
# 复制 本地路径 镜像里路径[可以通过镜像文档查看环境变量]
copy jpress.war /usr/local/tomcat/webapps
# 设置环境变量
ENV TZ="Hongkong"
ENV LANG=zh_CH.utf8
ENV PATH /root/anaconda3/bin:$PATH
# 设置用户
USER keras
# Define environment variable
ENV NAME World
# Make port 80 available to the world outside this container
EXPOSE 80
# Set the working directory to /app
WORKDIR /app
# Copy the current directory contents into the container at /app
ADD . /app
# 执行命令
cmd echo 'hello docker'
RUN localedef -c -i zh_CN -f UTF-8 zh_CN.UTF-8
RUN yum install -y bzip2 \
    && yum install -y gcc
# Run app.py when the container launches
CMD ["python", "app.py"]
CMD jupyter notebook --port=8888 --ip=0.0.0.0
```

# dockerfile example
```makefile
FROM centos
LABEL maintainer="yourname <xxx@xxx.com>"

# set timezone
ENV TZ="Hongkong"

# install zh_CN.utf8
RUN localedef -c -i zh_CN -f UTF-8 zh_CN.UTF-8
ENV LANG=zh_CH.utf8

# install something by yum
RUN yum install -y bzip2 \
    && yum install -y gcc

# install anaconda
COPY ./Anaconda3-4.4.0-Linux-x86_64.sh /tmp/Anaconda3-4.4.0-Linux-x86_64.sh
WORKDIR /tmp
RUN sh -c '/bin/echo -e "\nyes\n\nyes" | sh Anaconda3-4.4.0-Linux-x86_64.sh'
ENV PATH /root/anaconda3/bin:$PATH

# install some lib by pip
COPY ./*.whl /tmp/
RUN pip install Keras-2.0.8-py2.py3-none-any.whl \
    && pip install tensorflow-1.3.0-cp36-cp36m-manylinux1_x86_64.wh

```

# dockerfile python
```makefile
# Use an official Python runtime as a parent image
FROM python:2.7-slim

# Set the working directory to /app
WORKDIR /app

# Copy the current directory contents into the container at /app
ADD . /app

# Install any needed packages specified in requirements.txt
RUN pip install -r requirements.txt

# Make port 80 available to the world outside this container
EXPOSE 80

# Define environment variable
ENV NAME World

# Run app.py when the container launches
CMD ["python", "app.py"]
```

requirements.txt
```
Flask
Redis
```

app.py
```python
from flask import Flask
from redis import Redis, RedisError
import os
import socket

# Connect to Redis
redis = Redis(host="redis", db=0, socket_connect_timeout=2, socket_timeout=2)

app = Flask(__name__)

@app.route("/")
def hello():
    try:
        visits = redis.incr("counter")
    except RedisError:
        visits = "<i>cannot connect to Redis, counter disabled</i>"

    html = "<h3>Hello {name}!</h3>" \
           "<b>Hostname:</b> {hostname}<br/>" \
           "<b>Visits:</b> {visits}"
    return html.format(name=os.getenv("NAME", "world"), hostname=socket.gethostname(), visits=visits)

if __name__ == "__main__":
    app.run(host='0.0.0.0', port=80)
```

# dockerfile/keras
[Using Keras via Docker](https://github.com/fchollet/keras/tree/master/docker)
```makefile
ARG cuda_version=8.0
ARG cudnn_version=6
FROM nvidia/cuda:${cuda_version}-cudnn${cudnn_version}-devel

ENV CONDA_DIR /opt/conda
ENV PATH $CONDA_DIR/bin:$PATH

RUN mkdir -p $CONDA_DIR && \
    echo export PATH=$CONDA_DIR/bin:'$PATH' > /etc/profile.d/conda.sh && \
    apt-get update && \
    apt-get install -y wget git libhdf5-dev g++ graphviz openmpi-bin && \
    wget --quiet https://repo.continuum.io/miniconda/Miniconda3-4.2.12-Linux-x86_64.sh && \
    echo "c59b3dd3cad550ac7596e0d599b91e75d88826db132e4146030ef471bb434e9a *Miniconda3-4.2.12-Linux-x86_64.sh" | sha256sum -c - && \
    /bin/bash /Miniconda3-4.2.12-Linux-x86_64.sh -f -b -p $CONDA_DIR && \
    rm Miniconda3-4.2.12-Linux-x86_64.sh

ENV NB_USER keras
ENV NB_UID 1000

RUN useradd -m -s /bin/bash -N -u $NB_UID $NB_USER && \
    mkdir -p $CONDA_DIR && \
    chown keras $CONDA_DIR -R && \
    mkdir -p /src && \
    chown keras /src

USER keras

# Python
ARG python_version=3.5

RUN conda install -y python=${python_version} && \
    pip install --upgrade pip && \
    pip install tensorflow-gpu && \
    pip install https://cntk.ai/PythonWheel/GPU/cntk-2.1-cp35-cp35m-linux_x86_64.whl && \
    conda install Pillow scikit-learn notebook pandas matplotlib mkl nose pyyaml six h5py && \
    conda install theano pygpu bcolz && \
    pip install sklearn_pandas && \
    git clone git://github.com/fchollet/keras.git /src && pip install -e /src[tests] && \
    pip install git+git://github.com/fchollet/keras.git && \
    conda clean -yt

ADD theanorc /home/keras/.theanorc

ENV PYTHONPATH='/src/:$PYTHONPATH'

WORKDIR /src

EXPOSE 8888

CMD jupyter notebook --port=8888 --ip=0.0.0.0
```


# sources
[用 docker 搭建 python 量化计算环境](http://www.jianshu.com/p/26c8b7fd6f08)
