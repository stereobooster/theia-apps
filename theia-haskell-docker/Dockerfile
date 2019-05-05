ARG NODE_VERSION=10
FROM node:${NODE_VERSION}-stretch

RUN curl -sSL https://get.haskellstack.org/ | sh

RUN addgroup --system nixbld && \
  adduser --home /home/nix --disabled-password --gecos "" --shell /bin/bash nix && \
  adduser nix nixbld && \
  mkdir -m 0755 /nix && chown nix /nix && \
  mkdir -p /etc/nix && echo 'sandbox = false' > /etc/nix/nix.conf

CMD /bin/bash -l
USER nix
ENV USER nix
WORKDIR /home/nix

RUN touch .bash_profile && \
  curl https://nixos.org/nix/install | sh

RUN . /home/nix/.nix-profile/etc/profile.d/nix.sh && \
  nix-env -iA hies -f https://github.com/domenkozar/hie-nix/tarball/master

ARG version=latest

WORKDIR /home/theia
ADD $version.package.json ./package.json
ARG GITHUB_TOKEN
RUN yarn --cache-folder ./ycache && rm -rf ./ycache
RUN yarn theia build
EXPOSE 3000
ENV SHELL /bin/bash
ENTRYPOINT [ "yarn", "theia", "start", "/home/project", "--hostname=0.0.0.0" ]
