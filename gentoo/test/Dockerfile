FROM apnpucky/gentoo-perl as gentoo-test
RUN mkdir /var/db/repos/apn && printf "[apn]\nlocation = /var/db/repos/apn\nsync-type = git\nsync-uri = https://gitlab.com/APN-Pucky/gentoo-apn.git" > /etc/portage/repos.conf/apn.conf && emaint -r apn sync
RUN echo '*/*::apn * ~*' > /etc/portage/package.accept_keywords/apn
RUN echo '*/*::guru * ~*' > /etc/portage/package.accept_keywords/guru
RUN emerge app-eselect/eselect-repository
RUN eselect repository enable guru
RUN emerge --sync
RUN ACCEPT_KEYWORDS="* ~*" emerge -q '>=app-portage/pkg-testing-tools-0.2.1' && rm -rf /var/cache/distfiles/*