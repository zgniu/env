FROM ubuntu:latest

RUN apt-get update \ 
&& apt-get -y upgrade 

# 安装 tzdata 
ENV  DEBIAN_FRONTEND=noninteractive
RUN apt-get install -y tzdata
RUN ln -fs /usr/share/zoneinfo/Asia/Chongqing /etc/localtime
RUN dpkg-reconfigure --frontend noninteractive tzdata


RUN mkdir -p /etc/nginx/conf.d
RUN apt-get install -y nginx
RUN mkdir -p  /var/log/nginx
# 网站目录
RUN mkdir /www
# RUN chmod -r 

CMD /usr/sbin/nginx -g 'daemon off;'