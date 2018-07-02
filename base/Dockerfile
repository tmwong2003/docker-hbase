FROM openjdk:8
MAINTAINER Ivan Ermilov <ivan.s.ermilov@gmail.com>

RUN apt-get update && DEBIAN_FRONTEND=noninteractive apt-get install -y --no-install-recommends net-tools curl netcat

ENV HBASE_MINOR_VERSION 1.2
ENV HBASE_VERSION $HBASE_MINOR_VERSION.6.1
ENV HBASE_URL http://www.apache.org/dist/hbase/$HBASE_VERSION/hbase-$HBASE_VERSION-bin.tar.gz
RUN set -x \
    && curl -fSL "$HBASE_URL" -o /tmp/hbase.tar.gz \
    && curl -fSL "$HBASE_URL.asc" -o /tmp/hbase.tar.gz.asc \
    && tar -xvf /tmp/hbase.tar.gz -C /opt/ \
    && rm /tmp/hbase.tar.gz*

RUN ln -s /opt/hbase-$HBASE_VERSION/conf /etc/hbase
RUN mkdir /opt/hbase-$HBASE_VERSION/logs

RUN mkdir /hadoop-data

ENV HBASE_PREFIX=/opt/hbase-$HBASE_VERSION
ENV HBASE_CONF_DIR=/etc/hbase

ENV PHOENIX_VERSION=4.14.0
ENV PHOENIX_BIN_PKG=apache-phoenix-$PHOENIX_VERSION-HBase-$HBASE_MINOR_VERSION-bin
ENV PHOENIX_URL http://www.apache.org/dist/phoenix/apache-phoenix-$PHOENIX_VERSION-HBase-$HBASE_MINOR_VERSION/bin/$PHOENIX_BIN_PKG.tar.gz
RUN set -x \
    && curl -fSL "$PHOENIX_URL" -o /tmp/phoenix.tar.gz \
    && tar -xvf /tmp/phoenix.tar.gz -C /tmp \
    && cp -p /tmp/$PHOENIX_BIN_PKG/phoenix-$PHOENIX_VERSION-HBase-$HBASE_MINOR_VERSION-client.jar $HBASE_PREFIX/lib \
    && cp -p /tmp/$PHOENIX_BIN_PKG/phoenix-$PHOENIX_VERSION-HBase-$HBASE_MINOR_VERSION-server.jar $HBASE_PREFIX/lib \
    && rm -r /tmp/$PHOENIX_BIN_PKG \
    && rm /tmp/phoenix.tar.gz

ENV USER=root
ENV PATH $HBASE_PREFIX/bin/:$PATH

ADD entrypoint.sh /entrypoint.sh
RUN chmod a+x /entrypoint.sh

ENTRYPOINT ["/entrypoint.sh"]
