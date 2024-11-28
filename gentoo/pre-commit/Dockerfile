# gentoo-ansible
FROM apnpucky/gentoo-repo as gentoo-pre-commit
ARG PRE_COMMIT_VERSION=3.8.0
RUN emerge -q ~dev-vcs/pre-commit-${PRE_COMMIT_VERSION} && rm -rf /var/cache/distfiles/*