FROM apnpucky/gentoo-hep-forge as gentoo-scikit-hep
LABEL maintainer="APN-Pucky"
ARG SCIKIT_HEP_VERSION=2023.08.01
RUN emaint -r hep-forge sync
RUN echo 'dev-python/iminuit ~*' >> /etc/portage/package.accept_keywords/zz-autoaccept
RUN echo 'dev-python/scipy ~*' >> /etc/portage/package.accept_keywords/zz-autoaccept
RUN emerge -q ~virtual/scikit-hep:${SCIKIT_HEP_VERSION} && rm -rf /var/cache/distfiles/*