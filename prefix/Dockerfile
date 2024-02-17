FROM debian as debian-gentoo-prefix
RUN apt-get update && apt-get upgrade -y && apt-get install -y build-essential wget
RUN wget https://gitweb.gentoo.org/repo/proj/prefix.git/plain/scripts/bootstrap-prefix.sh && chmod +x bootstrap-prefix.sh && sed -i 's/UID/NOUID/g' bootstrap-prefix.sh && ./bootstrap-prefix.sh /gentoo-prefix noninteractive
# Forward the exit code through entrypoint
RUN sed -i '$ d' /gentoo-prefix/startprefix && echo 'exit $?' >> /gentoo-prefix/startprefix
# set entry point to start gentoo prefix
ENTRYPOINT ["/gentoo-prefix/startprefix"]

FROM apnpucky/debian-gentoo-prefix as debian-gentoo-prefix-ci
RUN ln -sf /bin/bash /bin/sh
RUN echo "emerge -q eselect-repository && rm -rf /gentoo-prefix/var/cache/distfiles/*" | /gentoo-prefix/startprefix 
RUN echo "emerge -q dev-vcs/git && rm -rf /gentoo-prefix/var/cache/distfiles/*" | /gentoo-prefix/startprefix 
RUN rm -rf /gentoo-prefix/var/db/repos/gentoo && mkdir -p /gentoo-prefix/var/db/repos/gentoo /gentoo-prefix/etc/portage/repos.conf/ && printf "[DEFAULT]\nmain-repo = gentoo\n\n[gentoo]\nlocation = /gentoo-prefix/var/db/repos/gentoo\nsync-type = git\nsync-uri = https://github.com/gentoo-mirror/gentoo.git\nauto-sync = yes\nsync-git-verify-commit-signature = yes\nsync-openpgp-key-path = /gentoo-prefix/usr/share/openpgp-keys/gentoo-release.asc" > /gentoo-prefix/etc/portage/repos.conf/gentoo.conf
RUN echo "emerge --sync" | /gentoo-prefix/startprefix
RUN cat /gentoo-prefix/etc/portage/make.conf/* > /gentoo-prefix/etc/portage/make.conf.tmp && rm -r /gentoo-prefix/etc/portage/make.conf && mv /gentoo-prefix/etc/portage/make.conf.tmp /gentoo-prefix/etc/portage/make.conf
RUN mkdir -p /gentoo-prefix/etc/portage/profile/ && echo "-python_targets_python3_12" >> /gentoo-prefix/etc/portage/profile/use.mask
RUN install -m 0750 -d /gentoo-prefix/etc/portage/env /gentoo-prefix/etc/portage/package.{accept_keywords,env,unmask,use}
RUN mkdir -p /gentoo-prefix/var/db/repos/apn && printf "[apn]\nlocation = /gentoo-prefix/var/db/repos/apn\nsync-type = git\nsync-uri = https://gitlab.com/APN-Pucky/gentoo-apn.git" > /gentoo-prefix/etc/portage/repos.conf/apn.conf && echo "emaint -r apn sync" | /gentoo-prefix/startprefix
RUN echo '*/*::apn * ~*' > /gentoo-prefix/etc/portage/package.accept_keywords/apn
RUN echo '*/*::guru * ~*' > /gentoo-prefix/etc/portage/package.accept_keywords/guru
RUN echo '*/*::jjakob * ~*' > /gentoo-prefix/etc/portage/package.accept_keywords/jjakob
RUN echo "eselect repository enable guru" | /gentoo-prefix/startprefix
RUN echo "eselect repository enable jjakob" | /gentoo-prefix/startprefix
RUN echo "emerge -q dev-util/pkgdev && rm -rf /gentoo-prefix/var/cache/distfiles/*" | /gentoo-prefix/startprefix 
# install langs
RUN echo 'USE="-qt5 -X"' >> /gentoo-prefix/etc/portage/make.conf
RUN echo 'PYTHON_TARGETS="python3_10 python3_11 python3_12"' >> /gentoo-prefix/etc/portage/make.conf
RUN echo "emerge -q dev-lang/python:3.10 dev-lang/python:3.11 dev-lang/python:3.12 && rm -rf /gentoo-prefix/var/cache/distfiles/*" | /gentoo-prefix/startprefix
RUN echo 'LUA_TARGETS="lua5-1 lua5-3 lua5-4"' >> /gentoo-prefix/etc/portage/make.conf
RUN echo "emerge -q dev-lang/lua:5.1 dev-lang/lua:5.3  && rm -rf /gentoo-prefix/var/cache/distfiles/*" | /gentoo-prefix/startprefix
RUN echo "emerge -q dev-lang/perl  && rm -rf /gentoo-prefix/var/cache/distfiles/*" | /gentoo-prefix/startprefix
# install boost
RUN echo "emerge -q dev-libs/boost  && rm -rf /gentoo-prefix/var/cache/distfiles/*" | /gentoo-prefix/startprefix
# most used pythons
RUN echo 'USE="lapack" emerge -q --autounmask=y --autounmask-write --autounmask-continue --autounmask-use=y dev-python/{matplotlib,numpy} && rm -rf /var/cache/distfiles/*' | /gentoo-prefix/startprefix
RUN echo "emerge -q '>=app-portage/pkg-testing-tools-0.2.1' && rm -rf /gentoo-prefix/var/cache/distfiles/*" | /gentoo-prefix/startprefix
RUN echo "emerge -q dev-util/ccache virtual/fortran dev-util/cmake  && rm -rf /gentoo-prefix/var/cache/distfiles/*" | /gentoo-prefix/startprefix
ENV PKGDIR="/gentoo-prefix/var/cache/binpkgs"
ENV CCACHE_DIR="/gentoo-prefix/var/cache/ccache"
ENV CCACHE_SIZE="1G"
RUN echo "PKGDIR=${PKGDIR}" >> /gentoo-prefix/etc/portage/make.conf
RUN echo "CCACHE_DIR=${CCACHE_DIR}" >> /gentoo-prefix/etc/portage/make.conf
RUN echo "CCACHE_SIZE=${CCACHE_SIZE}" >> /gentoo-prefix/etc/portage/make.conf
RUN install -m 0750 -d /gentoo-prefix/etc/portage/env /gentoo-prefix/etc/portage/package.{accept_keywords,env,unmask,use}
RUN install -m 0750 -d "${PKGDIR}" 
RUN install -m 0750 -d "${CCACHE_DIR}" 
RUN install -m 0775 -d "${CCACHE_DIR}"/{0,1,2,3,4,5,6,7,8,9,a,b,c,d,e,f}
RUN install -m 0775 -d "${CCACHE_DIR}"/{0,1,2,3,4,5,6,7,8,9,a,b,c,d,e,f}/{0,1,2,3,4,5,6,7,8,9,a,b,c,d,e,f}
# test.conf from https://wiki.gentoo.org/wiki/Package_testing
RUN printf 'COMMON_FLAGS="-O2 -pipe -frecord-gcc-switches"\nCFLAGS="${COMMON_FLAGS}"\nCXXFLAGS="${COMMON_FLAGS}"\nFCFLAGS="${COMMON_FLAGS}"\nFFLAGS="${COMMON_FLAGS}"\nLDFLAGS="${LDFLAGS} -Wl,--defsym=__gentoo_check_ldflags__=0"\nFEATURES="collision-protect ipc-sandbox network-sandbox sandbox split-log split-elog strict test userfetch userpriv usersandbox"\nFEATURES="${FEATURES} -parallel-install"\nPORTAGE_ELOG_CLASSES="log warn error qa"\nPORTAGE_ELOG_SYSTEM="echo save"\nDISTUTILS_STRICT_ENTRY_POINTS=1\nALLOW_TEST="network"\nIWDT_ALL=y\nQA_CMP=y\nQA_CMP_ARGS="-xS"\nQA_SED=y\nQA_VDB=y' > /gentoo-prefix/etc/portage/env/test.conf
RUN echo 'ACCEPT_LICENSE="* -@EULA"' >> /gentoo-prefix/etc/portage/make.conf
#RUN echo 'USE="-network-cron" emerge pfl' | /gentoo-prefix/startprefix
#RUN echo 'pfl' | /gentoo-prefix/startprefix
RUN echo 'emerge --depclean' | /gentoo-prefix/startprefix
# Remoe lastlien from startprefix and append exit $?
RUN sed -i '$ d' /gentoo-prefix/startprefix && echo 'exit $?' >> /gentoo-prefix/startprefix
    


FROM gentoo/stage3:latest as gentoo-gentoo-prefix
RUN wget https://gitweb.gentoo.org/repo/proj/prefix.git/plain/scripts/bootstrap-prefix.sh && chmod +x bootstrap-prefix.sh && sed -i 's/UID/NOUID/g' bootstrap-prefix.sh && ./bootstrap-prefix.sh /gentoo-prefix noninteractive
# set entry point to start gentoo prefix
ENTRYPOINT ["/gentoo-prefix/startprefix"]

