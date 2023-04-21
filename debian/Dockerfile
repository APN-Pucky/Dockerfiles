#FROM debian as latexmk
#LABEL maintainer="APN-Pucky"
#RUN apt-get update -y -qq && apt-get install -y -qq latexmk texlive-full texlive-latex-extra texlive-formats-extra texlive-fonts-extra texlive-science aspell aspell-en aspell-de texlive-bibtex-extra biber texlive-latex-extra python3-pygments && apt-get clean

FROM debian:buster as base
LABEL maintainer="APN-Pucky"
RUN apt-get update -y -qq && apt-get install -y -qq wget git curl rsync sudo && apt-get clean

FROM apnpucky/debian-base as cmake-boost-gfortran
LABEL maintainer="APN-Pucky"
RUN apt-get update -y -qq && apt-get install -y -qq git build-essential gfortran cmake libboost-all-dev libgsl-dev lcov libz-dev && apt-get clean
RUN update-alternatives --install /usr/bin/python python /usr/bin/python3 1

FROM apnpucky/docker-debian-cmake-boost-gfortran as lhapdf
ARG LHAPDF_VERSION=6.4.0
ENV LHAPDF=/usr
RUN wget https://lhapdf.hepforge.org/downloads/?f=LHAPDF-$LHAPDF_VERSION.tar.gz -O LHAPDF-$LHAPDF_VERSION.tar.gz && \
  tar xf LHAPDF-$LHAPDF_VERSION.tar.gz && \
  cd LHAPDF-$LHAPDF_VERSION  && \
  ./configure --prefix=$LHAPDF && \
  make && \
  make install && \
  cd .. && \
  rm -r LHAPDF-$LHAPDF_VERSION.tar.gz LHAPDF-$LHAPDF_VERSION
ENV PATH=$PATH:$LHAPDF/bin
ENV LIBRARY_PATH=$LIBRARY_PATH:$LHAPDF/lib
ENV LD_LIBRARY_PATH=$LD_LIBRARY_PATH:$LHAPDF/lib
ENV PYTHONPATH=$PYTHONPATH:$LHAPDF/lib/python3.7/site-packages/
RUN python3 -c "import lhapdf;print(lhapdf.__version__)"
RUN for n in NNPDF40_nlo_as_01180 NNPDF40_lo_as_01180 PDF4LHC15_nlo_mc PDF4LHC21_mc cteq66 cteq6l1 CT14lo CT14nlo MSTW2008lo68cl MSTW2008nlo68cl NNPDF23_nlo_as_0119; do wget http://lhapdfsets.web.cern.ch/lhapdfsets/current/$n.tar.gz && tar xvfz "$n.tar.gz" --directory $LHAPDF/share/LHAPDF/ ; done && rm -rf *.tar.gz

FROM apnpucky/docker-debian-lhapdf as lhapdf-looptools
RUN apt-get update -y -qq && apt-get install -y -qq looptools libooptools-dev && apt-get clean

FROM apnpucky/docker-debian-cmake-boost-gfortran as looptools
RUN apt-get update -y -qq && apt-get install -y -qq looptools && apt-get clean

FROM apnpucky/docker-debian-lhapdf-looptools as resummino
ARG RESUMMINO_VERSION=3.1.2
RUN git clone -b ${RESUMMINO_VERSION} --single-branch https://glpat-kXxsEN2wgp4XUV8tHJyN:glpat-kXxsEN2wgp4XUV8tHJyN@zivgitlab.uni-muenster.de/ag-klasen/resummino.git
RUN cd resummino && mkdir build && cd build && cmake .. -B . && make && make install
