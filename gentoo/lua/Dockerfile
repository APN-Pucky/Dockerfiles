FROM apnpucky/gentoo-python as gentoo-lua
RUN echo 'LUA_TARGETS="lua5-1 lua5-3 lua5-4"' >> /etc/portage/make.conf
RUN emerge -q dev-lang/lua:5.1 dev-lang/lua:5.3  && rm -rf /var/cache/distfiles/*