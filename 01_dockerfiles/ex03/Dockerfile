FROM debian

RUN apt-get update -y && apt-get upgrade -y

RUN apt-get install wget git gcc -y

# Source info https://gogs.io/docs/installation/install_from_source.html
RUN adduser --disabled-login --gecos 'Gogs' git
USER git

RUN mkdir -p /home/git/local
WORKDIR /home/git/local

# Download go
RUN wget https://storage.googleapis.com/golang/go1.8.linux-amd64.tar.gz && \
    tar -xzvf go1.8.linux-amd64.tar.gz

# init env go
ENV GOROOT=/home/git/local/go
ENV GOPATH=/home/git/go
ENV PATH=$PATH:$GOROOT/bin:$GOPATH/bin

# gogs installation
RUN mkdir -p /home/git/go/src/github.com/gogits
WORKDIR $GOPATH/src/github.com/gogits
RUN git clone --depth=1 -b develop https://github.com/gogits/gogs.git
WORKDIR $GOPATH/src/github.com/gogits/gogs
RUN go build
RUN go get -u -tags "sqlite" github.com/gogits/gogs
RUN go build -tags "sqlite"

EXPOSE 3000
ENV USER=git

ENTRYPOINT ./gogs web
