FROM harisekhon/debian-java

ARG HBASE_VERSION=2.4.11

LABEL org.opencontainers.image.description="HBase" \
      org.opencontainers.image.version="$HBASE_VERSION" \
      org.opencontainers.image.authors="Jemi Chow (https://github.com/zsword)" \
      org.opencontainers.image.url="https://ghcr.io/zsword/hbase" \
      org.opencontainers.image.documentation="https://hub.docker.com/r/jemichow/hbase" \
      org.opencontainers.image.source="https://github.com/zsword/dockerApps"

ENV PATH $PATH:/hbase/bin

ENV JAVA_HOME=/usr
#RUN sed -i 's/deb.debian.org/mirrors.163.com/g' /etc/apt/sources.list
RUN sed -i 's/deb.debian.org/mirrors.ustc.edu.cn/g' /etc/apt/sources.list
RUN sed -i 's/security.debian.org/mirrors.ustc.edu.cn/g' /etc/apt/sources.list
#RUN sed -i 's/deb.debian.org/mirrors.aliyun.com/g' /etc/apt/sources.list
#RUN sed -i 's/security.debian.org/mirrors.aliyun.com/g' /etc/apt/sources.list

RUN apt-get update

WORKDIR /

COPY hbase-$HBASE_VERSION-bin.tar.gz .

# hadolint ignore=SC2039
#RUN apt-get install wget tar -y
RUN mkdir "hbase-$HBASE_VERSION" && \
    tar zxf "hbase-$HBASE_VERSION-bin.tar.gz" -C "hbase-$HBASE_VERSION" --strip 1 && \
    test -d "hbase-$HBASE_VERSION" && \
    ln -sv "hbase-$HBASE_VERSION" hbase && \
    rm -fv "hbase-$HBASE_VERSION-bin.tar.gz" && \
    rm -rf hbase/docs hbase/src && \
    mkdir /hbase-data

# Needed for HBase 2.0+ hbase-shell
# asciidoctor solves 'NotImplementedError: fstat unimplemented unsupported or native support failed to load'
RUN apt-get install jruby asciidoctor -y --allow-unauthenticated && \
    echo exit
RUN bash -c ' hbase shell '
    #set -euxo pipefail && \
    # jruby-maven jruby-minitest jruby-rdoc jruby-rake jruby-testunit

VOLUME /hbase-data

COPY entrypoint.sh /
COPY conf/hbase-site.xml /hbase/conf/
COPY profile.d/java.sh /etc/profile.d/

# Stargate  8080  / 8085
# Thrift    9090  / 9095
# HMaster   16000 / 16010
# RS        16201 / 16301
EXPOSE 2181 8080 8085 9090 9095 16000 16010 16020 16030

ENTRYPOINT ["/entrypoint.sh"]
