FROM apnpucky/gentoo-lhapdf-looptools as gentoo-lhapdf-looptools-hepmc3-rivet3-pythia8
ARG HEPMC3_VERSION=3.1.6
ARG RIVET3_VERSION=3.1.6
ARG PYTHIA8_VERSION=8.3.07
RUN emaint -r hep-forge sync
RUN echo 'sci-physics/pythia ~*' >> /etc/portage/package.accept_keywords/zz-autoaccept
RUN echo 'sci-physics/hepmc ~*' >> /etc/portage/package.accept_keywords/zz-autoaccept
RUN echo 'sci-physics/yoda ~*' >> /etc/portage/package.accept_keywords/zz-autoaccept
RUN echo 'sci-physics/rivet ~*' >> /etc/portage/package.accept_keywords/zz-autoaccept
RUN USE="hepmc3 -hepmc2 python lhapdf" emerge -q ~sci-physics/rivet-${RIVET3_VERSION}::gentoo ~sci-physics/pythia-${PYTHIA8_VERSION}::gentoo ~sci-physics/hepmc-${HEPMC3_VERSION}::gentoo && rm -rf /var/cache/distfiles/*