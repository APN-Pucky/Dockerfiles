FROM apnpucky/gentoo-boost as gentoo-python
RUN echo 'PYTHON_TARGETS="python3_11 python3_12 python3_13 python3_14"' >> /etc/portage/make.conf
RUN mkdir -p /etc/portage/profile/ && echo "-python_targets_python3_14" >> /etc/portage/profile/use.mask
RUN emerge -q dev-lang/python:3.11 dev-lang/python:3.12 dev-lang/python:3.13  dev-lang/python:3.14 && rm -rf /var/cache/distfiles/*