# 使用 Rocky Linux 8 作為基礎映像
FROM rockylinux/rockylinux:8

# 啟用 PowerTools 並安裝必要的依賴項
RUN dnf install -y dnf-plugins-core && \
    dnf config-manager --set-enabled powertools && \
    dnf clean all && \
    dnf -y update && \
    dnf -y install \
      java-1.8.0-openjdk-devel \
      wget \
      net-tools \
      python3 \
      sudo \
      vim

# 下載並安裝 PostgreSQL JDBC 驅動（Ambari Server 需要與 PostgreSQL 通信）
RUN wget https://jdbc.postgresql.org/download/postgresql-42.7.4.jar -O /usr/share/java/postgresql-jdbc.jar

# 手動下載並安裝 Ambari Server RPM 包
RUN wget https://archive.apache.org/dist/ambari/ambari-2.7.5/ambari-server-2.7.5-0.el7.x86_64.rpm -O /tmp/ambari-server.rpm && \
    dnf install -y /tmp/ambari-server.rpm && \
    rm -f /tmp/ambari-server.rpm

# 設置 Ambari Server 環境
ENV JAVA_HOME=/usr/lib/jvm/java-1.8.0-openjdk
ENV PATH=$PATH:/usr/sbin

# 暴露必要的端口
EXPOSE 8080 8440 8441

# 設置工作目錄
WORKDIR /root

# 默認命令（docker-compose.yml 會覆蓋此命令）
CMD ["ambari-server", "start"]