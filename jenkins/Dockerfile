FROM jenkins/jenkins:lts

USER root
RUN mkdir /home/jenkins && chown jenkins:jenkins /home/jenkins && usermod -d /home/jenkins jenkins
COPY id_rsa /home/jenkins/.ssh/id_rsa
RUN chmod 700 /home/jenkins/.ssh && chmod 600 /home/jenkins/.ssh/id_rsa \
    && chown -R jenkins:jenkins /home/jenkins/.ssh
# install jenkins plugin
COPY ./plugins.txt /usr/share/jenkins/ref

RUN apt-get update -y
RUN apt-get install -y apt-transport-https ca-certificates curl gnupg2 software-properties-common

# install docker, docker-compose
ENV DOCKER_VERSION 18.09.1
RUN curl -fL -o docker.tgz "https://download.docker.com/linux/static/test/x86_64/docker-$DOCKER_VERSION.tgz" && \
    tar --strip-component=1 -xvaf docker.tgz -C /usr/bin

RUN curl -o /usr/local/bin/docker-compose -L https://github.com/docker/compose/releases/download/1.19.1/docker-dompose-`uname -s`-`uname -m` && chmod +x /usr/local/bin/docker-compose
RUN chmod +x /usr/local/bin/docker-compose

# add docker group
ARG DOCKER_GROUP_ID
RUN groupadd -g ${DOCKER_GROUP_ID} docker && usermod -aG ${DOCKER_GROUP_ID} jenkins

USER jenkins