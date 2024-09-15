FROM ubuntu:latest

RUN apt-get update && apt-get install bash

ADD https://raw.githubusercontent.com/Darrifus/itmo-cloud-labs/main/DevOps%20%D0%9B%D0%B0%D0%B1%D0%B0%202/MyScript.bash .

COPY . ./home/ubuntu/Lab2

CMD ["bash", "MyScript.bash"]
