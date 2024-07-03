FROM debian:buster-slim

ENV LANG C.UTF-8

# haskell dependencies
RUN apt-get update && \
    apt-get install -y --no-install-recommends \
        # ca-certificates \
        curl \
        # dpkg-dev \
        # git \
        gcc \
        # gnupg \
        g++ \
        libc6-dev \
        libffi-dev \
        libgmp-dev \
        libnuma-dev \
        libtinfo-dev \
        make \
        netbase \
        xz-utils \
        zlib1g-dev && \
    rm -rf /var/lib/apt/lists/*

ARG GHC=9.10.1

RUN set -eux; \
    cd /tmp; \
    ARCH="x86_64"; \
    GHC_URL="https://downloads.haskell.org/~ghc/$GHC/ghc-$GHC-$ARCH-deb10-linux.tar.xz"; \
    curl -sSLk "$GHC_URL" -o ghc.tar.xz; \
    tar xf ghc.tar.xz; \
    cd "ghc-$GHC-$ARCH-unknown-linux"; \
    ./configure --prefix "/opt/ghc/$GHC"; \
    make install; \
    # remove profiling support to save space
    find "/opt/ghc/$GHC/" \( -name "*_p.a" -o -name "*.p_hi" \) -type f -delete; \
    \
    # remove static libaries and interface files to save space
    find "/opt/ghc/$GHC/" \( -name "*.a" -o -name "*.hi" \) -type f -delete; \
    \
    # remove docs to save space
    rm -rf "/opt/ghc/$GHC/share/doc"; \
    \
    rm -rf /tmp/*; \
    "/opt/ghc/$GHC/bin/ghc" --version

ENV PATH /root/.cabal/bin:/root/.local/bin:/opt/ghc/${GHC}/bin:$PATH

CMD ["ghci"]
