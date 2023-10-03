# --------------------------------------------------------------------
# Copyright (c) 2020, WSO2 Inc. (http://wso2.com) All Rights Reserved.
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
# http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.
# -----------------------------------------------------------------------

FROM alpine:3.16.1

LABEL maintainer="WSO2 Docker Maintainers <dev@wso2.org>" \
      com.wso2.docker.source="https://github.com/wso2/docker-mg/releases/tag/v3.2.8.1"

ENV LANG='en_US.UTF-8' LANGUAGE='en_US:en' LC_ALL='en_US.UTF-8'

# Microgateway runtime distribution filename.
ARG USER=ballerina
ARG USER_ID=802
ARG USER_GROUP=ballerina
ARG USER_GROUP_ID=802
ARG USER_HOME=/home/${USER}
ARG JRE_BINARY_URL='https://github.com/adoptium/temurin8-binaries/releases/download/jdk8u382-b05/OpenJDK8U-jre_x64_linux_hotspot_8u382b05.tar.gz'
ARG INTERNAL_JRE=jdk8u332-b09-jre
ARG JRE_HOME=/opt/java/openjdk

# build arguments for WSO2 product installation
ARG MGW_SERVER_NAME=wso2am-micro-gw-linux
ARG MGW_SERVER_VERSION=3.2.8
ARG MGW_SERVER=${MGW_SERVER_NAME}-${MGW_SERVER_VERSION}
ARG MGW_RUNTIME_HOME=${USER_HOME}/wso2
ARG MGW_SERVER_DIST_URL=https://github.com/wso2/product-microgateway/releases/download/v${MGW_SERVER_VERSION}/${MGW_SERVER}.zip

# build argument for MOTD
ARG MOTD="\n\
 Welcome to WSO2 Docker Resources \n\
 --------------------------------- \n\
 This Docker container comprises of a WSO2 product, running with its latest GA release \n\
 which is under the Apache License, Version 2.0. \n\
 Read more about Apache License, Version 2.0 here @ http://www.apache.org/licenses/LICENSE-2.0.\n\n"
ENV ENV=${USER_HOME}"/.ashrc"

# GLIB based alpine image (alpine-glib) source is generously borrowed
# From https://github.com/Docker-Hub-frolvlad/docker-alpine-glibc
# Here we install GNU libc (aka glibc) and set C.UTF-8 locale as default.
RUN apk add --no-cache tzdata --virtual .build-deps curl binutils zstd \
    && GLIBC_VER="2.33-r0" \
    && ALPINE_GLIBC_REPO="https://github.com/sgerrand/alpine-pkg-glibc/releases/download" \
    && GCC_LIBS_URL="https://archive.archlinux.org/packages/g/gcc-libs/gcc-libs-10.1.0-2-x86_64.pkg.tar.zst" \
    && GCC_LIBS_SHA256="f80320a03ff73e82271064e4f684cd58d7dbdb07aa06a2c4eea8e0f3c507c45c" \
    && ZLIB_URL="https://archive.archlinux.org/packages/z/zlib/zlib-1%3A1.2.11-3-x86_64.pkg.tar.xz" \
    && ZLIB_SHA256=17aede0b9f8baa789c5aa3f358fbf8c68a5f1228c5e6cba1a5dd34102ef4d4e5 \
    && curl -LfsS https://alpine-pkgs.sgerrand.com/sgerrand.rsa.pub -o /etc/apk/keys/sgerrand.rsa.pub \
    && SGERRAND_RSA_SHA256="823b54589c93b02497f1ba4dc622eaef9c813e6b0f0ebbb2f771e32adf9f4ef2" \
    && echo "${SGERRAND_RSA_SHA256} */etc/apk/keys/sgerrand.rsa.pub" | sha256sum -c - \
    && curl -LfsS ${ALPINE_GLIBC_REPO}/${GLIBC_VER}/glibc-${GLIBC_VER}.apk > /tmp/glibc-${GLIBC_VER}.apk \
    && apk add --no-cache /tmp/glibc-${GLIBC_VER}.apk \
    && apk add unzip \
    && curl -LfsS ${ALPINE_GLIBC_REPO}/${GLIBC_VER}/glibc-bin-${GLIBC_VER}.apk > /tmp/glibc-bin-${GLIBC_VER}.apk \
    && apk add --no-cache /tmp/glibc-bin-${GLIBC_VER}.apk \
    && curl -Ls ${ALPINE_GLIBC_REPO}/${GLIBC_VER}/glibc-i18n-${GLIBC_VER}.apk > /tmp/glibc-i18n-${GLIBC_VER}.apk \
    && apk add --no-cache /tmp/glibc-i18n-${GLIBC_VER}.apk \
    && /usr/glibc-compat/bin/localedef --force --inputfile POSIX --charmap UTF-8 "$LANG" || true \
    && echo "export LANG=$LANG" > /etc/profile.d/locale.sh \
    && curl -LfsS ${GCC_LIBS_URL} -o /tmp/gcc-libs.tar.zst \
    && echo "${GCC_LIBS_SHA256} */tmp/gcc-libs.tar.zst" | sha256sum -c - \
    && mkdir /tmp/gcc \
    && zstd -d /tmp/gcc-libs.tar.zst --output-dir-flat /tmp \
    && tar -xf /tmp/gcc-libs.tar -C /tmp/gcc \
    && mv /tmp/gcc/usr/lib/libgcc* /tmp/gcc/usr/lib/libstdc++* /usr/glibc-compat/lib \
    && strip /usr/glibc-compat/lib/libgcc_s.so.* /usr/glibc-compat/lib/libstdc++.so* \
    && curl -LfsS ${ZLIB_URL} -o /tmp/libz.tar.xz \
    && echo "${ZLIB_SHA256} */tmp/libz.tar.xz" | sha256sum -c - \
    && mkdir /tmp/libz \
    && tar -xf /tmp/libz.tar.xz -C /tmp/libz \
    && mv /tmp/libz/usr/lib/libz.so* /usr/glibc-compat/lib \
    && apk del --purge .build-deps glibc-i18n \
    && rm -rf /tmp/*.apk /tmp/gcc /tmp/gcc-libs.tar* /tmp/libz /tmp/libz.tar.xz /var/cache/apk/*


# Prepare image with users, groups, packages and other configs.
# Add motd for both ash and bash
RUN \
    addgroup -S -g ${USER_GROUP_ID} ${USER_GROUP} \
    && adduser -S -u ${USER_ID} -h ${USER_HOME} -G ${USER_GROUP} ${USER} \
    && echo '[ ! -z "${TERM}" -a -r /etc/motd ] && printf "$(cat /etc/motd)"' >> ${ENV}; echo "${MOTD}" > /etc/motd \
    && echo '[ ! -z "${TERM}" -a -r /etc/motd ] && printf "$(cat /etc/motd)"' >> ${USER_HOME}"/.bashrc" \
    && apk add --update --no-cache bash libxml2-utils netcat-openbsd \
    && wget -O /root/${MGW_SERVER}.zip "${MGW_SERVER_DIST_URL}" \
    && wget -O /tmp/openjdk.tar.gz "${JRE_BINARY_URL}" \
    && unzip /root/${MGW_SERVER}.zip -d /home/${USER}/ >/dev/null 2>&1 \
    && mkdir -p /home/${USER}/conf \
    && mv /home/${USER}/${MGW_SERVER} ${MGW_RUNTIME_HOME} \
    && mv ${MGW_RUNTIME_HOME}/conf/micro-gw.conf ${USER_HOME}/conf/micro-gw.conf \
    && chown ${USER}:${USER_GROUP} -R ${USER_HOME}  \
    && rm /root/${MGW_SERVER}.zip > /dev/null 2>&1 \
    && rm -rf ${MGW_RUNTIME_HOME}/lib/${INTERNAL_JRE} \
    && rm -f "/root/.wget-hsts" \
    && mkdir -p ${JRE_HOME} \
    && cd ${JRE_HOME} \
    && tar -xf /tmp/openjdk.tar.gz --strip-components=1 \
    && rm -rf /tmp/openjdk.tar.gz \
    && rm -rf /var/cache/apk/*

ENV BALLERINA_HOME ${MGW_RUNTIME_HOME}/runtime
ENV GW_HOME ${MGW_RUNTIME_HOME}
ENV PATH $GW_HOME/bin:$PATH
ENV MGW_CONF_DIR=${USER_HOME}/conf
ENV JAVA_OPTS="${JAVA_OPTS}"
ENV JAVA_HOME=/opt/java/openjdk
ENV PATH="/opt/java/openjdk/bin:$PATH"

WORKDIR ${USER_HOME}

USER ${USER}

RUN \
    touch ${USER_HOME}/gc.log \
    && touch ${USER_HOME}/heapdump.hprof

# expose ports
EXPOSE 9090 9095

CMD gateway /home/exec/${project}.jar
