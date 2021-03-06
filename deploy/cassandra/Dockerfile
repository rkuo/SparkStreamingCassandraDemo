FROM centos:centos7
RUN yum clean all
RUN yum install -y tar yum-utils wget
RUN yum-config-manager --save --setopt=fedora.skip_if_unavailable=true     
RUN yum update -y

# Java
RUN yum install -y java-1.7.0-openjdk-devel.x86_64

# Spark  in /opt. default to hadoop 2.4 version.
ADD http://www.apache.org/dyn/closer.cgi?path=/cassandra/2.0.11/apache-cassandra-2.0.11-bin.tar.gz /opt
# ADD from HTTP doesn't unzip for us (https://github.com/docker/docker/issues/6353).  So untar here.
RUN tar -xzf /opt/apache-cassandra-2.0.11-bin.tar.gz -C /opt
RUN echo "CASSANDRA_HOME=/opt/apache-cassandra-2.0.11-bin/" >> /etc/environment

#### Now install SSH so we can layer in the spark components.
RUN yum -y install openssh-server openssh-clients sudo
RUN sed -i.bak s/UsePAM\ yes/UsePAM\ no/ /etc/ssh/sshd_config
RUN ssh-keygen -q -N "" -t dsa -f /etc/ssh/ssh_host_dsa_key
RUN ssh-keygen -q -N "" -t rsa -f /etc/ssh/ssh_host_rsa_key
# requiretty off
RUN sed -i.bak 's/requiretty/!requiretty/' /etc/sudoers
# setup vagrant account
RUN mkdir /root/.ssh/
RUN chmod 0755 /root/.ssh
RUN wget http://github.com/mitchellh/vagrant/raw/master/keys/vagrant.pub --no-check-certificate -O /root/.ssh/authorized_keys
RUN chmod 0644 /root/.ssh/authorized_keys
CMD /usr/sbin/sshd -D
