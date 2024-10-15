FROM apnpucky/gentoo-hepi as gentoo-hepi
LABEL maintainer="APN-Pucky"
#ARG NNLLFAST_VERSION=1.1
#ARG NLLFAST_VERSION=3.1
#ARG SPHENO_VERSION=3.3.8
#ARG SOFTSUSY_VERSION=4.1.13
#ARG RESUMMINO_VERSION=3.1.2
ARG HEPI_VERSION=0.2.8
#ARG FEYNHIGGS_VERSION=2.18.1
RUN emaint -r hep-forge sync
RUN echo 'dev-python/uncertainties ~*' >> /etc/portage/package.accept_keywords/zz-autoaccept
RUN emerge -q resummino nll-fast nnll-fast softsusy spheno ~hepi-$HEPI_VERSION && rm -rf /var/cache/distfiles/*