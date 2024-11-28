# gentoo-ansible
FROM apnpucky/gentoo-repo as gentoo-pre-commit
ARG PRE_COMMIT_VERSION=3.8.0
RUN echo 'dev-python/cfgv * ~*' >> /etc/portage/package.accept_keywords/pre-commit
RUN echo 'dev-vcs/pre-commit * ~*' >> /etc/portage/package.accept_keywords/pre-commit
RUN emerge -q ~dev-vcs/pre-commit-${PRE_COMMIT_VERSION} && rm -rf /var/cache/distfiles/*