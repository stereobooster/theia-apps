ARG NODE_VERSION=10
FROM node:${NODE_VERSION}-stretch

RUN curl -sSL https://get.haskellstack.org/ | sh

RUN git clone https://github.com/haskell/haskell-ide-engine --recursive --depth 1 && \
    cd haskell-ide-engine && \
    stack install

ARG version=latest

WORKDIR /home/theia
ADD $version.package.json ./package.json
ARG GITHUB_TOKEN
RUN yarn --cache-folder ./ycache && rm -rf ./ycache
RUN yarn theia build
EXPOSE 3000
ENV SHELL /bin/bash
ENTRYPOINT [ "yarn", "theia", "start", "/home/project", "--hostname=0.0.0.0" ]
