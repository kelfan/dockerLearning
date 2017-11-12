```bash
# 从 Ubuntu 版本开始
FROM ubuntu
# 维护者是 xbf
MAINTAINER xbf
# 运行命令
# RUN sed -i 's/archive.ubuntu.com/mirrros.ustc.edu.cn/g' /etc/apt/sources.list
RUN apt-get update
RUN apt-get install -y nginx
# 把本地文件拷贝到容器里
# COPY index.html /var/www/html
# 容器入口 []里最后会用空格隔开执行 意思是让nginx在前台执行
ENTRYPOINT ["/usr/sbin/nginx","-g","daemon off;"]
# 暴露80端口
EXPOSE 80
```
