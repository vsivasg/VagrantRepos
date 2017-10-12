FROM ubuntu:14.04.1
MAINTAINER Venkata Siva "vsiva.sg@gmail.com"
RUN apt-get update
RUN apt-get install -y software-properties-common
RUN apt-add-repository ppa:ansible/ansible
RUN apt-get update
RUN apt-get install -y ansible
RUN apt-get install -y rsync
RUN apt-get install -y python-pip
# Install Java JDK
RUN apt-get install -y openjdk-7-jdk
# add our user and group first to make sure their IDs get assigned consistently, regardless of whatever dependencies get added
RUN groupadd -r jboss && useradd -r -g jboss -m -d /home/jboss jboss
# Install EAP 6.4.0.GA

RUN apt-get install -y zip unzip

USER jboss
ENV INSTALLDIR /home/jboss/EAP-6.4.0
ENV HOME /home/jboss

RUN mkdir $INSTALLDIR && \
   mkdir $INSTALLDIR/distribution && \
   mkdir $INSTALLDIR/resources


USER root
ADD distribution $INSTALLDIR/distribution
ADD distribution-patches $INSTALLDIR/distribution-patches
RUN chown -R jboss:jboss /home/jboss
RUN find /home/jboss -type d -execdir chmod 770 {} \;
RUN find /home/jboss -type f -execdir chmod 660 {} \;

USER jboss
RUN unzip $INSTALLDIR/distribution/jboss-eap-6.4.0.zip  -d $INSTALLDIR



############################################
# Create start script to run EAP instance
############################################
USER root

RUN apt-get install -y curl
RUN curl -o /usr/local/bin/gosu -SL "https://github.com/tianon/gosu/releases/download/1.3/gosu-amd64" \
    	&& chmod +x /usr/local/bin/gosu

############################################
# Remove install artifacts
############################################
RUN rm -rf $INSTALLDIR/distribution
RUN rm -rf $INSTALLDIR/distribution-patches
RUN rm -rf $INSTALLDIR/resources

############################################
# Add customization sub-directories (for entrypoint)
############################################
ADD docker-entrypoint-initdb.d  /docker-entrypoint-initdb.d
RUN chown -R jboss:jboss        /docker-entrypoint-initdb.d
RUN find /docker-entrypoint-initdb.d -type d -execdir chmod 770 {} \;
RUN find /docker-entrypoint-initdb.d -type f -execdir chmod 660 {} \;

ADD modules  $INSTALLDIR/modules
RUN chown -R jboss:jboss $INSTALLDIR/modules
RUN find $INSTALLDIR/modules -type d -execdir chmod 770 {} \;
RUN find $INSTALLDIR/modules -type f -execdir chmod 660 {} \;

############################################
# Expose paths and start JBoss
############################################

EXPOSE 22 5455 9999 8009 8080 8443 3528 3529 7500 45700 7600 57600 5445 23364 5432 8090 4447 4712 4713 9990 8787

RUN mkdir /etc/jboss-as
RUN mkdir /var/log/jboss/
RUN chown jboss:jboss /var/log/jboss/

COPY docker-entrypoint.sh /
RUN chmod 700 /docker-entrypoint.sh

############################################
# Start JBoss in stand-alone mode
############################################

ENTRYPOINT ["/docker-entrypoint.sh"]

CMD ["start-jboss"]