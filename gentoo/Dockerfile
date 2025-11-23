# gentoo-base
FROM gentoo/portage:latest as portage-base
FROM gentoo/stage3:latest as gentoo-base
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

FROM apnpucky/gentoo-base as gentoo-repo
RUN emerge -q eix dev-vcs/git && rm -rf /var/cache/distfiles/*
RUN rm -rf /var/db/repos/gentoo && mkdir -p /var/db/repos/gentoo && printf "[DEFAULT]\nmain-repo = gentoo\n\n[gentoo]\nlocation = /var/db/repos/gentoo\nsync-type = git\nsync-uri = https://github.com/gentoo-mirror/gentoo.git\nauto-sync = yes\nsync-git-verify-commit-signature = yes\nsync-openpgp-key-path = /usr/share/openpgp-keys/gentoo-release.asc" > /etc/portage/repos.conf/gentoo.conf && emerge --sync 
RUN eix-update
RUN install -m 0750 -o portage -g portage -d /etc/portage/env /etc/portage/package.{accept_keywords,env,unmask,use}

# gentoo-ebuild-ci
FROM apnpucky/gentoo-repo as gentoo-ebuild-ci
RUN emerge -q dev-util/pkgdev && rm -rf /var/cache/distfiles/*

# gentoo-rust
FROM apnpucky/gentoo-ebuild-ci as gentoo-rust
RUN emerge -q rust-bin && rm -rf /var/cache/distfiles/*

# gentoo-dev
FROM apnpucky/gentoo-rust as gentoo-c
RUN echo 'USE="-qt5 -X"' >> /etc/portage/make.conf
RUN emerge -q sys-devel/gcc && rm -rf /var/cache/distfiles/*

FROM apnpucky/gentoo-c as gentoo-c-tools
RUN echo 'USE="-qt5 -X"' >> /etc/portage/make.conf
RUN emerge -q dev-util/ccache virtual/fortran dev-build/cmake  && rm -rf /var/cache/distfiles/*

# gentoo-dev
FROM apnpucky/gentoo-c-tools as gentoo-llvm
RUN echo 'USE="-qt5 -X"' >> /etc/portage/make.conf
RUN emerge -q llvm-core/llvm  && rm -rf /var/cache/distfiles/*

# gentoo-dev
FROM apnpucky/gentoo-llvm as gentoo-boost
RUN emerge -q dev-libs/boost  && rm -rf /var/cache/distfiles/*

FROM apnpucky/gentoo-boost as gentoo-python
RUN echo 'PYTHON_TARGETS="python3_11 python3_12 python3_13"' >> /etc/portage/make.conf
RUN mkdir -p /etc/portage/profile/ && echo "-python_targets_python3_13" >> /etc/portage/profile/use.mask
RUN emerge -q dev-lang/python:3.11 dev-lang/python:3.12 dev-lang/python:3.13 && rm -rf /var/cache/distfiles/*

FROM apnpucky/gentoo-python as gentoo-lua
RUN echo 'LUA_TARGETS="lua5-1 lua5-3 lua5-4"' >> /etc/portage/make.conf
RUN emerge -q dev-lang/lua:5.1 dev-lang/lua:5.3  && rm -rf /var/cache/distfiles/*

FROM apnpucky/gentoo-lua as gentoo-perl
RUN emerge -q dev-lang/perl  && rm -rf /var/cache/distfiles/*

#gentoo-test
FROM apnpucky/gentoo-perl as gentoo-test
RUN mkdir /var/db/repos/apn && printf "[apn]\nlocation = /var/db/repos/apn\nsync-type = git\nsync-uri = https://gitlab.com/APN-Pucky/gentoo-apn.git" > /etc/portage/repos.conf/apn.conf && emaint -r apn sync
RUN echo '*/*::apn * ~*' > /etc/portage/package.accept_keywords/apn
RUN echo '*/*::guru * ~*' > /etc/portage/package.accept_keywords/guru
RUN emerge app-eselect/eselect-repository
RUN eselect repository enable guru
RUN emerge --sync
RUN ACCEPT_KEYWORDS="* ~*" emerge -q '>=app-portage/pkg-testing-tools-0.2.1' && rm -rf /var/cache/distfiles/*

## gentoo-hep-forge
#FROM apnpucky/gentoo-dev as gentoo-hep-forge
## USE="X" for mesa for opengl for fastjet
#RUN echo 'USE="X -qt5"' >> /etc/portage/make.conf
#RUN mkdir -p /var/db/repos/hep-forge && printf "[hep-forge]\nlocation = /var/db/repos/hep-forge\nsync-type = git\nsync-uri = https://gitlab.com/APN-Pucky/gentoo-hep-forge.git" > /etc/portage/repos.conf/hep-forge.conf
#RUN echo '*/*::hep-forge * ~*' > /etc/portage/package.accept_keywords/hepforge
#RUN echo '*/*::science * ~*' > /etc/portage/package.accept_keywords/science
#RUN emerge app-eselect/eselect-repository
#RUN eselect repository enable science
#RUN emerge --sync
##TODO better use --autounmask=y --autounmask-write --autounmask-continue --autounmask-use=y
#RUN echo '~sci-physics/root-6.28.08 **' > /etc/portage/package.accept_keywords/root 
#RUN echo '~sci-physics/root-6.28.08 -X -qt5 -asimage -opengl -jupyter' > /etc/portage/package.use/root 
#RUN echo 'sci-physics/hepmc gev' >> /etc/portage/package.use/zz-autoaccept
#RUN echo 'sci-physics/siscone ~*' >> /etc/portage/package.accept_keywords/zz-autoaccept
#RUN echo 'dev-python/keyrings-alt ~*' >> /etc/portage/package.accept_keywords/zz-autoaccept
#RUN echo 'dev-python/jaraco-classes ~*' >> /etc/portage/package.accept_keywords/zz-autoaccept
#RUN emerge -q sci-physics/root  --autounmask=y --autounmask-write --autounmask-continue --autounmask-use=y && rm -rf /var/cache/distfiles/*
#RUN emerge -q dev-python/scipy  --autounmask=y --autounmask-write --autounmask-continue --autounmask-use=y && rm -rf /var/cache/distfiles/*
#
#FROM apnpucky/gentoo-hep-forge as gentoo-hep-forge-unstable
#RUN echo 'ACCEPT_KEYWORDS="~"' >> /etc/portage/make.conf
#RUN emerge -e @world && emerge -e @preserved-rebuild && rm -rf /var/cache/distfiles/* && emerge --depclean
#
#FROM apnpucky/gentoo-hep-forge as gentoo-hep-forge-live
#RUN echo 'ACCEPT_KEYWORDS="~ **"' >> /etc/portage/make.conf
#RUN emerge -e @world && emerge -e @preserved-rebuild && rm -rf /var/cache/distfiles/* && emerge --depclean
#
#FROM apnpucky/gentoo-apn as gentoo-apn-unstable
#RUN echo 'ACCEPT_KEYWORDS="~"' >> /etc/portage/make.conf
#RUN emerge -e @world && emerge -e @preserved-rebuild && rm -rf /var/cache/distfiles/* && emerge --depclean
#
#FROM apnpucky/gentoo-apn as gentoo-apn-live
#RUN echo 'ACCEPT_KEYWORDS="~ **"' >> /etc/portage/make.conf
#RUN emerge -e @world && emerge -e @preserved-rebuild && rm -rf /var/cache/distfiles/* && emerge --depclean
#
## gentoo-latex
#FROM apnpucky/gentoo-base as gentoo-latex
#LABEL maintainer="APN-Pucky"
#RUN echo '>=media-libs/harfbuzz-4.3.0 icu -introspection' > /etc/portage/package.use/harfbuzz
#RUN emerge -q latexmk biber app-text/texlive dev-texlive/texlive-latex dev-python/pygments && rm -rf /var/cache/distfiles/*

