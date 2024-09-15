FROM ubuntu:18.04

LABEL maintainer="alexozavr2005@gmail.com"

WORKDIR = /home/ubuntu/Lab2/good_docker

RUN apt-get update && apt-get install bash

COPY . .

ENTRYPOINT ["bash", "MyScript.bash"]

VOLUME /my-volume
