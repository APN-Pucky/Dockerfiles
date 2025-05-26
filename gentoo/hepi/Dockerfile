FROM gentoo/portage:latest AS portage-base
FROM gentoo/stage3:latest AS gentoo-hepi
LABEL maintainer="APN-Pucky"

#ARG NNLLFAST_VERSION=1.1
#ARG NLLFAST_VERSION=3.1
#ARG SPHENO_VERSION=3.3.8
#ARG SOFTSUSY_VERSION=4.1.13
#ARG RESUMMINO_VERSION=3.1.2
ARG HEPI_VERSION=0.2.10
#ARG FEYNHIGGS_VERSION=2.18.1

ARG NICENESS=14
COPY --from=portage-base /var/db/repos/gentoo /var/db/repos/gentoo
RUN mkdir -p /etc/portage/repos.conf/ && cp /usr/share/portage/config/repos.conf /etc/portage/repos.conf/gentoo.conf
RUN echo "PORTAGE_NICENESS=\"${NICENESS}\"" >> /etc/portage/make.conf
RUN echo "FEATURES=\"${FEATURES} getbinpkg\"" >> /etc/portage/make.conf
#RUN printf "" > /etc/portage/binrepos.conf/gentoobinhost.conf
RUN printf "[amd64gentoobinhost]\npriority=9999\nsync-uri=https://ftp.uni-stuttgart.de/gentoo-distfiles/releases/amd64/binpackages/23.0/x86-64/\n"  > /etc/portage/binrepos.conf/amd64gentoobinhost.conf
RUN printf "[arm64gentoobinhost]\npriority=9999\nsync-uri=https://ftp.uni-stuttgart.de/gentoo-distfiles/releases/arm64/binpackages/23.0/arm64/\n" > /etc/portage/binrepos.conf/arm64gentoobinhost.conf
RUN printf "[armgentoobinhost]\npriority=9999\nsync-uri=https://ftp.uni-stuttgart.de/gentoo-distfiles/releases/arm/binpackages/23.0/armv7a_hf/\n" > /etc/portage/binrepos.conf/armgentoobinhost.conf
RUN printf "[riscvgentoobinhost]\npriority=9999\nsync-uri=https://ftp.uni-stuttgart.de/gentoo-distfiles/releases/riscv/binpackages/23.0/rv64_lp64d/\n" > /etc/portage/binrepos.conf/riscvgentoobinhost.conf
RUN mv /etc/portage/gnupg /etc/portage/gnupg.bak ; getuto


RUN emerge -q eix dev-vcs/git && rm -rf /var/cache/distfiles/*
RUN rm -rf /var/db/repos/gentoo && mkdir -p /var/db/repos/gentoo && printf "[DEFAULT]\nmain-repo = gentoo\n\n[gentoo]\nlocation = /var/db/repos/gentoo\nsync-type = git\nsync-uri = https://github.com/gentoo-mirror/gentoo.git\nauto-sync = yes\nsync-git-verify-commit-signature = yes\nsync-openpgp-key-path = /usr/share/openpgp-keys/gentoo-release.asc" > /etc/portage/repos.conf/gentoo.conf && emerge --sync 
RUN eix-update
RUN install -m 0750 -o portage -g portage -d /etc/portage/env /etc/portage/package.{accept_keywords,env,unmask,use}

RUN emerge app-eselect/eselect-repository
RUN eselect repository enable science && emaint -r science sync
RUN eselect repository enable hep-forge && emaint -r hep-forge sync


RUN echo ">=sci-physics/lhapdf-sets-0 lhapdf_sets_ct14lo lhapdf_sets_ct14nlo lhapdf_sets_cteq66 lhapdf_sets_cteq6l1 lhapdf_sets_ct10nnlo lhapdf_sets_pdf4lhc15_nnlo_100" >> /etc/portage/package.use/zz-autouse
RUN echo 'dev-python/uncertainties ~*' >> /etc/portage/package.accept_keywords/zz-autoaccept
RUN emerge -q --autounmask=y --autounmask-write --autounmask-continue --autounmask-use=y sci-physics/lhapdf-sets dev-python/pip hatch lhapdf resummino nll-fast nnll-fast:1 softsusy ~sci-physics/spheno-3.3.8  && rm -rf /var/cache/distfiles/* # ~hepi-$HEPI_VERSION