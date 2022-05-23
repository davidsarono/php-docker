FROM nginx:stable

RUN apt-get update && \
    apt-get -y install bash tzdata curl redis

RUN echo Asia/Jakarta > /etc/timezone && \
    rm /etc/localtime && \
    ln -snf /usr/share/zoneinfo/Asia/Jakarta /etc/localtime && \
    dpkg-reconfigure -f noninteractive tzdata

EXPOSE 80 443
