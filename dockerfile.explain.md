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
