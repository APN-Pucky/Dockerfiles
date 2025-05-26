FROM gentoo/portage:latest AS portage-base
FROM gentoo/stage3:latest AS gentoo-yoda
LABEL maintainer="APN-Pucky"
ARG YODA_VERSION=2.0.1

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



RUN emaint -r gentoo sync
RUN echo 'sci-physics/yoda ~* **' >> /etc/portage/package.accept_keywords/zz-autoaccept
RUN USE="python" emerge -q ~sci-physics/yoda-${YODA_VERSION}::gentoo && rm -rf /var/cache/distfiles/*