# gentoo-ebuild-ci
FROM gentoo/portage:latest as portage
FROM gentoo/stage3:latest as gentoo-ebuild-ci
LABEL maintainer="APN-Pucky"
COPY --from=portage /var/db/repos/gentoo /var/db/repos/gentoo
RUN emerge -qv dev-vcs/git app-portage/repoman dev-util/pkgdev && rm -rf /var/cache/distfiles/*
RUN mkdir -p /etc/portage/repos.conf/ && cp /usr/share/portage/config/repos.conf /etc/portage/repos.conf/gentoo.conf

# gentoo-dev
FROM apnpucky/gentoo-ebuild-ci as gentoo-dev
RUN emerge -qv sys-devel/gcc virtual/fortran dev-util/cmake && rm -rf /var/cache/distfiles/*
RUN emerge -qv dev-libs/boost && rm -rf /var/cache/distfiles/*

# gentoo-hep-forge
FROM apnpucky/gentoo-dev as gentoo-hep-forge
RUN emerge -qv sci-physics/lhapdf sci-libs/gsl && rm -rf /var/cache/distfiles/*
RUN echo 'ACCEPT_KEYWORDS="~amd64"' >> /etc/portage/make.conf
RUN mkdir /var/db/repos/hep-forge && printf "[hep-forge]\nlocation = /var/db/repos/hep-forge\nsync-type = git\nsync-uri = https://gitlab.com/APN-Pucky/gentoo-hep-forge.git" >> /etc/portage/repos.conf/hep-forge.conf && emaint -r hep-forge sync