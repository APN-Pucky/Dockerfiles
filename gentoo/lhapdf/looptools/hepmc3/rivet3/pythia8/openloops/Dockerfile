FROM apnpucky/gentoo-lhapdf-looptools-hepmc3-rivet3-pythia8 as gentoo-lhapdf-looptools-hepmc3-rivet3-pythia8-openloops
ARG OPENLOOPS_VERSION=2.1.3
RUN emaint -r science sync
RUN echo 'sci-physics/openloops ~*' >> /etc/portage/package.accept_keywords/zz-autoaccept
RUN emerge -q ~sci-physics/openloops-${OPENLOOPS_VERSION}::science && rm -rf /var/cache/distfiles/*