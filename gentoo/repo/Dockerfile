FROM apnpucky/gentoo-base as gentoo-repo
RUN emerge -q eix dev-vcs/git && rm -rf /var/cache/distfiles/*
RUN rm -rf /var/db/repos/gentoo && mkdir -p /var/db/repos/gentoo && printf "[DEFAULT]\nmain-repo = gentoo\n\n[gentoo]\nlocation = /var/db/repos/gentoo\nsync-type = git\nsync-uri = https://github.com/gentoo-mirror/gentoo.git\nauto-sync = yes\nsync-git-verify-commit-signature = yes\nsync-openpgp-key-path = /usr/share/openpgp-keys/gentoo-release.asc" > /etc/portage/repos.conf/gentoo.conf && emerge --sync 
RUN eix-update
RUN install -m 0750 -o portage -g portage -d /etc/portage/env /etc/portage/package.{accept_keywords,env,unmask,use}