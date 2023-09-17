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

FROM apnpucky/gentoo-hep-forge as gentoo-rivet3-herwig7-hepmc3-lhapdf
LABEL maintainer="APN-Pucky"
ARG HEPMC3_VERSION=3.1.6
ARG RIVET3_VERSION=3.1.6
ARG HERWIG7_VERSION=7.2.3
ARG LHAPDF_VERSION=6.5.4
RUN emaint -r hep-forge sync
RUN USE="hepmc3 -hepmc2 python lhapdf" emerge -q ~sci-physics/rivet-${RIVET3_VERSION} ~sci-physics/herwig-${HERWIG7_VERSION} ~sci-physics/hepmc-${HEPMC3_VERSION}  ~sci-physics/lhapdf-${LHAPDF_VERSION} && rm -rf /var/cache/distfiles/*


FROM apnpucky/gentoo-hep-forge as gentoo-rivet3-pythia8-hepmc3-lhapdf
LABEL maintainer="APN-Pucky"
ARG HEPMC3_VERSION=3.1.6
ARG RIVET3_VERSION=3.1.6
ARG PYTHIA8_VERSION=8.3.07
ARG LHAPDF_VERSION=6.5.4
RUN emaint -r hep-forge sync
RUN USE="hepmc3 -hepmc2 python lhapdf" emerge -q ~sci-physics/rivet-${RIVET3_VERSION} ~sci-physics/pythia-${PYTHIA8_VERSION} ~sci-physics/hepmc-${HEPMC3_VERSION}  ~sci-physics/lhapdf-${LHAPDF_VERSION} && rm -rf /var/cache/distfiles/*


FROM apnpucky/gentoo-hep-forge as gentoo-rivet3-pythia8-hepmc2-lhapdf
LABEL maintainer="APN-Pucky"
ARG RIVET3_VERSION=3.1.6
ARG PYTHIA8_VERSION=8.3.07
ARG LHAPDF_VERSION=6.5.4
ARG HEPMC2_VERSION=2.06.11
RUN emaint -r hep-forge sync
RUN USE="-hepmc3 hepmc2 python lhapdf" emerge -q ~sci-physics/rivet-${RIVET3_VERSION} ~sci-physics/pythia-${PYTHIA8_VERSION} ~sci-physics/hepmc-${HEPMC2_VERSION} ~sci-physics/lhapdf-${LHAPDF_VERSION} && rm -rf /var/cache/distfiles/*


FROM apnpucky/gentoo-hep-forge as gentoo-rivet3-pythia8-hepmc3-lhapdf-looptools
LABEL maintainer="APN-Pucky"
ARG HEPMC3_VERSION=3.1.6
ARG RIVET3_VERSION=3.1.6
ARG PYTHIA8_VERSION=8.3.07
ARG LHAPDF_VERSION=6.5.4
ARG LOOPTOOLS_VERSION=2.15
RUN emaint -r hep-forge sync
RUN USE="hepmc3 -hepmc2 python lhapdf" emerge -q ~sci-physics/rivet-${RIVET3_VERSION} ~sci-physics/pythia-${PYTHIA8_VERSION} ~sci-physics/hepmc-${HEPMC3_VERSION}  ~sci-physics/lhapdf-${LHAPDF_VERSION} ~sci-physics/looptools-${LOOPTOOLS_VERSION} && rm -rf /var/cache/distfiles/*

FROM apnpucky/gentoo-hep-forge as gentoo-rivet3-pythia8-hepmc2-lhapdf-looptools
LABEL maintainer="APN-Pucky"
ARG RIVET3_VERSION=3.1.6
ARG PYTHIA8_VERSION=8.3.07
ARG LHAPDF_VERSION=6.5.4
ARG LOOPTOOLS_VERSION=2.15
ARG HEPMC2_VERSION=2.06.11
RUN emaint -r hep-forge sync
RUN USE="-hepmc3 hepmc2 python lhapdf" emerge -q ~sci-physics/rivet-${RIVET3_VERSION} ~sci-physics/pythia-${PYTHIA8_VERSION} ~sci-physics/hepmc-${HEPMC2_VERSION} ~sci-physics/lhapdf-${LHAPDF_VERSION} ~sci-physics/looptools-${LOOPTOOLS_VERSION} && rm -rf /var/cache/distfiles/*

FROM apnpucky/gentoo-hep-forge as gentoo-rivet3-pythia8-hepmc2-lhapdf-looptools
LABEL maintainer="APN-Pucky"
ARG RIVET3_VERSION=3.1.6
ARG PYTHIA8_VERSION=8.3.07
ARG LHAPDF_VERSION=6.5.4
ARG LOOPTOOLS_VERSION=2.15
ARG HEPMC2_VERSION=2.06.11
RUN emaint -r hep-forge sync
RUN USE="-hepmc3 hepmc2 python lhapdf" emerge -q ~sci-physics/rivet-${RIVET3_VERSION} ~sci-physics/pythia-${PYTHIA8_VERSION} ~sci-physics/hepmc-${HEPMC2_VERSION} ~sci-physics/lhapdf-${LHAPDF_VERSION} ~sci-physics/looptools-${LOOPTOOLS_VERSION} && rm -rf /var/cache/distfiles/*

FROM apnpucky/gentoo-hep-forge as gentoo-scikit-hep
LABEL maintainer="APN-Pucky"
RUN emaint -r hep-forge sync
RUN echo 'dev-python/iminuit ~amd64' >> /etc/portage/package.accept_keywords/zz-autoaccept
RUN emerge -q virtual/scikit-hep && rm -rf /var/cache/distfiles/*

FROM apnpucky/gentoo-hep-forge as gentoo-powheg-box-v2
LABEL maintainer="APN-Pucky"
ARG POWHEG_BOX_V2_VERSION=4027
RUN emaint -r hep-forge sync
RUN emerge -q virtual/powheg-box-v2 && rm -rf /var/cache/distfiles/*
