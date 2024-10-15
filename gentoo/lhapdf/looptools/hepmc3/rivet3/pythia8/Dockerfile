FROM apnpucky/gentoo-lhapdf-looptools as gentoo-lhapdf-looptools-hepmc3-rivet3-pythia8
LABEL maintainer="APN-Pucky"
ARG HEPMC3_VERSION=3.1.6
ARG RIVET3_VERSION=3.1.6
ARG PYTHIA8_VERSION=8.3.07
RUN emaint -r hep-forge sync
RUN echo 'sci-physics/pythia ~*' >> /etc/portage/package.accept_keywords/zz-autoaccept
RUN USE="hepmc3 -hepmc2 python lhapdf" emerge -q ~sci-physics/rivet-${RIVET3_VERSION} ~sci-physics/pythia-${PYTHIA8_VERSION} ~sci-physics/hepmc-${HEPMC3_VERSION} && rm -rf /var/cache/distfiles/*