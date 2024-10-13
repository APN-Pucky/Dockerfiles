FROM apnpucky/gentoo-hep-forge as gentoo-yoda
LABEL maintainer="APN-Pucky"
ARG YODA_VERSION=2.0.1
RUN emaint -r gentoo sync
RUN echo 'sci-physics/yoda ~amd64' >> /etc/portage/package.accept_keywords/zz-autoaccept
RUN USE="python" emerge -q ~sci-physics/yoda-${YODA_VERSION}::gentoo && rm -rf /var/cache/distfiles/*
