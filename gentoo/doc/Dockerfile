FROM apnpucky/gentoo-test AS gentoo-doc
# Documentation
RUN echo 'media-libs/harfbuzz icu -introspection' > /etc/portage/package.use/harfbuzz
RUN emerge -q imagemagick ghostscript-gpl virtual/latex-base && rm -rf /var/cache/distfiles/*
RUN emerge -q --changed-use --deep --keep-going @world && rm -rf /var/cache/distfiles/*
RUN USE="lapack" emerge -q --autounmask=y --autounmask-write --autounmask-continue --autounmask-use=y dev-python/{matplotlib,numpy} && rm -rf /var/cache/distfiles/*
RUN emerge -q dev-python/pip && rm -rf /var/cache/distfiles/*