# name the portage image
FROM gentoo/portage:latest as portage
FROM gentoo/stage3:latest
MAINTAINER APN
COPY --from=portage /var/db/repos/gentoo /var/db/repos/gentoo
RUN emerge -qv dev-vcs/git app-portage/repoman dev-util/pkgdev sys-devel/gcc virtual/fortran && rm -rf /var/cache/distfiles/*
RUN mkdir -p /etc/portage/repos.conf/