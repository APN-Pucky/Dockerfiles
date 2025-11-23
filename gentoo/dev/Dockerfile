FROM apnpucky/gentoo-test AS gentoo-dev
#FROM apnpucky/gentoo-doc AS gentoo-dev
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
# Feed portagefilelist.org
RUN echo 'sys-apps/util-linux caps' > /etc/portage/package.use/util-linux
RUN USE="-network-cron" emerge pfl
RUN pfl
RUN emerge --depclean