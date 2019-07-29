FROM jenkins/jenkins:lts-alpine

USER root

ENV JENKINS_REF /usr/share/jenkins/ref

# install jenkins plugins
COPY ./plugins.txt $JENKINS_REF
RUN /usr/local/bin/install-plugins.sh < $JENKINS_REF/plugins.txt

RUN apk add --no-cache --virtual make && \
    apk add --no-cache ca-certificates wget && update-ca-certificates && \
    apk add --no-cache openssl

VOLUME /var/jenkins

