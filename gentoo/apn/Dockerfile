FROM apnpucky/gentoo-dev as gentoo-apn
RUN mkdir -p /var/db/repos/apn && printf "[apn]\nlocation = /var/db/repos/apn\nsync-type = git\nsync-uri = https://gitlab.com/APN-Pucky/gentoo-apn.git" > /etc/portage/repos.conf/apn.conf && emaint -r apn sync
RUN echo '*/*::apn * ~*' > /etc/portage/package.accept_keywords/apn
RUN echo '*/*::guru * ~*' > /etc/portage/package.accept_keywords/guru
RUN echo '*/*::jjakob * ~*' > /etc/portage/package.accept_keywords/jjakob
RUN emerge app-eselect/eselect-repository
RUN eselect repository enable guru
RUN eselect repository enable jjakob
RUN emerge --sync
