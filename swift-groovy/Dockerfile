FROM swift:5.5-bionic

RUN sed -i 's/bionic/groovy/g' /etc/apt/sources.list
RUN apt-get update \
        && apt-get upgrade -y \
        && apt-get install -o APT::Immediate-Configure=false -y libexif-dev libiptcdata0-dev \
        && apt-get purge -o APT::Immediate-Configure=false -y ircd \
        && rm -rf /var/lib/apt/lists/*

