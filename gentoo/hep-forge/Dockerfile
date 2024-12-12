FROM apnpucky/gentoo-dev as gentoo-hep-forge
# USE="X" for mesa for opengl for fastjet
RUN echo 'USE="X -qt5"' >> /etc/portage/make.conf
RUN mkdir -p /var/db/repos/hep-forge && printf "[hep-forge]\nlocation = /var/db/repos/hep-forge\nsync-type = git\nsync-uri = https://gitlab.com/APN-Pucky/gentoo-hep-forge.git" > /etc/portage/repos.conf/hep-forge.conf
RUN echo '*/*::hep-forge * ~*' > /etc/portage/package.accept_keywords/hepforge
RUN echo '*/*::science * ~*' > /etc/portage/package.accept_keywords/science
RUN emerge app-eselect/eselect-repository
RUN eselect repository enable science
RUN emerge --sync
#TODO better use --autounmask=y --autounmask-write --autounmask-continue --autounmask-use=y
RUN echo '~sci-physics/root-6.28.08 **' > /etc/portage/package.accept_keywords/root 
RUN echo '~sci-physics/root-6.28.08 -X -qt5 -asimage -opengl -jupyter' > /etc/portage/package.use/root 
RUN echo 'sci-physics/hepmc gev' >> /etc/portage/package.use/zz-autoaccept
RUN echo 'sci-physics/siscone ~*' >> /etc/portage/package.accept_keywords/zz-autoaccept
RUN echo 'dev-python/keyrings-alt ~*' >> /etc/portage/package.accept_keywords/zz-autoaccept
RUN echo 'dev-python/jaraco-classes ~*' >> /etc/portage/package.accept_keywords/zz-autoaccept
RUN emerge -q sci-physics/root  --autounmask=y --autounmask-write --autounmask-continue --autounmask-use=y && rm -rf /var/cache/distfiles/*
RUN emerge -q dev-python/scipy  --autounmask=y --autounmask-write --autounmask-continue --autounmask-use=y && rm -rf /var/cache/distfiles/*