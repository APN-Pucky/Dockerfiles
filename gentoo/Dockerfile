# gentoo-base
FROM gentoo/portage:latest as portage-base
FROM gentoo/stage3:latest as gentoo-base
LABEL maintainer="APN-Pucky"
COPY --from=portage-base /var/db/repos/gentoo /var/db/repos/gentoo
RUN mkdir -p /etc/portage/repos.conf/ && cp /usr/share/portage/config/repos.conf /etc/portage/repos.conf/gentoo.conf

# gentoo-ebuild-ci
FROM gentoo/portage:latest as portage
FROM gentoo/stage3:latest as gentoo-ebuild-ci
LABEL maintainer="APN-Pucky"
COPY --from=portage /var/db/repos/gentoo /var/db/repos/gentoo
RUN emerge -q dev-vcs/git app-portage/repoman dev-util/pkgdev && rm -rf /var/cache/distfiles/*
RUN mkdir -p /etc/portage/repos.conf/ && cp /usr/share/portage/config/repos.conf /etc/portage/repos.conf/gentoo.conf

# gentoo-dev
FROM apnpucky/gentoo-ebuild-ci as gentoo-dev
RUN emerge -q virtual/fortran dev-util/cmake dev-libs/boost app-portage/pkg-testing-tools && rm -rf /var/cache/distfiles/*
RUN mkdir /etc/portage/package.unmask && mkdir /etc/portage/package.env && mkdir /etc/portage/env/ && printf 'COMMON_FLAGS="-march=native -O2 -pipe -frecord-gcc-switches"\nCFLAGS="${COMMON_FLAGS}"\nCXXFLAGS="${COMMON_FLAGS}"\nFCFLAGS="${COMMON_FLAGS}"\nFFLAGS="${COMMON_FLAGS}"\nLDFLAGS="${LDFLAGS} -Wl,--defsym=__gentoo_check_ldflags__=0"\nFEATURES="collision-protect ipc-sandbox network-sandbox sandbox split-log split-elog strict test userfetch userpriv usersandbox"\nFEATURES="${FEATURES} -parallel-install"\nPORTAGE_ELOG_CLASSES="log warn error qa"\nPORTAGE_ELOG_SYSTEM="echo save"\nDISTUTILS_STRICT_ENTRY_POINTS=1\nALLOW_TEST="network"\nIWDT_ALL=y\nQA_CMP=y\nQA_CMP_ARGS="-xS"\nQA_SED=y\nQA_VDB=y' > /etc/portage/env/test.conf

# gentoo-hep-forge
FROM apnpucky/gentoo-dev as gentoo-hep-forge
#RUN echo 'PYTHON_TARGETS="python3_9"' >> /etc/portage/make.conf
RUN PYTHON_TARGETS="python3_9" emerge -q sci-physics/lhapdf sci-libs/gsl && rm -rf /var/cache/distfiles/*
RUN echo 'ACCEPT_KEYWORDS="~amd64"' >> /etc/portage/make.conf
RUN mkdir /var/db/repos/hep-forge && printf "[hep-forge]\nlocation = /var/db/repos/hep-forge\nsync-type = git\nsync-uri = https://gitlab.com/APN-Pucky/gentoo-hep-forge.git" >> /etc/portage/repos.conf/hep-forge.conf && emaint -r hep-forge sync

# gentoo-latex
FROM apnpucky/gentoo-base as gentoo-latex
LABEL maintainer="APN-Pucky"
COPY --from=portage /var/db/repos/gentoo /var/db/repos/gentoo
RUN echo '>=media-libs/harfbuzz-4.3.0 icu' > /etc/portage/package.use/harfbuzz
RUN emerge -q latexmk biber app-text/texlive dev-texlive/texlive-latex dev-python/pygments && rm -rf /var/cache/distfiles/*

