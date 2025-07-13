FROM alpine:latest AS builder

WORKDIR /tmp

COPY flink-*.tgz .
RUN tar -zxvf flink-*.tgz -C /

COPY hadoop-*.tar.gz .
RUN tar -zxvf hadoop-*.tar.gz -C /

COPY jdk-* .
RUN tar -zxvf jdk-*.tar.gz -C /

FROM ubuntu:latest

RUN mkdir -p /home/flink

WORKDIR /home/flink

COPY --from=builder flink-* /home/flink/flink-current
COPY --from=builder /hadoop-* hadoop-current
COPY --from=builder /jdk-* /opt/java
COPY ./*.jar /home/flink/flink-current/lib

RUN ln -snf /home/flink/hadoop-current/share/hadoop/mapreduce/hadoop-mapreduce-client-core-*.jar \
	/home/flink/flink-current/lib

ENV PATH="/opt/java/bin:/home/flink/hadoop-current/bin:/home/flink/flink-current/bin:${PATH}"
ENV JAVA_HOME='/opt/java'
ENV HADOOP_CLASSPATH="/home/flink/hadoop-current/etc/hadoop:\
/home/flink/hadoop-current/share/hadoop/common/lib/*:\
/home/flink/hadoop-current/share/hadoop/common/*:\
/home/flink/hadoop-current/share/hadoop/hdfs:\
/home/flink/hadoop-current/share/hadoop/hdfs/lib/*:\
/home/flink/hadoop-current/share/hadoop/hdfs/*:\
/home/flink/hadoop-current/share/hadoop/mapreduce/*:\
/home/flink/hadoop-current/share/hadoop/yarn:\
/home/flink/hadoop-current/share/hadoop/yarn/lib/*:\
/home/flink/hadoop-current/share/hadoop/yarn/*"

CMD ["/usr/bin/bash"]
