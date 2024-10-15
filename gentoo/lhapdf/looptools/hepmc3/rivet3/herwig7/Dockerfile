FROM apnpucky/gentoo-lhapdf-looptools as gentoo-lhapdf-looptools-hepmc3-rivet3-herwig7
LABEL maintainer="APN-Pucky"
ARG HEPMC3_VERSION=3.2.6
ARG RIVET3_VERSION=3.1.9
ARG HERWIG7_VERSION=7.3.0
RUN emaint -r hep-forge sync
RUN echo "sci-physics/evtgen pythia herwig" >> /etc/portage/package.use/zz-autoaccept
RUN echo "sci-physics/thepeg lhapdf hepmc3 fastjet rivet" >> /etc/portage/package.use/zz-autoaccept
RUN echo 'sci-physics/herwig ~*' >> /etc/portage/package.accept_keywords/zz-autoaccept
RUN echo 'sci-physics/thepeg ~*' >> /etc/portage/package.accept_keywords/zz-autoaccept
RUN echo 'sci-physics/hepmc ~*' >> /etc/portage/package.accept_keywords/zz-autoaccept
RUN echo 'sci-physics/yoda ~*' >> /etc/portage/package.accept_keywords/zz-autoaccept
RUN echo 'sci-physics/rivet ~*' >> /etc/portage/package.accept_keywords/zz-autoaccept
RUN USE="hepmc3 -hepmc2" emerge -q ~sci-physics/rivet-${RIVET3_VERSION}::gentoo ~sci-physics/herwig-${HERWIG7_VERSION}::gentoo ~sci-physics/hepmc-${HEPMC3_VERSION}::gentoo  && rm -rf /var/cache/distfiles/*