FROM agaveapi/gridengine

# We need to install MAKEDEV first or the update fails with dep problems!
RUN yum install -y MAKEDEV && \
  yum --disablerepo="epel" update -y && \
  yum install -y yum-utils && \
  yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo && \
  yum install -y docker-ce-cli

RUN yum install -y centos-release-scl && \
  yum install -y python27

RUN ln -s /usr/bin/python2.6 /usr/local/bin/python

# Install singularity
RUN yum update -y && \
  yum install -y openssl-devel \
  libuuid-devel \
  libseccomp-devel \
  wget \
  squashfs-tools \
  gcc \
  tar \
  git

RUN export VERSION=1.12 OS=linux ARCH=amd64 && \
  wget https://dl.google.com/go/go$VERSION.$OS-$ARCH.tar.gz && \
  tar -C /usr/local -xzvf go$VERSION.$OS-$ARCH.tar.gz && \
  rm go$VERSION.$OS-$ARCH.tar.gz

ENV GOPATH /go

ENV PATH /usr/local/go/bin:${PATH}:${GOPATH}/bin

RUN go get -u github.com/golang/dep/cmd/dep

RUN export VERSION=3.0.3 && \
  mkdir -p /go/src/github.com/sylabs && \
  cd /go/src/github.com/sylabs && \
  wget https://github.com/sylabs/singularity/releases/download/v${VERSION}/singularity-${VERSION}.tar.gz && \
  tar -xzf singularity-${VERSION}.tar.gz && \
  cd ./singularity && \
  ./mconfig

RUN cd /go/src/github.com/sylabs/singularity/builddir/ && \
  make && \
  make install

COPY entrypoint.sh /entrypoint.sh

ENTRYPOINT  [  "/entrypoint.sh" ]
CMD ["/usr/bin/supervisord"]
