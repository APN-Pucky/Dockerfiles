FROM apnpucky/gentoo-c-tools as gentoo-llvm
RUN echo 'USE="-qt5 -X"' >> /etc/portage/make.conf
RUN emerge -q sys-devel/llvm  && rm -rf /var/cache/distfiles/*