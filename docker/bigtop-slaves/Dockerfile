# Licensed to the Apache Software Foundation (ASF) under one or more
# contributor license agreements.  See the NOTICE file distributed with
# this work for additional information regarding copyright ownership.
# The ASF licenses this file to You under the Apache License, Version 2.0
# (the "License"); you may not use this file except in compliance with
# the License.  You may obtain a copy of the License at
#
#     http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.

FROM bigtop/puppet:3.3.0-centos-7

MAINTAINER dev@bigtop.apache.org

# 更新 yum 源并安装必要的软件包
RUN sed -e "s|^mirrorlist=|#mirrorlist=|g" \
    -e "s|^#baseurl=http://mirror.centos.org/centos/\$releasever|baseurl=https://mirrors.ustc.edu.cn/centos-vault/7.9.2009|g" \
    -e "s|^#baseurl=http://mirror.centos.org/\$contentdir/\$releasever|baseurl=https://mirrors.ustc.edu.cn/centos-vault/7.9.2009|g" \
    -i.bak \
    /etc/yum.repos.d/CentOS-*.repo && \
    yum -y install git java-1.8.0-openjdk-devel rpm-build

# 设置 JAVA_HOME 环境变量
ENV JAVA_HOME /usr/lib/jvm/java-1.8.0-openjdk

RUN wget https://archive.apache.org/dist/maven/maven-3/3.8.8/binaries/apache-maven-3.8.8-bin.tar.gz && \
    tar -zxvf apache-maven-3.8.8-bin.tar.gz -C /opt && \
    ln -s /opt/apache-maven-3.8.8 /opt/maven

# 设置 Maven 环境变量
ENV MAVEN_HOME /opt/maven
ENV PATH $MAVEN_HOME/bin:$PATH

# 克隆 Apache Bigtop 代码
RUN git clone https://github.com/apache/bigtop.git /bigtop

# 切换到 Bigtop 目录
WORKDIR /bigtop

# 编译 RPM 包
RUN ./gradlew rpm

# 设置容器启动命令
CMD ["/bin/bash"]
