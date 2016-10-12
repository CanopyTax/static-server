FROM debian:wheezy
#MAINTAINER nick.humrich@canopytax.com

# Set the NODEJS_VERSION below to change the node version
ENV NODEJS_VERSION=5.7.1

# Make sure python, awscli, and git are all installed.
RUN apt-get update && \
    apt-get install python python-pip git -y && \
    pip install awscli && \
    apt-get remove --purge -y python-pip && \
    apt-get autoremove -y && \
    rm -rf /var/lib/apt/lists/* && \
    mkdir -p /www

# Now install node/npm.
RUN apt-get update && apt-get install bzip2 curl build-essential libssl-dev libfontconfig1 -y && \
    curl -SLO "https://nodejs.org/download/release/v$NODEJS_VERSION/node-v$NODEJS_VERSION-linux-x64.tar.gz" && \
    tar -xzf "node-v$NODEJS_VERSION-linux-x64.tar.gz" -C /usr/local --strip-components=1 && \
    npm install -g npm && \
    npm set progress=false \
    apt-get remove --purge -y libssl-dev && \
    apt-get autoremove -y && \
    rm -rf /var/lib/apt/lists/*

# install yarn (yarnpkg.com)
RUN sudo apt-key adv --keyserver pgp.mit.edu --recv D101F7899D41F3C3 && \
    echo "deb http://dl.yarnpkg.com/debian/ stable main" | sudo tee /etc/apt/sources.list.d/yarn.list && \
    sudo apt-get update && sudo apt-get install yarn && \
    rm -rf /var/lib/apt/lists/*

# If you need to update npm add the following line to the above command
# npm install -g npm

WORKDIR /www
