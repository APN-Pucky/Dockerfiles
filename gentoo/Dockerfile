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
RUN emerge -q app-admin/ansible-lint app-admin/ansible && rm -rf /var/cache/distfiles/*

# gentoo-dev
FROM apnpucky/gentoo-ebuild-ci as gentoo-dev
RUN emerge -q rust && rm -rf /var/cache/distfiles/*
RUN echo 'USE="-qt5 -X"' >> /etc/portage/make.conf
RUN echo 'LUA_TARGETS="lua5-1 lua5-3 lua5-4"' >> /etc/portage/make.conf
RUN echo 'PYTHON_TARGETS="python3_8 python3_9 python3_10 python3_11"' >> /etc/portage/make.conf
RUN emerge -q sys-devel/gcc sys-devel/llvm dev-util/ccache virtual/fortran dev-util/cmake dev-libs/boost app-portage/pkg-testing-tools dev-lang/perl dev-lang/python:3.8 dev-lang/python:3.9 dev-lang/python:3.10 dev-lang/lua:5.1 dev-lang/lua:5.3  && rm -rf /var/cache/distfiles/*
RUN ACCEPT_KEYWORDS="~amd64" emerge -q '>=app-portage/pkg-testing-tools-0.1.2' && rm -rf /var/cache/distfiles/*
# Documentation
RUN echo 'media-libs/harfbuzz icu -introspection' > /etc/portage/package.use/harfbuzz
RUN emerge -q virtual/latex-base && rm -rf /var/cache/distfiles/*
RUN emerge -q imagemagick ghostscript-gpl && rm -rf /var/cache/distfiles/*
# python3_11
RUN ACCEPT_KEYWORDS="~amd64" emerge -q dev-lang/python:3.11 && rm -rf /var/cache/distfiles/*
RUN emerge -q --changed-use --deep --keep-going @world && rm -rf /var/cache/distfiles/*
RUN USE="lapack" emerge -q --autounmask=y --autounmask-write --autounmask-continue --autounmask-use=y dev-python/{matplotlib,numpy} && rm -rf /var/cache/distfiles/*
RUN emerge -q dev-python/pip && rm -rf /var/cache/distfiles/*
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
RUN echo '*/*::apn ~amd64' > /etc/portage/package.accept_keywords/apn

FROM apnpucky/gentoo-apn as gentoo-apn-unstable
RUN echo 'ACCEPT_KEYWORDS="~"' >> /etc/portage/make.conf
RUN emerge -e @world && emerge -e @preserved-rebuild && rm -rf /var/cache/distfiles/* && emerge --depclean

FROM apnpucky/gentoo-apn as gentoo-apn-live
RUN echo 'ACCEPT_KEYWORDS="~ **"' >> /etc/portage/make.conf
RUN emerge -e @world && emerge -e @preserved-rebuild && rm -rf /var/cache/distfiles/* && emerge --depclean

# gentoo-hep-forge
FROM apnpucky/gentoo-dev as gentoo-hep-forge
RUN echo '>=media-libs/libafterimage-1.20-r6 png gif tiff jpeg' >> /etc/portage/package.use/root
# USE="X" for mesa for opengl for fastjet
RUN echo 'USE="X"' >> /etc/portage/make.conf
RUN ACCEPT_KEYWORDS="~amd64" emerge -q sci-physics/root  --autounmask=y --autounmask-write --autounmask-continue --autounmask-use=y && rm -rf /var/cache/distfiles/*
RUN mkdir /var/db/repos/hep-forge && printf "[hep-forge]\nlocation = /var/db/repos/hep-forge\nsync-type = git\nsync-uri = https://gitlab.com/APN-Pucky/gentoo-hep-forge.git" >> /etc/portage/repos.conf/hep-forge.conf && emaint -r hep-forge sync
RUN echo '*/*::hep-forge ~amd64' > /etc/portage/package.accept_keywords/hepforge
#TODO better use --autounmask=y --autounmask-write --autounmask-continue --autounmask-use=y
RUN echo 'sci-physics/hepmc gev' >> /etc/portage/package.use/zz-autoaccept
RUN echo 'sci-physics/siscone ~amd64' >> /etc/portage/package.accept_keywords/zz-autoaccept
RUN echo 'dev-python/keyrings-alt ~amd64' >> /etc/portage/package.accept_keywords/zz-autoaccept
RUN echo 'dev-python/jaraco-classes ~amd64' >> /etc/portage/package.accept_keywords/zz-autoaccept

FROM apnpucky/gentoo-hep-forge as gentoo-hep-forge-unstable
RUN echo 'ACCEPT_KEYWORDS="~"' >> /etc/portage/make.conf
RUN emerge -e @world && emerge -e @preserved-rebuild && rm -rf /var/cache/distfiles/* && emerge --depclean

FROM apnpucky/gentoo-hep-forge as gentoo-hep-forge-live
RUN echo 'ACCEPT_KEYWORDS="~ **"' >> /etc/portage/make.conf
RUN emerge -e @world && emerge -e @preserved-rebuild && rm -rf /var/cache/distfiles/* && emerge --depclean


FROM apnpucky/gentoo-hep-forge as gentoo-lhapdf
LABEL maintainer="APN-Pucky"
ARG LHAPDF_VERSION=6.5.4
RUN emaint -r hep-forge sync
# stabel lhapdf only support 3.8
RUN emerge -q eselect-python && eselect python set python3.10 && rm -rf /var/cache/distfiles/*
#RUN echo 'sci-physics/lhapdf ~amd64' >> /etc/portage/package.accept_keywords/zz-autoaccept
RUN USE="python" emerge -q ~sci-physics/lhapdf-${LHAPDF_VERSION} && rm -rf /var/cache/distfiles/*

FROM apnpucky/gentoo-hep-forge as gentoo-lhapdf-looptools
LABEL maintainer="APN-Pucky"
ARG LOOPTOOLS_VERSION=2.15
ARG LHAPDF_VERSION=6.5.4
RUN emaint -r hep-forge sync
# stabel lhapdf only support 3.8
RUN emerge -q eselect-python && eselect python set python3.10 && rm -rf /var/cache/distfiles/*
#RUN echo 'sci-physics/lhapdf ~amd64' >> /etc/portage/package.accept_keywords/zz-autoaccept
RUN echo 'sci-physics/looptools **' >> /etc/portage/package.accept_keywords/zz-autoaccept
RUN USE="python" emerge -q ~sci-physics/lhapdf-${LHAPDF_VERSION} ~sci-physics/looptools-${LOOPTOOLS_VERSION} && rm -rf /var/cache/distfiles/*



FROM apnpucky/gentoo-hep-forge as gentoo-rivet-hepmc3
LABEL maintainer="APN-Pucky"
ARG RIVET_VERSION=3.1.6-r1
ARG HEPMC_VERSION=3.2.5
RUN emaint -r hep-forge sync
RUN USE="hepmc3 -hepmc2 python" emerge -q =sci-physics/rivet-${RIVET_VERSION} =sci-physics/hepmc-${HEPMC_VERSION} && rm -rf /var/cache/distfiles/*


# gentoo-lhapdf-looptools
FROM apnpucky/gentoo-lhapdf-looptools as gentoo-rivet3-pythia8-hepmc3
LABEL maintainer="APN-Pucky"
ARG RIVET_VERSION=3.1.6-r1
ARG PYTHIA_VERSION=8.3.07-r4
ARG LHAPDF_VERSION=6.3.0-r1
RUN emaint -r hep-forge sync
#RUN echo 'sci-physics/lhapdf ~amd64' >> /etc/portage/package.accept_keywords/zz-autoaccept
#RUN echo 'sci-physics/looptools ~amd64' >> /etc/portage/package.accept_keywords/zz-autoaccept
RUN USE="hepmc3 -hepmc2 python lhapdf" emerge -q =sci-physics/rivet-${RIVET_VERSION} =sci-physics/pythia-${PYTHIA_VERSION} =sci-physics/hepmc-${HEPMC_VERSION} && rm -rf /var/cache/distfiles/*

# gentoo-lhapdf-looptools
FROM apnpucky/gentoo-lhapdf-looptools as gentoo-rivet3-pythia8-hepmc2
LABEL maintainer="APN-Pucky"
ARG RIVET_VERSION=3.1.6-r1
ARG PYTHIA_VERSION=8.3.07-r4
ARG HEPMC_VERSION=2.06.11
RUN emaint -r hep-forge sync
#RUN echo 'sci-physics/lhapdf ~amd64' >> /etc/portage/package.accept_keywords/zz-autoaccept
#RUN echo 'sci-physics/looptools ~amd64' >> /etc/portage/package.accept_keywords/zz-autoaccept
RUN USE="-hepmc3 hepmc2 python lhapdf" emerge -q =sci-physics/rivet-${RIVET_VERSION} =sci-physics/pythia-${PYTHIA_VERSION} =sci-physics/hepmc-${HEPMC_VERSION} && rm -rf /var/cache/distfiles/*


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