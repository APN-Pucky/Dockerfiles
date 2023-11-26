# gentoo-base
FROM gentoo/portage:latest as portage-base
FROM gentoo/stage3:latest as gentoo-base
LABEL maintainer="APN-Pucky"
ARG NICENESS=14
COPY --from=portage-base /var/db/repos/gentoo /var/db/repos/gentoo
RUN mkdir -p /etc/portage/repos.conf/ && cp /usr/share/portage/config/repos.conf /etc/portage/repos.conf/gentoo.conf
RUN echo "PORTAGE_NICENESS=\"${NICENESS}\"" >> /etc/portage/make.conf

FROM apnpucky/gentoo-base as gentoo-repo
RUN emerge -q eix dev-vcs/git && rm -rf /var/cache/distfiles/*
RUN rm -rf /var/db/repos/gentoo && mkdir /var/db/repos/gentoo && printf "[DEFAULT]\nmain-repo = gentoo\n\n[gentoo]\nlocation = /var/db/repos/gentoo\nsync-type = git\nsync-uri = https://github.com/gentoo-mirror/gentoo.git\nauto-sync = yes\nsync-git-verify-commit-signature = yes\nsync-openpgp-key-path = /usr/share/openpgp-keys/gentoo-release.asc" > /etc/portage/repos.conf/gentoo.conf && emerge --sync 
RUN eix-update

# gentoo-ebuild-ci
FROM apnpucky/gentoo-repo as gentoo-ebuild-ci
RUN emerge -q dev-util/pkgdev && rm -rf /var/cache/distfiles/*

# gentoo-ansible
FROM apnpucky/gentoo-repo as gentoo-ansible
RUN ACCEPT_KEYWORDS="* ~*" emerge -q app-admin/ansible-lint app-admin/ansible && rm -rf /var/cache/distfiles/*

# gentoo-dev
FROM apnpucky/gentoo-ebuild-ci as gentoo-rust
RUN emerge -q rust-bin && rm -rf /var/cache/distfiles/*

# gentoo-dev
FROM apnpucky/gentoo-rust as gentoo-c
RUN echo 'USE="-qt5 -X"' >> /etc/portage/make.conf
RUN emerge -q sys-devel/gcc && rm -rf /var/cache/distfiles/*

FROM apnpucky/gentoo-c as gentoo-c-tools
RUN echo 'USE="-qt5 -X"' >> /etc/portage/make.conf
RUN emerge -q dev-util/ccache virtual/fortran dev-util/cmake  && rm -rf /var/cache/distfiles/*

# gentoo-dev
FROM apnpucky/gentoo-c-tools as gentoo-llvm
RUN echo 'USE="-qt5 -X"' >> /etc/portage/make.conf
RUN emerge -q sys-devel/llvm  && rm -rf /var/cache/distfiles/*

# gentoo-dev
FROM apnpucky/gentoo-llvm as gentoo-boost
RUN emerge -q dev-libs/boost  && rm -rf /var/cache/distfiles/*

FROM apnpucky/gentoo-boost as gentoo-python
RUN echo 'PYTHON_TARGETS="python3_10 python3_11 python3_12"' >> /etc/portage/make.conf
RUN emerge -q dev-lang/python:3.10 dev-lang/python:3.11 && rm -rf /var/cache/distfiles/*

FROM apnpucky/gentoo-python as gentoo-lua
RUN echo 'LUA_TARGETS="lua5-1 lua5-3 lua5-4"' >> /etc/portage/make.conf
RUN emerge -q dev-lang/lua:5.1 dev-lang/lua:5.3  && rm -rf /var/cache/distfiles/*

FROM apnpucky/gentoo-lua as gentoo-perl
RUN emerge -q dev-lang/perl  && rm -rf /var/cache/distfiles/*

FROM apnpucky/gentoo-perl as gentoo-test
RUN ACCEPT_KEYWORDS="* ~*" emerge -q '>=app-portage/pkg-testing-tools-0.1.2' && rm -rf /var/cache/distfiles/*

FROM apnpucky/gentoo-test as gentoo-doc
# Documentation
RUN echo 'media-libs/harfbuzz icu -introspection' > /etc/portage/package.use/harfbuzz
RUN emerge -q virtual/latex-base && rm -rf /var/cache/distfiles/*
RUN emerge -q imagemagick ghostscript-gpl && rm -rf /var/cache/distfiles/*
RUN emerge -q --changed-use --deep --keep-going @world && rm -rf /var/cache/distfiles/*
RUN USE="lapack" emerge -q --autounmask=y --autounmask-write --autounmask-continue --autounmask-use=y dev-python/{matplotlib,numpy} && rm -rf /var/cache/distfiles/*
RUN emerge -q dev-python/pip && rm -rf /var/cache/distfiles/*

FROM apnpucky/gentoo-doc as gentoo-dev
# Config
ENV PKGDIR="/var/cache/binpkgs"
ENV CCACHE_DIR="/var/cache/ccache"
ENV CCACHE_SIZE="1G"
RUN install -m 0750 -o portage -g portage -d /etc/portage/env /etc/portage/package.{accept_keywords,env,unmask,use}
RUN install -m 0750 -o portage -g portage -d "${PKGDIR}" 
RUN install -m 0750 -o portage -g portage -d "${CCACHE_DIR}" 
RUN install -m 0775 -o portage -g portage -d "${CCACHE_DIR}"/{0,1,2,3,4,5,6,7,8,9,a,b,c,d,e,f}
RUN install -m 0775 -o portage -g portage -d "${CCACHE_DIR}"/{0,1,2,3,4,5,6,7,8,9,a,b,c,d,e,f}/{0,1,2,3,4,5,6,7,8,9,a,b,c,d,e,f}
# test.conf from https://wiki.gentoo.org/wiki/Package_testing
RUN printf 'COMMON_FLAGS="-O2 -pipe -frecord-gcc-switches"\nCFLAGS="${COMMON_FLAGS}"\nCXXFLAGS="${COMMON_FLAGS}"\nFCFLAGS="${COMMON_FLAGS}"\nFFLAGS="${COMMON_FLAGS}"\nLDFLAGS="${LDFLAGS} -Wl,--defsym=__gentoo_check_ldflags__=0"\nFEATURES="collision-protect ipc-sandbox network-sandbox sandbox split-log split-elog strict test userfetch userpriv usersandbox"\nFEATURES="${FEATURES} -parallel-install"\nPORTAGE_ELOG_CLASSES="log warn error qa"\nPORTAGE_ELOG_SYSTEM="echo save"\nDISTUTILS_STRICT_ENTRY_POINTS=1\nALLOW_TEST="network"\nIWDT_ALL=y\nQA_CMP=y\nQA_CMP_ARGS="-xS"\nQA_SED=y\nQA_VDB=y' > /etc/portage/env/test.conf
RUN echo 'ACCEPT_LICENSE="* -@EULA"' >> /etc/portage/make.conf
RUN emerge --depclean

# gentoo-apn
FROM apnpucky/gentoo-dev as gentoo-apn
RUN mkdir /var/db/repos/apn && printf "[apn]\nlocation = /var/db/repos/apn\nsync-type = git\nsync-uri = https://gitlab.com/APN-Pucky/gentoo-apn.git" >> /etc/portage/repos.conf/apn.conf && emaint -r apn sync
RUN echo '*/*::apn * ~*' > /etc/portage/package.accept_keywords/apn
RUN echo '*/*::guru * ~*' > /etc/portage/package.accept_keywords/guru
RUN echo '*/*::jjakob * ~*' > /etc/portage/package.accept_keywords/jjakob
RUN emerge app-eselect/eselect-repository
RUN eselect repository enable guru
RUN eselect repository enable jjakob
RUN emerge --sync

# gentoo-hep-forge
FROM apnpucky/gentoo-dev as gentoo-hep-forge
# USE="X" for mesa for opengl for fastjet
RUN echo 'USE="X -qt5"' >> /etc/portage/make.conf
RUN mkdir /var/db/repos/hep-forge && printf "[hep-forge]\nlocation = /var/db/repos/hep-forge\nsync-type = git\nsync-uri = https://gitlab.com/APN-Pucky/gentoo-hep-forge.git" >> /etc/portage/repos.conf/hep-forge.conf
RUN echo '*/*::hep-forge * ~*' > /etc/portage/package.accept_keywords/hepforge
RUN echo '*/*::science * ~*' > /etc/portage/package.accept_keywords/science
RUN emerge app-eselect/eselect-repository
RUN eselect repository enable science
RUN emerge --sync
#TODO better use --autounmask=y --autounmask-write --autounmask-continue --autounmask-use=y
RUN echo '~sci-physics/root-6.28.08 **' > /etc/portage/package.accept_keywords/root 
RUN echo '~sci-physics/root-6.28.08 -X -qt5 -asimage -opengl -jupyter' > /etc/portage/package.use/root 
RUN emerge -q ~sci-physics/root-6.28.08  --autounmask=y --autounmask-write --autounmask-continue --autounmask-use=y && rm -rf /var/cache/distfiles/*
RUN echo 'sci-physics/hepmc gev' >> /etc/portage/package.use/zz-autoaccept
RUN echo 'sci-physics/siscone ~*' >> /etc/portage/package.accept_keywords/zz-autoaccept
RUN echo 'dev-python/keyrings-alt ~*' >> /etc/portage/package.accept_keywords/zz-autoaccept
RUN echo 'dev-python/jaraco-classes ~*' >> /etc/portage/package.accept_keywords/zz-autoaccept

FROM apnpucky/gentoo-hep-forge as gentoo-hep-forge-unstable
RUN echo 'ACCEPT_KEYWORDS="~"' >> /etc/portage/make.conf
RUN emerge -e @world && emerge -e @preserved-rebuild && rm -rf /var/cache/distfiles/* && emerge --depclean

FROM apnpucky/gentoo-hep-forge as gentoo-hep-forge-live
RUN echo 'ACCEPT_KEYWORDS="~ **"' >> /etc/portage/make.conf
RUN emerge -e @world && emerge -e @preserved-rebuild && rm -rf /var/cache/distfiles/* && emerge --depclean

FROM apnpucky/gentoo-apn as gentoo-apn-unstable
RUN echo 'ACCEPT_KEYWORDS="~"' >> /etc/portage/make.conf
RUN emerge -e @world && emerge -e @preserved-rebuild && rm -rf /var/cache/distfiles/* && emerge --depclean

FROM apnpucky/gentoo-apn as gentoo-apn-live
RUN echo 'ACCEPT_KEYWORDS="~ **"' >> /etc/portage/make.conf
RUN emerge -e @world && emerge -e @preserved-rebuild && rm -rf /var/cache/distfiles/* && emerge --depclean

# gentoo-latex
FROM apnpucky/gentoo-base as gentoo-latex
LABEL maintainer="APN-Pucky"
RUN echo '>=media-libs/harfbuzz-4.3.0 icu -introspection' > /etc/portage/package.use/harfbuzz
RUN emerge -q latexmk biber app-text/texlive dev-texlive/texlive-latex dev-python/pygments && rm -rf /var/cache/distfiles/*

# gentoo-prefix
FROM apnpucky/gentoo-base as prefix
LABEL maintainer="APN-Pucky"
RUN wget https://gitweb.gentoo.org/repo/proj/prefix.git/plain/scripts/bootstrap-prefix.sh && chmod +x bootstrap-prefix.sh && sed -i 's/UID/NOUID/g' bootstrap-prefix.sh && ./bootstrap-prefix.sh /gentoo-prefix noninteractive
# set entry point to start gentoo prefix
ENTRYPOINT ["/gentoo-prefix/startprefix"]
