# gentoo-base
FROM gentoo/portage:latest as portage-base
FROM gentoo/stage3:latest as gentoo-base
LABEL maintainer="APN-Pucky"
COPY --from=portage-base /var/db/repos/gentoo /var/db/repos/gentoo
RUN mkdir -p /etc/portage/repos.conf/ && cp /usr/share/portage/config/repos.conf /etc/portage/repos.conf/gentoo.conf
RUN emerge --sync

# gentoo-ebuild-ci
FROM apnpucky/gentoo-base as gentoo-ebuild-ci
RUN emerge -q dev-vcs/git app-portage/repoman dev-util/pkgdev && rm -rf /var/cache/distfiles/*
RUN mkdir -p /etc/portage/repos.conf/ && cp /usr/share/portage/config/repos.conf /etc/portage/repos.conf/gentoo.conf

# gentoo-dev
FROM apnpucky/gentoo-ebuild-ci as gentoo-dev
RUN echo 'USE="-qt5 -X"' >> /etc/portage/make.conf
RUN echo 'PORTAGE_NICENESS="20"' >> /etc/portage/make.conf 
RUN echo 'PYTHON_TARGETS="python3_8 python3_9 python3_10 python3_11"' >> /etc/portage/make.conf
RUN emerge -q sys-devel/gcc sys-devel/llvm dev-util/ccache virtual/fortran dev-util/cmake dev-libs/boost app-portage/pkg-testing-tools dev-lang/perl dev-lang/python:3.8 dev-lang/python:3.9 dev-lang/python:3.10 && rm -rf /var/cache/distfiles/*
# FROM here on unstable
RUN echo 'ACCEPT_KEYWORDS="~amd64"' >> /etc/portage/make.conf
RUN emerge -q dev-lang/python:3.11 && rm -rf /var/cache/distfiles/*
ENV PKGDIR="/var/cache/binpkgs"
ENV CCACHE_DIR="/var/cache/ccache"
ENV CCACHE_SIZE="1G"
RUN install -m 0750 -o portage -g portage -d /etc/portage/env /etc/portage/package.{accept_keywords,env,unmask,use}
RUN install -m 0750 -o portage -g portage -d "${PKGDIR}" 
RUN install -m 0750 -o portage -g portage -d "${CCACHE_DIR}" 
RUN install -m 0775 -o portage -g portage -d "${CCACHE_DIR}"/{0,1,2,3,4,5,6,7,8,9,a,b,c,d,e,f}
RUN install -m 0775 -o portage -g portage -d "${CCACHE_DIR}"/{0,1,2,3,4,5,6,7,8,9,a,b,c,d,e,f}/{0,1,2,3,4,5,6,7,8,9,a,b,c,d,e,f}
# test.conf from https://wiki.gentoo.org/wiki/Package_testing
RUN printf 'COMMON_FLAGS="-march=native -O2 -pipe -frecord-gcc-switches"\nCFLAGS="${COMMON_FLAGS}"\nCXXFLAGS="${COMMON_FLAGS}"\nFCFLAGS="${COMMON_FLAGS}"\nFFLAGS="${COMMON_FLAGS}"\nLDFLAGS="${LDFLAGS} -Wl,--defsym=__gentoo_check_ldflags__=0"\nFEATURES="collision-protect ipc-sandbox network-sandbox sandbox split-log split-elog strict test userfetch userpriv usersandbox"\nFEATURES="${FEATURES} -parallel-install"\nPORTAGE_ELOG_CLASSES="log warn error qa"\nPORTAGE_ELOG_SYSTEM="echo save"\nDISTUTILS_STRICT_ENTRY_POINTS=1\nALLOW_TEST="network"\nIWDT_ALL=y\nQA_CMP=y\nQA_CMP_ARGS="-xS"\nQA_SED=y\nQA_VDB=y' > /etc/portage/env/test.conf
RUN echo 'ACCEPT_LICENSE="* -@EULA"' >> /etc/portage/make.conf


# gentoo-hep-forge
# TODO maybe install sci-physics/root here
FROM apnpucky/gentoo-dev as gentoo-hep-forge
# USE="X" for mesa for opengl for fastjet
RUN echo 'USE="X"' >> /etc/portage/make.conf
RUN mkdir /var/db/repos/hep-forge && printf "[hep-forge]\nlocation = /var/db/repos/hep-forge\nsync-type = git\nsync-uri = https://gitlab.com/APN-Pucky/gentoo-hep-forge.git" >> /etc/portage/repos.conf/hep-forge.conf && emaint -r hep-forge sync

# gentoo-latex
FROM apnpucky/gentoo-base as gentoo-latex
LABEL maintainer="APN-Pucky"
RUN echo '>=media-libs/harfbuzz-4.3.0 icu -introspection' > /etc/portage/package.use/harfbuzz
RUN emerge -q latexmk biber app-text/texlive dev-texlive/texlive-latex dev-python/pygments && rm -rf /var/cache/distfiles/*

