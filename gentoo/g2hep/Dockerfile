FROM gentoo/portage:latest AS portage-base
FROM gentoo/stage3:latest AS gentoo-g2hep
LABEL maintainer="APN-Pucky"

ARG NICENESS=14
COPY --from=portage-base /var/db/repos/gentoo /var/db/repos/gentoo
RUN mkdir -p /etc/portage/repos.conf/ && cp /usr/share/portage/config/repos.conf /etc/portage/repos.conf/gentoo.conf
RUN echo "PORTAGE_NICENESS=\"${NICENESS}\"" >> /etc/portage/make.conf
RUN echo "FEATURES=\"${FEATURES} getbinpkg\"" >> /etc/portage/make.conf
#RUN printf "" > /etc/portage/binrepos.conf/gentoobinhost.conf
RUN printf "[amd64gentoobinhost]\npriority=9999\nsync-uri=https://ftp.uni-stuttgart.de/gentoo-distfiles/releases/amd64/binpackages/23.0/x86-64/\n"  > /etc/portage/binrepos.conf/amd64gentoobinhost.conf
RUN printf "[arm64gentoobinhost]\npriority=9999\nsync-uri=https://ftp.uni-stuttgart.de/gentoo-distfiles/releases/arm64/binpackages/23.0/arm64/\n" > /etc/portage/binrepos.conf/arm64gentoobinhost.conf
RUN printf "[armgentoobinhost]\npriority=9999\nsync-uri=https://ftp.uni-stuttgart.de/gentoo-distfiles/releases/arm/binpackages/23.0/armv7a_hf/\n" > /etc/portage/binrepos.conf/armgentoobinhost.conf
RUN printf "[riscvgentoobinhost]\npriority=9999\nsync-uri=https://ftp.uni-stuttgart.de/gentoo-distfiles/releases/riscv/binpackages/23.0/rv64_lp64d/\n" > /etc/portage/binrepos.conf/riscvgentoobinhost.conf
RUN mv /etc/portage/gnupg /etc/portage/gnupg.bak ; getuto


RUN emerge -q eix dev-vcs/git && rm -rf /var/cache/distfiles/*
RUN rm -rf /var/db/repos/gentoo && mkdir -p /var/db/repos/gentoo && printf "[DEFAULT]\nmain-repo = gentoo\n\n[gentoo]\nlocation = /var/db/repos/gentoo\nsync-type = git\nsync-uri = https://github.com/gentoo-mirror/gentoo.git\nauto-sync = yes\nsync-git-verify-commit-signature = yes\nsync-openpgp-key-path = /usr/share/openpgp-keys/gentoo-release.asc" > /etc/portage/repos.conf/gentoo.conf && emerge --sync 
RUN eix-update
RUN install -m 0750 -o portage -g portage -d /etc/portage/env /etc/portage/package.{accept_keywords,env,unmask,use}


RUN emerge app-eselect/eselect-repository
RUN eselect repository enable science
RUN eselect repository enable hep-forge
RUN emerge --sync

RUN echo "=sci-physics/fastjet-3.5.1-r1"                                                                             >> /etc/portage/package.mask/fastjet

RUN echo ">=sci-physics/lhapdf-sets-0 lhapdf_sets_*"                                                                 >> /etc/portage/package.use/zz-autouse
RUN echo "sci-physics/evtgen pythia herwig"                                                                          >> /etc/portage/package.use/zz-autouse
RUN echo "sci-physics/thepeg lhapdf hepmc3 fastjet rivet"                                                            >> /etc/portage/package.use/zz-autouse
RUN echo ">=sci-physics/oneloop-3.7.2 qpkind16 -qpkind"                                                              >> /etc/portage/package.use/zz-autouse
RUN echo ">=sci-physics/root-6 -X -qt5 -asimage -opengl -jupyter"                                                    >> /etc/portage/package.use/zz-autouse
RUN echo "sci-physics/hepmc python_single_target_python3_12 -python_single_target_python3_13"                        >> /etc/portage/package.use/zz-autouse
RUN echo "sci-physics/openloops openloops_processes_*"                                                               >> /etc/portage/package.use/zz-autouse

RUN USE="python hepmc3 -hepmc2 lhapdf -root ppllj pplljj pplnj-ckm pplnjj-ckm pplla-ew ppjj ppajj ppjjj pphllj-ew ppllj-ew pplla ppllaj ppllajj" emerge --autounmask=y --autounmask-write --autounmask-continue --autounmask-use=y  dev-python/setuptools sci-physics/recola2 sci-physics/recola2-SM sci-physics/lhapdf sci-physics/lhapdf-sets sci-physics/looptools sci-physics/fastjet sci-physics/rivet:3 sci-physics/pythia:8 sci-physics/openloops '>=sci-physics/hepmc-3.2.7' && rm -rf /var/cache/distfiles/*
