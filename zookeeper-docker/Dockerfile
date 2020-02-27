FROM ubuntu:latest
MAINTAINER Wurstmeister 
RUN apt-get update; apt-get install -y unzip openjdk-8-jre-headless wget supervisor docker.io openssh-server
ENV JAVA_HOME /usr/lib/jvm/java-8-openjdk-amd64/
RUN echo 'root:wurstmeister' | chpasswd
RUN mkdir /var/run/sshd
RUN sed -i 's/PermitRootLogin without-password/PermitRootLogin yes/' /etc/ssh/sshd_config

EXPOSE 22

#Download Zookeeper
RUN wget  -q http://mirror.vorboss.net/apache/zookeeper/stable/apache-zookeeper-3.5.7.tar.gz
RUN mv apache-zookeeper-3.5.7.tar.gz  zookeeper-3.5.7.tar.gz
RUN wget -q https://www.apache.org/dist/zookeeper/KEYS
RUN wget -q https://downloads.apache.org/zookeeper/zookeeper-3.5.7/apache-zookeeper-3.5.7.tar.gz.asc
RUN wget -q https://downloads.apache.org/zookeeper/zookeeper-3.5.7/apache-zookeeper-3.5.7.tar.gz.sha512

#Verify download
#RUN sha512sum -c apache-zookeeper-3.5.7.tar.gz.sha512 && \
#gpg --import KEYS && \
#gpg --verify apache-zookeeper-3.5.7.tar.gz.asc

#Install

RUN tar -xzf zookeeper-3.5.7.tar.gz -C /opt
RUN ls /opt
RUN mv /opt/apache-zookeeper-3.5.7 /opt/zookeeper-3.5.7
#Configure
RUN mv /opt/zookeeper-3.5.7/conf/zoo_sample.cfg /opt/zookeeper-3.5.7/conf/zoo.cfg

ENV JAVA_HOME /usr/lib/jvm/java-8-openjdk-amd64
ENV ZK_HOME /opt/zookeeper-3.5.7
RUN sed  -i "s|/tmp/zookeeper|$ZK_HOME/data|g" $ZK_HOME/conf/zoo.cfg; mkdir $ZK_HOME/data

ADD start-zk.sh /usr/bin/start-zk.sh 
EXPOSE 2181 2888 3888

WORKDIR /opt/zookeeper-3.5.7
VOLUME ["/opt/zookeeper-3.5.7/conf", "/opt/zookeeper-3.5.7/data"]

CMD /usr/sbin/sshd && bash /usr/bin/start-zk.sh
