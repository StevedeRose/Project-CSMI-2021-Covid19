# -*- mode: Dockerfile -*-

FROM ubuntu:focal

ARG CHANNEL
ENV CHANNEL=${CHANNEL:-latest}
RUN apt-get update \
    && apt-get install -y \
    vim \
    ssh \
    sudo \
    wget \
    software-properties-common ;\
    rm -rf /var/lib/apt/lists/*

RUN useradd --user-group --create-home --shell /bin/bash feelpp ;\
    echo "feelpp ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers


ENV DEBIAN_FRONTEND=noninteractive        
RUN echo "deb http://archive.ubuntu.com/ubuntu focal-proposed main restricted" |  tee -a /etc/apt/sources.list; \ 
    echo "deb http://archive.ubuntu.com/ubuntu focal-proposed universe" |  tee -a /etc/apt/sources.list; \
    echo "deb http://archive.ubuntu.com/ubuntu focal-proposed multiverse" |  tee -a /etc/apt/sources.list; \
    apt-get -qq update; \
    apt-get -y install tzdata dos2unix

RUN sh -c "wget -O - https://bintray.com/user/downloadSubjectPublicKey?username=bintray | apt-key add -" ;\
    echo "deb https://dl.bintray.com/feelpp/ubuntu focal ${CHANNEL}" | tee -a /etc/apt/sources.list;\
    apt-get update ;\
    apt-get install -y --no-install-recommends feelpp-tools rsync;\
    rm -rf /var/lib/apt/lists/* ;

RUN     echo "export FEELPP_REPOSITORY=/feel" >> ~feelpp/.bashrc;\
    echo "export OMPI_MCA_btl_vader_single_copy_mechanism=none" >> ~feelpp/.bashrc


COPY . /home/feelpp/
RUN ls -lrtR /home/feelpp

RUN dpkg -i /home/feelpp/*.deb 

USER feelpp

