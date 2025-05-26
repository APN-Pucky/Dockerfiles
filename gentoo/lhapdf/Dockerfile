FROM apnpucky/gentoo-hep-forge as gentoo-lhapdf
LABEL maintainer="APN-Pucky"
ARG LHAPDF_VERSION=6.5.5
RUN emaint -r gentoo sync
RUN emaint -r science sync
RUN echo ">=sci-physics/lhapdf-sets-0 lhapdf_sets_ct14nlo lhapdf_sets_ct14lo lhapdf_sets_nnpdf31_nnlo_as_0118 lhapdf_sets_nnpdf23_nlo_as_0118 lhapdf_sets_ct14nlo lhapdf_sets_ct14lo lhapdf_sets_ct10 lhapdf_sets_cteq6l1 lhapdf_sets_nnpdf23_nlo_as_0119_qe" >> /etc/portage/package.use/zz-autoaccept
RUN USE="python" emerge -q ~sci-physics/lhapdf-${LHAPDF_VERSION}::gentoo sci-physics/lhapdf-sets::science && rm -rf /var/cache/distfiles/*