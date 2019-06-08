FROM postgres:11.3
ENV SCWS_VERSION 1.2.3
RUN mv /etc/apt/sources.list /etc/apt/sources.list.bak && \
        echo "deb http://mirrors.163.com/debian/ stretch main non-free contrib" >/etc/apt/sources.list && \
        echo "deb http://mirrors.163.com/debian/ stretch-updates main non-free contrib" >>/etc/apt/sources.list && \
        echo "deb http://mirrors.163.com/debian/ stretch-backports main non-free contrib" >>/etc/apt/sources.list && \
        echo "deb-src http://mirrors.163.com/debian/ stretch main non-free contrib" >>/etc/apt/sources.list && \
        echo "deb-src http://mirrors.163.com/debian/ stretch-updates main non-free contrib" >>/etc/apt/sources.list && \
        echo "deb-src http://mirrors.163.com/debian/ stretch-backports main non-free contrib" >>/etc/apt/sources.list && \
        echo "deb http://mirrors.163.com/debian-security/ stretch/updates main non-free contrib" >>/etc/apt/sources.list && \
        echo "deb-src http://mirrors.163.com/debian-security/ stretch/updates main non-free contrib" >>/etc/apt/sources.list


RUN apt-get update && apt-get install -y --no-install-recommends ca-certificates \
        gcc g++ libc6-dev systemtap-sdt-dev cmake make git wget unzip postgresql-server-dev-11 libpq-dev && rm -rf /var/lib/apt/lists/* && \
        mkdir -p /usr/lib/scws/rum && \
        wget -O /usr/lib/scws/scws-${SCWS_VERSION}.tar.bz2 "http://www.xunsearch.com/scws/down/scws-${SCWS_VERSION}.tar.bz2" && \
        wget -O /usr/lib/scws/zhparser.zip "https://github.com/amutu/zhparser/archive/master.zip" && \
        git clone https://github.com/postgrespro/rum /usr/lib/scws/rum && \
        git clone https://github.com/jaiminpan/pg_jieba /usr/lib/scws/pg_jieba && \
        chown -R postgres.postgres /usr/lib/scws/rum/ && \
        tar xjf /usr/lib/scws/scws-${SCWS_VERSION}.tar.bz2 -C /usr/lib/scws/ && \
        unzip /usr/lib/scws/zhparser.zip -d /usr/lib/scws/ && \
        rm -rf /usr/lib/scws/scws-${SCWS_VERSION}.tar.gz /usr/lib/scws/zhparser.zip && \
        cd /usr/lib/scws/scws-1.2.3 && \
        ./configure && \
        make install && \
        cd /usr/lib/scws/zhparser-master && \
        SCWS_HOME=/usr/local make && make install 

RUN     /sbin/ldconfig -v && \ 
        chown -R postgres.postgres /usr/lib/postgresql && \
        chown -R postgres.postgres /usr/share/postgresql && \
        chown -R postgres.postgres /usr/include/postgresql && \
        cd /usr/lib/scws/pg_jieba && \
        git submodule update --init --recursive && \
        mkdir /usr/lib/scws/pg_jieba/build && \
        cd /usr/lib/scws/pg_jieba/build &&\ 
        cmake -DPostgreSQL_TYPE_INCLUDE_DIR=/usr/include/postgresql/11/server .. && \
        make && make install  && \
        apt-get purge -y --auto-remove ca-certificates cmake wget unzip && \
        rm -rf /usr/lib/scws/scws* /usr/lib/scws/zhparser* /usr/lib/scws/pg_jieba
