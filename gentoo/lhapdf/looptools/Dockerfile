FROM apnpucky/gentoo-lhapdf as gentoo-lhapdf-looptools
LABEL maintainer="APN-Pucky"
ARG LOOPTOOLS_VERSION=2.15
RUN emaint -r gentoo sync
RUN echo 'sci-physics/looptools **' >> /etc/portage/package.accept_keywords/zz-autoaccept
RUN USE="python" emerge -q ~sci-physics/looptools-${LOOPTOOLS_VERSION}::gentoo && rm -rf /var/cache/distfiles/*