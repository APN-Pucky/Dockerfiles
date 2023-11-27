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
#RUN emerge -q ~sci-physics/root-6.28.08  --autounmask=y --autounmask-write --autounmask-continue --autounmask-use=y && rm -rf /var/cache/distfiles/*
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

FROM apnpucky/gentoo-apn as gentoo-apn-unstable
RUN echo 'ACCEPT_KEYWORDS="~"' >> /etc/portage/make.conf
RUN emerge -e @world && emerge -e @preserved-rebuild && rm -rf /var/cache/distfiles/* && emerge --depclean

FROM apnpucky/gentoo-apn as gentoo-apn-live
RUN echo 'ACCEPT_KEYWORDS="~ **"' >> /etc/portage/make.conf
RUN emerge -e @world && emerge -e @preserved-rebuild && rm -rf /var/cache/distfiles/* && emerge --depclean


