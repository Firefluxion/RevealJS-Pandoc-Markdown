FROM pandoc/latex:2.14.0.2

RUN apk --no-cache add make

# Install Microsoft True Type Fonts
RUN apk --no-cache add msttcorefonts-installer fontconfig && \
    update-ms-fonts && \
    fc-cache -f

#### From https://github.com/nbrownuk/docker-revealjs/blob/3.5.0/alpine/Dockerfile

# npm loglevel in base image is verbose, adjust to warnings only
ENV NPM_CONFIG_LOGLEVEL warn

# Set environment variables for reveal.js GitHub repo
ENV VERSION=4.1.2 \
    REPO=https://github.com/hakimel/reveal.js \
    SHA1=b444529be8a1041942f3f30ae4f626e10497498c

RUN set -ex \
    \
# Add tini for handling signals
# https://github.com/nodejs/docker-node/blob/master/docs/BestPractices.md#handling-kernel-signals
    && apk add --no-cache tini \
    \
# Install dependencies for fetching reveal.js source code
    && apk add --no-cache --virtual .fetch-deps \
        ca-certificates \
        tar \
        wget \
# Fetch reveal.js
    && wget -qO /tmp/reveal.js.tar.gz $REPO/archive/$VERSION.tar.gz \
    && echo "$SHA1  /tmp/reveal.js.tar.gz" \
    && tar -xzf /tmp/reveal.js.tar.gz -C / \
    && rm -f /tmp/reveal.js.tar.gz \
    && echo $(ls -a -h /) \
    && echo $(ls -a -h /reveal.js-4.1.2) \
    && mv /reveal.js-$VERSION /reveal.js \
    \
# Install build tools
    # && apk add --no-cache --virtual .build-deps \
    #     g++ \
    #     make \
    #     python \
    \
#install npm
    && apk add --update nodejs npm \
    && apk add --update npm \
    \
# Build and install dependencies
    && mkdir -p /reveal.js/node_modules \
    && npm install -g grunt-cli \
    && npm install --prefix /reveal.js \
    \
# Clean up
    && npm cache clean --force \
    && rm -rf /root/.node-gyp /tmp/npm* /tmp/phantomjs \
    && apk del .fetch-deps 
    #&& apk del .build-deps \
    #&& chown -R node:node /reveal.js

WORKDIR /reveal.js

ENTRYPOINT tail -f /dev/null
CMD [ "/bin/bash" ]
