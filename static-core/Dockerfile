FROM ubuntu:16.04
#MAINTAINER nick.humrich@canopytax.com

# Set the NODEJS_VERSION below to change the node version
ENV NODEJS_VERSION=8.2.1
ENV DEBIAN_FRONTEND=noninteractive


COPY woff-code-latest.zip /tmp/
# Make sure python, awscli, fontcustom, and git are all installed.
RUN apt -y update && \
    apt install -y python python-pip python-dev wget unzip git build-essential zlib1g-dev ruby-full rubygems fontforge && \
    pip install awscli

RUN unzip /tmp/woff-code-latest.zip -d sfnt2woff && cd sfnt2woff && make && mv sfnt2woff /usr/local/bin/ && \
    gem install fontcustom && \
    rm -rf /var/lib/apt/lists/* && \
    mkdir -p /www

# Now install node/npm.
RUN apt update && apt install bzip2 curl build-essential libssl-dev libfontconfig1 -y && \
    curl -SLO "https://nodejs.org/download/release/v$NODEJS_VERSION/node-v$NODEJS_VERSION-linux-x64.tar.gz" && \
    tar -xzf "node-v$NODEJS_VERSION-linux-x64.tar.gz" -C /usr/local --strip-components=1 && \
    npm install -g npm && \
    npm set progress=false \
    apt remove --purge -y libssl-dev && \
    apt autoremove -y && \
    rm -rf /var/lib/apt/lists/*

# install yarn (yarnpkg.com)
RUN apt-key adv --keyserver pgp.mit.edu --recv D101F7899D41F3C3 && \
    echo "deb http://dl.yarnpkg.com/debian/ stable main" | tee /etc/apt/sources.list.d/yarn.list && \
    apt update && apt install -y yarn && \
rm -rf /var/lib/apt/lists/*
