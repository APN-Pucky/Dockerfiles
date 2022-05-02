FROM apnpucky/docker-debian-cmake-boost-gfortran
MAINTAINER APN
RUN apt-get update -qq && apt-get install -y -qq wget
ENV LHAPDF=/usr/share/LHAPDF
RUN wget https://lhapdf.hepforge.org/downloads/?f=LHAPDF-6.2.3.tar.gz -O LHAPDF-6.2.3.tar.gz && tar xf LHAPDF-6.2.3.tar.gz && cd LHAPDF-6.2.3  && ./configure --prefix=$LHAPDF && make && make install
ENV PATH=$PATH:$LHAPDF/bin
ENV LIBRARY_PATH=$LIBRARY_PATH:$LHAPDF/lib
ENV LD_LIBRARY_PATH=$LD_LIBRARY_PATH:$LHAPDF/lib
ENV PYTHONPATH=$PYTHONPATH:$LHAPDF/lib/python3.8/site-packages/LHAPDF-6.2.3-py3.8-linux-x86_64.egg/
RUN for n in NNPDF40_nlo_as_01180 NNPDF40_lo_as_01180 PDF4LHC15_nlo_mc PDF4LHC21_mc cteq66 cteq6l1 CT14lo CT14nlo MSTW2008lo68cl MSTW2008nlo68cl NNPDF23_nlo_as_0119; do wget http://lhapdfsets.web.cern.ch/lhapdfsets/current/$n.tar.gz && tar xvfz "$n.tar.gz" --directory $LHAPDF/share/LHAPDF/ ; done && rm -rf *.tar.gz