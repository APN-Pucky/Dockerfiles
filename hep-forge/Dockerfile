FROM apnpucky/gentoo-hep-forge as gentoo-lhapdf
LABEL maintainer="APN-Pucky"
ARG LHAPDF_VERSION=6.5.4
RUN emaint -r hep-forge sync
RUN USE="python" emerge -q ~sci-physics/lhapdf-${LHAPDF_VERSION} && rm -rf /var/cache/distfiles/*

FROM apnpucky/gentoo-hep-forge as gentoo-lhapdf-looptools
LABEL maintainer="APN-Pucky"
ARG LOOPTOOLS_VERSION=2.15
ARG LHAPDF_VERSION=6.5.4
RUN emaint -r hep-forge sync
RUN echo 'sci-physics/looptools **' >> /etc/portage/package.accept_keywords/zz-autoaccept
RUN USE="python" emerge -q ~sci-physics/lhapdf-${LHAPDF_VERSION} ~sci-physics/looptools-${LOOPTOOLS_VERSION} && rm -rf /var/cache/distfiles/*

FROM apnpucky/gentoo-hep-forge as gentoo-rivet3-hepmc3
LABEL maintainer="APN-Pucky"
ARG RIVET_VERSION=3.1.6
ARG HEPMC_VERSION=3.2.5
RUN emaint -r hep-forge sync
RUN USE="hepmc3 -hepmc2 python" emerge -q ~sci-physics/rivet-${RIVET_VERSION} ~sci-physics/hepmc-${HEPMC_VERSION} && rm -rf /var/cache/distfiles/*

FROM apnpucky/gentoo-hep-forge as gentoo-rivet3-pythia8-hepmc3-lhapdf-looptools
LABEL maintainer="APN-Pucky"
ARG HEPMC3_VERSION=3.1.6
ARG RIVET3_VERSION=3.1.6
ARG PYTHIA8_VERSION=8.3.07
ARG LHAPDF_VERSION=6.5.4
ARG LOOPTOOLS_VERSION=2.15
RUN emaint -r hep-forge sync
RUN USE="hepmc3 -hepmc2 python lhapdf" emerge -q ~sci-physics/rivet-${RIVET_VERSION} ~sci-physics/pythia-${PYTHIA_VERSION} ~sci-physics/hepmc-${HEPMC_VERSION}  ~sci-physics/lhapdf-${LHAPDF_VERSION} ~sci-physics/looptools-${LOOPTOOLS_VERSION} && rm -rf /var/cache/distfiles/*

FROM apnpucky/gentoo-hep-forge as gentoo-rivet3-pythia8-hepmc2-lhapdf-looptools
LABEL maintainer="APN-Pucky"
ARG RIVET3_VERSION=3.1.6
ARG PYTHIA8_VERSION=8.3.07
ARG LHAPDF_VERSION=6.5.4
ARG LOOPTOOLS_VERSION=2.15
ARG HEPMC2_VERSION=2.06.11
RUN emaint -r hep-forge sync
RUN USE="-hepmc3 hepmc2 python lhapdf" emerge -q =sci-physics/rivet-${RIVET_VERSION} =sci-physics/pythia-${PYTHIA_VERSION} =sci-physics/hepmc-${HEPMC_VERSION} ~sci-physics/lhapdf-${LHAPDF_VERSION} ~sci-physics/looptools-${LOOPTOOLS_VERSION} && rm -rf /var/cache/distfiles/*


# gentoo-latex
FROM apnpucky/gentoo-base as gentoo-latex
LABEL maintainer="APN-Pucky"
RUN echo '>=media-libs/harfbuzz-4.3.0 icu -introspection' > /etc/portage/package.use/harfbuzz
RUN emerge -q latexmk biber app-text/texlive dev-texlive/texlive-latex dev-python/pygments && rm -rf /var/cache/distfiles/*

# gentoo-prefix
FROM apnpucky/gentoo-base as prefix
LABEL maintainer="APN-Pucky"
RUN wget https://gitweb.gentoo.org/repo/proj/prefix.git/plain/scripts/bootstrap-prefix.sh && chmod +x bootstrap-prefix.sh && sed -i 's/UID/NOUID/g' bootstrap-prefix.sh && ./bootstrap-prefix.sh /gentoo-prefix noninteractive
# set entry point to start gentoo prefix
ENTRYPOINT ["/gentoo-prefix/startprefix"]