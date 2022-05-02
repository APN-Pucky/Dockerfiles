# name the portage image
FROM ubuntu:latest
MAINTAINER APN
ENV TZ=Europe/Berlin
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone
RUN apt-get update -y -qq && apt-get install -y -qq libboost-all-dev libgsl-dev lcov wget build-essential sudo git rsync pandoc curl python3 python3-pip python3-venv cmake && apt-get clean
RUN update-alternatives --install /usr/bin/python python /usr/bin/python3 1