FROM apnpucky/gentoo-hep-forge as gentoo-rivet3-hepmc3
LABEL maintainer="APN-Pucky"
ARG RIVET3_VERSION=3.1.6
ARG HEPMC3_VERSION=3.2.5
RUN emaint -r hep-forge sync
RUN USE="hepmc3 -hepmc2 python" emerge -q ~sci-physics/rivet-${RIVET3_VERSION} ~sci-physics/hepmc-${HEPMC3_VERSION} && rm -rf /var/cache/distfiles/*

FROM apnpucky/gentoo-hep-forge as gentoo-rivet3-hepmc2
LABEL maintainer="APN-Pucky"
ARG RIVET3_VERSION=3.1.6
ARG HEPMC2_VERSION=2.06.11
RUN emaint -r hep-forge sync
RUN USE="-hepmc3 hepmc2 python" emerge -q ~sci-physics/rivet-${RIVET3_VERSION} ~sci-physics/hepmc-${HEPMC2_VERSION} && rm -rf /var/cache/distfiles/*

FROM apnpucky/gentoo-lhapdf as gentoo-rivet3-herwig7-hepmc3-lhapdf
LABEL maintainer="APN-Pucky"
ARG HEPMC3_VERSION=3.2.6
ARG RIVET3_VERSION=3.1.9
ARG HERWIG7_VERSION=7.3.0
ARG LHAPDF_VERSION=6.5.4
RUN emaint -r hep-forge sync
RUN echo "sci-physics/evtgen pythia herwig" >> /etc/portage/package.use/zz-autoaccept
RUN echo "sci-physics/thepeg lhapdf hepmc3 fastjet rivet" >> /etc/portage/package.use/zz-autoaccept
RUN USE="hepmc3 -hepmc2" emerge -q ~sci-physics/rivet-${RIVET3_VERSION} ~sci-physics/herwig-${HERWIG7_VERSION} ~sci-physics/hepmc-${HEPMC2_VERSION}  ~sci-physics/lhapdf-${LHAPDF_VERSION} && rm -rf /var/cache/distfiles/*

FROM apnpucky/gentoo-lhapdf as gentoo-rivet3-herwig7-hepmc2-lhapdf
LABEL maintainer="APN-Pucky"
ARG HEPMC2_VERSION=2.06.11
ARG RIVET3_VERSION=3.1.6
ARG HERWIG7_VERSION=7.2.3
ARG LHAPDF_VERSION=6.5.4
RUN emaint -r hep-forge sync
RUN echo "sci-physics/evtgen pythia herwig" >> /etc/portage/package.use/zz-autoaccept
RUN echo "sci-physics/thepeg lhapdf hepmc2 fastjet rivet" >> /etc/portage/package.use/zz-autoaccept
RUN USE="-hepmc3 hepmc2" emerge -q ~sci-physics/rivet-${RIVET3_VERSION} ~sci-physics/herwig-${HERWIG7_VERSION} ~sci-physics/hepmc-${HEPMC2_VERSION}  ~sci-physics/lhapdf-${LHAPDF_VERSION} && rm -rf /var/cache/distfiles/*


FROM apnpucky/gentoo-lhapdf as gentoo-rivet3-pythia8-hepmc3-lhapdf
LABEL maintainer="APN-Pucky"
ARG HEPMC3_VERSION=3.1.6
ARG RIVET3_VERSION=3.1.6
ARG PYTHIA8_VERSION=8.3.07
RUN emaint -r hep-forge sync
RUN USE="hepmc3 -hepmc2 python lhapdf" emerge -q ~sci-physics/rivet-${RIVET3_VERSION} ~sci-physics/pythia-${PYTHIA8_VERSION} ~sci-physics/hepmc-${HEPMC3_VERSION} && rm -rf /var/cache/distfiles/*


FROM apnpucky/gentoo-hep-forge as gentoo-rivet3-pythia8-hepmc2-lhapdf
LABEL maintainer="APN-Pucky"
ARG RIVET3_VERSION=3.1.6
ARG PYTHIA8_VERSION=8.3.07
ARG LHAPDF_VERSION=6.5.4
ARG HEPMC2_VERSION=2.06.11
RUN emaint -r hep-forge sync
RUN USE="-hepmc3 hepmc2 python lhapdf" emerge -q ~sci-physics/rivet-${RIVET3_VERSION} ~sci-physics/pythia-${PYTHIA8_VERSION} ~sci-physics/hepmc-${HEPMC2_VERSION} ~sci-physics/lhapdf-${LHAPDF_VERSION} && rm -rf /var/cache/distfiles/*


FROM apnpucky/gentoo-lhapdf-looptools as gentoo-rivet3-pythia8-hepmc3-lhapdf-looptools
LABEL maintainer="APN-Pucky"
ARG HEPMC3_VERSION=3.1.6
ARG RIVET3_VERSION=3.1.6
ARG PYTHIA8_VERSION=8.3.07
RUN emaint -r hep-forge sync
RUN USE="hepmc3 -hepmc2 python lhapdf" emerge -q ~sci-physics/rivet-${RIVET3_VERSION} ~sci-physics/pythia-${PYTHIA8_VERSION} ~sci-physics/hepmc-${HEPMC3_VERSION} && rm -rf /var/cache/distfiles/*

FROM apnpucky/gentoo-lhapdf-looptools as gentoo-rivet3-pythia8-hepmc2-lhapdf-looptools
LABEL maintainer="APN-Pucky"
ARG RIVET3_VERSION=3.1.6
ARG PYTHIA8_VERSION=8.3.07
ARG HEPMC2_VERSION=2.06.11
RUN emaint -r hep-forge sync
RUN USE="-hepmc3 hepmc2 python lhapdf" emerge -q ~sci-physics/rivet-${RIVET3_VERSION} ~sci-physics/pythia-${PYTHIA8_VERSION} ~sci-physics/hepmc-${HEPMC2_VERSION} && rm -rf /var/cache/distfiles/*


FROM apnpucky/gentoo-lhapdf-looptools as gentoo-powheg-box-v2
LABEL maintainer="APN-Pucky"
ARG POWHEG_BOX_V2_VERSION=4027
RUN emaint -r hep-forge sync
RUN emerge -q virtual/powheg-box-v2 && rm -rf /var/cache/distfiles/*

FROM apnpucky/gentoo-lhapdf as gentoo-susy
LABEL maintainer="APN-Pucky"
ARG NNLLFAST_VERSION=1.1
ARG NLLFAST_VERSION=3.1
ARG SPHENO_VERSION=3.3.8
ARG SOFTSUSY_VERSION=4.1.13
ARG RESUMMINO_VERSION=3.1.2
#ARG FEYNHIGGS_VERSION=2.18.1
RUN emaint -r hep-forge sync && echo "rebuild"
RUN emerge -q ~resummino-$RESUMMINO_VERSION ~nll-fast-$NLLFAST_VERSION ~nnll-fast-$NNLLFAST_VERSION ~softsusy-$SOFTSUSY_VERSION ~spheno-$SPHENO_VERSION && rm -rf /var/cache/distfiles/*

FROM apnpucky/gentoo-susy as gentoo-hepi
LABEL maintainer="APN-Pucky"
RUN emaint -r hep-forge sync
RUN echo 'dev-python/poetry ~amd64' >> /etc/portage/package.accept_keywords/zz-autoaccept
RUN echo 'dev-python/poetry-plugin-export ~amd64' >> /etc/portage/package.accept_keywords/zz-autoaccept
RUN echo 'dev-python/shellingham ~amd64' >> /etc/portage/package.accept_keywords/zz-autoaccept
RUN emerge -q poetry && rm -rf /var/cache/distfiles/*
RUN emerge -q pandoc-bin && rm -rf /var/cache/distfiles/*

