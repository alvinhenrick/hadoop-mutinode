FROM alvinhenrick/hadoop-base

MAINTAINER Alvin Henrick

RUN apt-get install -y iputils-ping daemontools runit

ENV HADOOP_INSTALL /usr/local/hadoop

RUN mkdir -p /home/hduser/hdfs/namenode  
RUN mkdir -p /home/hduser/hdfs/datanode 
 
RUN mkdir $HADOOP_INSTALL/logs

RUN mkdir -p /etc/service/serf
RUN mkdir -p /etc/service/sshd

ADD config/service /etc/service 

RUN chmod +x /etc/service/serf/run 
RUN chmod +x /etc/service/sshd/run 


ADD config/hdfs-site.xml $HADOOP_INSTALL/etc/hadoop/hdfs-site.xml 
ADD config/core-site.xml $HADOOP_INSTALL/etc/hadoop/core-site.xml 
ADD config/mapred-site.xml $HADOOP_INSTALL/etc/hadoop/mapred-site.xml 
ADD config/yarn-site.xml $HADOOP_INSTALL/etc/hadoop/yarn-site.xml 
ADD config/slaves $HADOOP_INSTALL/etc/hadoop/slaves 

RUN chown -R hduser:hadoop /home/hduser/hdfs/namenode
RUN chown -R hduser:hadoop /home/hduser/hdfs/datanode
RUN chown -R hduser:hadoop $HADOOP_INSTALL/logs
RUN chmod 1777 /tmp

# Format namenode
RUN su hduser -c "/usr/local/hadoop/bin/hdfs namenode -format"

ADD config/start-hadoop.sh $HADOOP_INSTALL/bin/start-hadoop.sh
RUN chmod 755 $HADOOP_INSTALL/bin/start-hadoop.sh

# SSH and SERF ports
EXPOSE 22 7373 7946 

# HDFS ports
EXPOSE 9000 50010 50020 50070 50075 50090 50475

# YARN ports
EXPOSE 8030 8031 8032 8033 8040 8042 8060 8088 50060

#ENTRYPOINT ["/bin/bash", "/usr/local/hadoop/bin/start-hadoop.sh"]
CMD ["/bin/bash"]