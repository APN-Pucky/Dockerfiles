FROM apnpucky/gentoo-repo as gentoo-g2hep
LABEL maintainer="APN-Pucky"

RUN emerge app-eselect/eselect-repository
RUN eselect repository enable science
RUN eselect repository enable hep-forge
RUN emerge --sync

RUN echo ">=sci-physics/lhapdf-sets-0 lhapdf_sets_ct14lo lhapdf_sets_ct14nlo lhapdf_sets_cteq66 lhapdf_sets_cteq6l1" >> /etc/portage/package.use/zz-autoaccept
RUN echo "sci-physics/evtgen pythia herwig"                                                                          >> /etc/portage/package.use/zz-autoaccept
RUN echo "sci-physics/thepeg lhapdf hepmc3 fastjet rivet"                                                            >> /etc/portage/package.use/zz-autoaccept
RUN echo ">=sci-libs/hdf5-1.14.4_p3-r1 cxx"                                                                          >> /etc/portage/package.use/zz-autoaccept
RUN echo ">=sci-physics/oneloop-3.7.2 qpkind16 -qpkind"                                                              >> /etc/portage/package.use/zz-autoaccept
RUN echo ">=sci-physics/root-6 -X -qt5 -asimage -opengl -jupyter"                                                    >> /etc/portage/package.use/zz-autoaccept

RUN echo 'sci-physics/looptools **'    >> /etc/portage/package.accept_keywords/zz-autoaccept
RUN echo 'sci-physics/pythia ~*'       >> /etc/portage/package.accept_keywords/zz-autoaccept
RUN echo 'sci-physics/hepmc ~*'        >> /etc/portage/package.accept_keywords/zz-autoaccept
RUN echo 'sci-physics/yoda ~*'         >> /etc/portage/package.accept_keywords/zz-autoaccept
RUN echo 'sci-physics/rivet ~*'        >> /etc/portage/package.accept_keywords/zz-autoaccept
RUN echo 'dev-python/uncertainties ~*' >> /etc/portage/package.accept_keywords/zz-autoaccept
RUN echo 'sci-physics/herwig ~*'       >> /etc/portage/package.accept_keywords/zz-autoaccept
RUN echo 'sci-physics/thepeg ~*'       >> /etc/portage/package.accept_keywords/zz-autoaccept
RUN echo 'sci-physics/root ~*'         >> /etc/portage/package.accept_keywords/zz-autoaccept

RUN USE="lapack python hepmc3 -hepmc2 lhapdf root" emerge -q powheg-box-v2 sci-physics/lhapdf sci-physics/lhapdf-sets sci-physics/looptools::hep-forge sci-physics/rivet:3 sci-physics/pythia:8 sci-physics/openloops '>=sci-physics/hepmc-3.2.7' && rm -rf /var/cache/distfiles/*
# TODO hepi, resummino, sherpa, herwig, n(n)ll-fast scikit-hep