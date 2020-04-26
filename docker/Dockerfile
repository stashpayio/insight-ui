FROM node:8-alpine

RUN apk add --update --no-cache git \
                                libzmq \
                                zeromq-dev \
                                python \
                                make \
                                g++

WORKDIR /insight

# Copy stashcore-node
RUN git clone --branch master --single-branch --depth 1 https://github.com/stashcore/stashcore-node.git .

# Copy config file
COPY stashcore-node.json .

# Install NPM modules
RUN npm ci

ARG VERSION
ARG MAJOR_VERSION

# Install Insight modules
RUN bin/stashcore-node install @stashcore/insight-api@${MAJOR_VERSION}
RUN bin/stashcore-node install @stashcore/insight-ui@${VERSION}

FROM node:8-alpine

LABEL maintainer="Stash Developers <dev@stash.org>"
LABEL description="Dockerised Insight-Stash"

WORKDIR /insight

# Copy project files
COPY --from=0 /insight/ .

EXPOSE 3001

CMD ["bin/stashcore-node", "start"]
