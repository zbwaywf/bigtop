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

COPY ../bigtop_toolchain /etc/puppet/modules/bigtop_toolchain
RUN sed -e "s|^mirrorlist=|#mirrorlist=|g" \
    -e "s|^#baseurl=http://mirror.centos.org/centos/\$releasever|baseurl=https://mirrors.ustc.edu.cn/centos-vault/7.9.2009|g" \
    -e "s|^#baseurl=http://mirror.centos.org/\$contentdir/\$releasever|baseurl=https://mirrors.ustc.edu.cn/centos-vault/7.9.2009|g" \
    -i.bak \
    /etc/yum.repos.d/CentOS-*.repo
    
RUN if [ -f ~/.bash_profile ]; then . ~/.bash_profile; fi && \
    yum clean all && yum updateinfo && \
    puppet apply -e "include bigtop_toolchain::installer" || if [ $? -ne 2 ]; then exit 1; fi
COPY . /tmp/bigtop
RUN cd /tmp/bigtop && ./gradlew && cd && rm -rf /tmp/*
