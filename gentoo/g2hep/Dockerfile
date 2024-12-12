FROM apnpucky/gentoo-hep-forge as gentoo-lhapdf
LABEL maintainer="APN-Pucky"
RUN emaint -r gentoo sync
RUN emaint -r science sync
RUN emaint -r hep-forge sync
RUN echo ">=sci-physics/lhapdf-sets-0 lhapdf_sets_ct14lo lhapdf_sets_ct14nlo lhapdf_sets_cteq66 lhapdf_sets_cteq6l1" >> /etc/portage/package.use/zz-autoaccept
RUN echo 'sci-physics/looptools **' >> /etc/portage/package.accept_keywords/zz-autoaccept
RUN echo 'sci-physics/pythia ~*' >> /etc/portage/package.accept_keywords/zz-autoaccept
RUN echo 'sci-physics/hepmc ~*' >> /etc/portage/package.accept_keywords/zz-autoaccept
RUN echo 'sci-physics/yoda ~*' >> /etc/portage/package.accept_keywords/zz-autoaccept
RUN echo 'sci-physics/rivet ~*' >> /etc/portage/package.accept_keywords/zz-autoaccept
RUN USE="python hepmc3 -hepmc2 lhapdf" emerge -q sci-physics/lhapdf sci-physics/lhapdf-sets sci-physics/looptools sci-physics/rivet sci-physics/openloops sci-physics/herwig virtual/scikit-hep && rm -rf /var/cache/distfiles/*