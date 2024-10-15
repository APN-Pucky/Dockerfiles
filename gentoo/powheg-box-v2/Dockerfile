FROM apnpucky/gentoo-hep-forge as gentoo-powheg-box-v2
LABEL maintainer="APN-Pucky"
ARG POWHEG_BOX_V2_VERSION=4027
RUN emaint -r hep-forge sync
RUN emerge -q virtual/powheg-box-v2 && rm -rf /var/cache/distfiles/*