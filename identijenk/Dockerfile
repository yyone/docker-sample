FROM jenkins/jenkins:lts-alpine

USER root

ENV JENKINS_REF /usr/share/jenkins/ref

# install jenkins plugins
COPY ./plugins.txt $JENKINS_REF
RUN /usr/local/bin/install-plugins.sh < $JENKINS_REF/plugins.txt

RUN apk add --no-cache --virtual make && \
    apk add --no-cache ca-certificates wget curl && update-ca-certificates && \
    apk add --no-cache openssl && \
    apk add --no-cache sudo

# install glibc
ENV ALPINE_GLIBC_BASE_URL "https://github.com/sgerrand/alpine-pkg-glibc/releases/download"
ENV ALPINE_GLIBC_PACKAGE_VERSION "2.29-r0"
ENV ALPINE_GLIBC_BASE_PACKAGE_FILENAME "glibc-$ALPINE_GLIBC_PACKAGE_VERSION.apk"
ENV ALPINE_GLIBC_BIN_PACKAGE_FILENAME "glibc-bin-$ALPINE_GLIBC_PACKAGE_VERSION.apk"
ENV ALPINE_GLIBC_I18N_PACKAGE_FILENAME="glibc-i18n-$ALPINE_GLIBC_PACKAGE_VERSION.apk"
RUN apk add --no-cache --virtual=.build-dependencies wget ca-certificates && \
    wget "https://alpine-pkgs.sgerrand.com/sgerrand.rsa.pub" -O "/etc/apk/keys/sgerrand.rsa.pub" && \
    wget \
        "$ALPINE_GLIBC_BASE_URL/$ALPINE_GLIBC_PACKAGE_VERSION/$ALPINE_GLIBC_BASE_PACKAGE_FILENAME" \
        "$ALPINE_GLIBC_BASE_URL/$ALPINE_GLIBC_PACKAGE_VERSION/$ALPINE_GLIBC_BIN_PACKAGE_FILENAME" \
        "$ALPINE_GLIBC_BASE_URL/$ALPINE_GLIBC_PACKAGE_VERSION/$ALPINE_GLIBC_I18N_PACKAGE_FILENAME" && \
    apk add --no-cache \
        "$ALPINE_GLIBC_BASE_PACKAGE_FILENAME" \
        "$ALPINE_GLIBC_BIN_PACKAGE_FILENAME" \
        "$ALPINE_GLIBC_I18N_PACKAGE_FILENAME" && \
    \
    rm "/etc/apk/keys/sgerrand.rsa.pub" && \
    /usr/glibc-compat/bin/localedef --force --inputfile POSIX --charmap UTF-8 C.UTF-8 || true && \
    echo "export LANG=C.UTF-8" > /etc/profile.d/locale.sh && \
    \
    apk del glibc-i18n && \
    \
    rm "/root/.wget-hsts" && \
    apk del .build-dependencies && \
    rm \
        "$ALPINE_GLIBC_BASE_PACKAGE_FILENAME" \
        "$ALPINE_GLIBC_BIN_PACKAGE_FILENAME" \
        "$ALPINE_GLIBC_I18N_PACKAGE_FILENAME"

#RUN wget -q -O /etc/apk/keys/sgerrand.rsa.pub https://alpine-pkgs.sgerrand.com/sgerrand.rsa.pub
#RUN wget https://github.com/sgerrand/alpine-pkg-glibc/releases/download/2.29-r0/glibc-2.29-r0.apk
#RUN apk add glibc-2.29-r0.apk

# install docker
RUN apk add docker

# install docker-compose
RUN curl -o /usr/local/bin/docker-compose -L https://github.com/docker/compose/releases/download/1.24.1/docker-compose-`uname -s`-`uname -m` && chmod +x /usr/local/bin/docker-compose
RUN adduser jenkins docker

VOLUME /var/jenkins

