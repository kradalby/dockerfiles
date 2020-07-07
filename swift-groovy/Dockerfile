FROM swift:bionic

RUN sed -i 's/bionic/groovy/g' /etc/apt/sources.list
RUN apt-get update \
        && apt-get upgrade -y \
        && apt-get install -y libexif-dev libiptcdata0-dev libgd-dev
