FROM ubuntu:16.04
#MAINTAINER nick.humrich@canopytax.com

# Set the NODEJS_VERSION below to change the node version
ENV NODEJS_VERSION=8.11.2
ENV DEBIAN_FRONTEND=noninteractive


COPY woff-code-latest.zip /tmp/
# Make sure python, awscli, fontcustom, and git are all installed.
RUN apt -y update && \
    apt install -y python python-pip python-dev wget unzip git build-essential zlib1g-dev ruby-full rubygems fontforge apt-transport-https ca-certificates && \
    pip install awscli

RUN git clone https://github.com/bramstein/sfnt2woff-zopfli.git sfnt2woff-zopfli && cd sfnt2woff-zopfli && make && mv sfnt2woff-zopfli /usr/local/bin/sfnt2woff && \
    git clone --recursive https://github.com/google/woff2.git && cd woff2 && make clean all && mv woff2_compress /usr/local/bin/ && mv woff2_decompress /usr/local/bin/ && \
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
RUN curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | apt-key add - && \
    echo "deb https://dl.yarnpkg.com/debian/ stable main" | tee /etc/apt/sources.list.d/yarn.list && \
    apt update && apt install --no-install-recommends yarn -y && \
    rm -rf /var/lib/apt/lists/*

# https://github.com/GoogleChrome/puppeteer/blob/master/docs/troubleshooting.md#running-puppeteer-in-docker
# See https://crbug.com/795759
RUN apt-get update && apt-get install -yq libgconf-2-4

# Install latest chrome dev package and fonts to support major charsets (Chinese, Japanese, Arabic, Hebrew, Thai and a few others)
# Note: this installs the necessary libs to make the bundled version of Chromium that Puppeteer
# installs, work.
RUN apt-get update && apt-get install -y wget --no-install-recommends \
    && wget -q -O - https://dl-ssl.google.com/linux/linux_signing_key.pub | apt-key add - \
    && sh -c 'echo "deb [arch=amd64] http://dl.google.com/linux/chrome/deb/ stable main" >> /etc/apt/sources.list.d/google.list' \
    && apt-get update \
    && apt-get install -y google-chrome-unstable fonts-ipafont-gothic fonts-wqy-zenhei fonts-thai-tlwg fonts-kacst ttf-freefont \
      --no-install-recommends \
    && rm -rf /var/lib/apt/lists/* \
    && apt-get purge --auto-remove -y curl \
    && rm -rf /src/*.deb

# It's a good idea to use dumb-init to help prevent zombie chrome processes.
ADD https://github.com/Yelp/dumb-init/releases/download/v1.2.0/dumb-init_1.2.0_amd64 /usr/local/bin/dumb-init
RUN chmod +x /usr/local/bin/dumb-init

# Uncomment to skip the chromium download when installing puppeteer. If you do,
# you'll need to launch puppeteer with:
#     browser.launch({executablePath: 'google-chrome-unstable'})
# ENV PUPPETEER_SKIP_CHROMIUM_DOWNLOAD true

# Install puppeteer so it's available in the container.
RUN npm i puppeteer

# Add user so we don't need --no-sandbox.
RUN groupadd -r pptruser && useradd -r -g pptruser -G audio,video pptruser \
    && mkdir -p /home/pptruser/Downloads \
    && chown -R pptruser:pptruser /home/pptruser \
    && chown -R pptruser:pptruser /node_modules

# Run everything after as non-privileged user.
USER pptruser

ENTRYPOINT ["dumb-init", "--"]
CMD ["google-chrome-unstable"]
