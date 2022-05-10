# name the portage image
FROM apnpucky/gentoo-dev
MAINTAINER APN
RUN emerge -qv sci-physics/lhapdf sci-libs/gsl && rm -rf /var/cache/distfiles/*
RUN echo 'ACCEPT_KEYWORDS="~amd64"' >> /etc/make.conf
RUN mkdir /var/db/repos/hep-forge && printf "[hep-forge]\nlocation = /var/db/repos/hep-forge\nsync-type = git\nsync-uri = https://gitlab.com/APN-Pucky/gentoo-hep-forge.git" >> /etc/portage/repos.conf/hep-forge.conf && emaint -r hep-forge sync