FROM apnpucky/gentoo-hep-forge as gentoo-resummino
LABEL maintainer="APN-Pucky"
#ARG NNLLFAST_VERSION=1.1
#ARG NLLFAST_VERSION=3.1
#ARG SPHENO_VERSION=3.3.8
#ARG SOFTSUSY_VERSION=4.1.13
ARG RESUMMINO_VERSION=3.1.2
#ARG FEYNHIGGS_VERSION=2.18.1
RUN emaint -r hep-forge sync
RUN emerge -q ~resummino-$RESUMMINO_VERSION && rm -rf /var/cache/distfiles/*