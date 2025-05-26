FROM gentoo/portage:latest AS portage-base
FROM gentoo/stage3:latest AS gentoo-base
LABEL maintainer="APN-Pucky"
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