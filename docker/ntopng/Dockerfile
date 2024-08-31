FROM debian:bullseye

RUN apt-get update && apt-get install -y software-properties-common wget curl 

RUN wget https://packages.ntop.org/RaspberryPI/apt-ntop.deb 

RUN apt-get install -y gnupg

RUN dpkg -i apt-ntop.deb \
    && apt-get update 

RUN apt-get install -y ntopng nprobe

RUN apt-get upgrade -y

RUN mkdir /ntopngcustom \
    && chmod 777 /ntopngcustom

#CMD [ "ntopng --community -i wlan0 -w localhost:3000 -r localhost:6379 -d /ntopngcustom" ]