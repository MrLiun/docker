FROM idoall/supervisor
MAINTAINER lion <lion.net@163.com>



# -----------------------------------------------------------------------------
# 安装 expect 工具
# -----------------------------------------------------------------------------
RUN yum makecache \
    && yum install -y tcl expect \
    \
# -----------------------------------------------------------------------------
# 安装java8环境
# -----------------------------------------------------------------------------
    && cd /home/work/_src \
    && wget --no-check-certificate --no-cookies --header "Cookie: oraclelicense=accept-securebackup-cookie" http://download.oracle.com/otn-pub/java/jdk/8u111-b14/jdk-8u111-linux-x64.tar.gz \
    && mkdir -p /usr/lib/jvm \
    && tar zxvf jdk-8u111-linux-x64.tar.gz -C /usr/lib/jvm \
    \
# -----------------------------------------------------------------------------
#   配置root用户的环境变量
# -----------------------------------------------------------------------------
    && echo "export JAVA_HOME=/usr/lib/jvm/jdk1.8.0_111" >>  ~/.bashrc \        
    && echo "export JRE_HOME=\$JAVA_HOME/jre" >>  ~/.bashrc \
    && echo "export CLASSPATH=.:\$JAVA_HOME/lib:\$JRE_HOME/lib" >>  ~/.bashrc \
    && echo "PATH=\$JAVA_HOME/bin:\$PATH" >>  ~/.bashrc \
    \
# -----------------------------------------------------------------------------
#   配置ssh登录后的环境变量
# -----------------------------------------------------------------------------
    && echo "export JAVA_HOME=/usr/lib/jvm/jdk1.8.0_111" >>  /etc/profile \
    && echo "export JRE_HOME=\$JAVA_HOME/jre" >>  /etc/profile \
    && echo "export CLASSPATH=.:\$JAVA_HOME/lib:\$JRE_HOME/lib" >>  /etc/profile \
    && echo "PATH=\$JAVA_HOME/bin:\$PATH" >>  /etc/profile


# -----------------------------------------------------------------------------
# 加入破解文件
# -----------------------------------------------------------------------------
ADD jira7.2_hack.zip /home/work/_src

# -----------------------------------------------------------------------------
# 加入安装文件
# -----------------------------------------------------------------------------
ADD install.sh /home/work/_script

# -----------------------------------------------------------------------------
# 安装jira
# -----------------------------------------------------------------------------
RUN cd /home/work/_src \
    && wget https://downloads.atlassian.com/software/jira/downloads/atlassian-jira-software-7.2.7-x64.bin \
    && chmod 755 atlassian-jira-software-7.2.7-x64.bin \
    && chown work:work -R /home/work \
    && expect /home/work/_script/install.sh \
    && unzip jira7.2_hack.zip \
    && cp /opt/atlassian/jira/atlassian-jira/WEB-INF/lib/atlassian-extras-3.1.2.jar /opt/atlassian/jira/atlassian-jira/WEB-INF/lib/atlassian-extras-3.1.2.jar.bak \
    && cp /home/work/_src/jira7.2/atlassian-extras-3.1.2.jar /opt/atlassian/jira/atlassian-jira/WEB-INF/lib \
    && cp /home/work/_src/jira7.2/mysql-connector-java-5.1.39-bin.jar /opt/atlassian/jira/atlassian-jira/WEB-INF/lib \
    && service jira stop \
    && service jira start \
    \
# -----------------------------------------------------------------------------
# 删除多余文件
# -----------------------------------------------------------------------------
    && yum clean all \
    && rm -rf /home/work/_src/*

# -----------------------------------------------------------------------------
# 通过 supervisor 启动 jira
# -----------------------------------------------------------------------------
ADD _app/supervisord/conf.d/jira.ini /home/work/_app/supervisord/conf.d/jira.ini


# -----------------------------------------------------------------------------
# 映射端口
# -----------------------------------------------------------------------------
EXPOSE 8080 8085 8443 8090