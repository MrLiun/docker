FROM idoall/supervisor
MAINTAINER lion <lion.net@163.com>


Add tengine-2.2.0.zip /home/work/_src
ADD pcre-8.35.tar.gz /home/work/_src
ADD zlib-1.2.8.tar.gz /home/work/_src
ADD openssl-1.0.1g.tar.gz /home/work/_src


# -----------------------------------------------------------------------------
# 安装tengine2.2.0基于nginx1.8
# -----------------------------------------------------------------------------
RUN mkdir -p /home/work/_script /home/work/_app/nginx \
# -----------------------------------------------------------------------------
#   安装pcre8.35
# -----------------------------------------------------------------------------
    && cd /home/work/_src/pcre-8.35 \
    && ./configure --enable-utf8 \
    && make \
    && make install \
    \
# -----------------------------------------------------------------------------
#   安装tengine
# -----------------------------------------------------------------------------
    && cd /home/work/_src/ \
    && unzip tengine-2.2.0.zip \
    && cd tengine-tengine-2.2.0 \
    && ./configure --prefix=/home/work/_app/nginx --with-pcre=/home/work/_src/pcre-8.35 --with-zlib=/home/work/_src/zlib-1.2.8 --with-openssl=/home/work/_src/openssl-1.0.1g --with-http_gzip_static_module --with-http_realip_module --with-http_stub_status_module --with-http_concat_module --with-http_ssl_module \
    && make \
    && make install \
    \
# -----------------------------------------------------------------------------
#   设置环境变量
# -----------------------------------------------------------------------------
    && echo "export PATH=\$PATH:/home/work/_app/nginx/sbin" >> /etc/profile \
    && echo "export PATH=\$PATH:/home/work/_app/nginx/sbin" >> ~/.bashrc \
    && sed -i "s/#user  nobody;/user  work;/" /home/work/_app/nginx/conf/nginx.conf \
    \
# -----------------------------------------------------------------------------
# 删除多余文件
# -----------------------------------------------------------------------------
    && yum clean all \
    && rm -rf /home/work/_src/*

# -----------------------------------------------------------------------------
# 通过 supervisor 启动 Tengine/2.2.0 (Nginx/1.8.1)
# -----------------------------------------------------------------------------
ADD _app/supervisord/conf.d/nginx.ini /home/work/_app/supervisord/conf.d/nginx.ini


# -----------------------------------------------------------------------------
# 映射端口
# -----------------------------------------------------------------------------
EXPOSE 80

