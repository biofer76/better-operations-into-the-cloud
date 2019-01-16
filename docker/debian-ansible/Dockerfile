FROM debian:stretch

ENV DEBIAN_FRONTEND noninteractive

RUN apt-get update
RUN apt-get install -y \
  gnupg \
  apt-transport-https \
  git \
  curl \
  openssl

RUN echo "deb http://ppa.launchpad.net/ansible/ansible/ubuntu trusty main" >> /etc/apt/sources.list
RUN apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 93C4A3FD7BB9C367

RUN apt-get update
RUN apt-get install ansible -y

