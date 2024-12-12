
FROM apnpucky/gentoo-c as gentoo-c-tools
RUN echo 'USE="-qt5 -X"' >> /etc/portage/make.conf
RUN emerge -q dev-util/ccache virtual/fortran dev-build/cmake  && rm -rf /var/cache/distfiles/*