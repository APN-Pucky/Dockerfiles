FROM apnpucky/gentoo-hep-forge as gentoo-lhapdf
LABEL maintainer="APN-Pucky"
ARG LHAPDF_VERSION=6.5.4
RUN emaint -r gentoo sync
RUN emaint -r science sync
RUN echo ">=sci-physics/lhapdf-sets-0 lhapdf_sets_ct14lo lhapdf_sets_ct14nlo lhapdf_sets_cteq66 lhapdf_sets_cteq6l1" >> /etc/portage/package.use/zz-autoaccept
RUN USE="python" emerge -q ~sci-physics/lhapdf-${LHAPDF_VERSION}::gentoo sci-physics/lhapdf-sets::science && rm -rf /var/cache/distfiles/*