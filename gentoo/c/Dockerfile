# gentoo-dev
FROM apnpucky/gentoo-rust as gentoo-c
RUN echo 'USE="-qt5 -X"' >> /etc/portage/make.conf
RUN emerge -q sys-devel/gcc && rm -rf /var/cache/distfiles/*