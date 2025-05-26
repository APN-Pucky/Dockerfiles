FROM apnpucky/gentoo-repo AS gentoo-g2hep
LABEL maintainer="APN-Pucky"

RUN emerge app-eselect/eselect-repository
RUN eselect repository enable science
RUN eselect repository enable hep-forge
RUN emerge --sync

RUN echo ">=sci-physics/lhapdf-sets-0 lhapdf_sets_ct14lo lhapdf_sets_ct14nlo lhapdf_sets_cteq66 lhapdf_sets_cteq6l1 lhapdf_sets_ct10nnlo lhapdf_sets_pdf4lhc15_nnlo_100" >> /etc/portage/package.use/zz-autouse
RUN echo "sci-physics/evtgen pythia herwig"                                                                          >> /etc/portage/package.use/zz-autouse
RUN echo "sci-physics/thepeg lhapdf hepmc3 fastjet rivet"                                                            >> /etc/portage/package.use/zz-autouse
RUN echo ">=sci-physics/oneloop-3.7.2 qpkind16 -qpkind"                                                              >> /etc/portage/package.use/zz-autouse
RUN echo ">=sci-physics/root-6 -X -qt5 -asimage -opengl -jupyter"                                                    >> /etc/portage/package.use/zz-autouse

RUN USE="python hepmc3 -hepmc2 lhapdf -root ppllj pplljj pplnj-ckm pplnjj-ckm pplla-ew ppjj ppajj ppjjj pphllj-ew pplla ppllaj ppllajj" emerge --autounmask=y --autounmask-write --autounmask-continue --autounmask-use=y  dev-python/setuptools sci-physics/recola2 sci-physics/recola2-SM sci-physics/lhapdf sci-physics/lhapdf-sets sci-physics/looptools sci-physics/rivet:3 sci-physics/pythia:8 sci-physics/openloops '>=sci-physics/hepmc-3.2.7' && rm -rf /var/cache/distfiles/*