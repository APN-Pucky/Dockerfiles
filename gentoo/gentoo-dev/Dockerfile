# name the portage image
FROM apnpucky/gentoo-ebuild-ci
MAINTAINER APN
RUN emerge -qv sys-devel/gcc virtual/fortran dev-util/cmake && rm -rf /var/cache/distfiles/*
RUN mkdir -p /etc/portage/repos.conf/