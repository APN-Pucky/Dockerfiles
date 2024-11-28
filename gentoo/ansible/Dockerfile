FROM apnpucky/gentoo-repo as gentoo-ansible
LABEL maintainer="APN-Pucky"
ARG ANSIBLE_VERSION=10.2.0
RUN emerge -q app-admin/ansible-lint ~app-admin/ansible-${ANSIBLE_VERSION} && rm -rf /var/cache/distfiles/*