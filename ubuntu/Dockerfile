FROM i686/ubuntu as i686-libxt
LABEL maintainer="APN-Pucky"
ENV TZ=Europe/Berlin
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone
RUN apt-get update -y -qq && apt-get install -y -qq libXt-dev  libXext-dev && apt-get clean

FROM ubuntu:20.04 as base
LABEL maintainer="APN-Pucky"
ENV TZ=Europe/Berlin
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

FROM apnpucky/ubuntu-base as dev
RUN apt-get update -y -qq && apt-get install -y -qq libboost-all-dev libgsl-dev lcov wget build-essential sudo git rsync pandoc curl python3 python3-pip python3-venv cmake python3-tk graphviz && apt-get clean
RUN update-alternatives --install /usr/bin/python python /usr/bin/python3 1

FROM apnpucky/ubuntu-base as apt
RUN apt-get update -y -qq && apt-get install -y -qq python3-tqdm  python3-uncertainties python3-sympy  python3-scipy python3-numpy python3-pytest  python3-pytest-cov  python3-setuptools python3-wheel python3 python3-pip git rsync pandoc python3-venv sudo && apt-get clean
RUN python3 -m pip install --no-cache-dir --upgrade nbconvert notebook sphinx-math-dollar numpydoc sphinx nbsphinx sphinx-rtd-theme matplotlib pandas build

FROM apnpucky/ubuntu-dev as pip
RUN python3 -m  pip install --no-cache-dir --upgrade setuptools wheel pytest pytest-cov pytest-profiling sympy  scipy numpy tqdm  ipython  uncertainties  pandas build matplotlib sphinx-math-dollar jupyterlab numpydoc sphinx nbsphinx sphinx-rtd-theme ipywidgets jupyter_sphinx sphinx-autoapi sphinxcontrib-napoleon

FROM apnpucky/ubuntu-pip as pip-lhapdf
ARG LHAPDF_VERSION=6.4.0
ENV LHAPDF=/usr
RUN wget https://lhapdf.hepforge.org/downloads/?f=LHAPDF-$LHAPDF_VERSION.tar.gz -O LHAPDF-$LHAPDF_VERSION.tar.gz && tar xf LHAPDF-$LHAPDF_VERSION.tar.gz && cd LHAPDF-$LHAPDF_VERSION  && ./configure --prefix=$LHAPDF && make && make install
ENV PATH=$PATH:$LHAPDF/bin
ENV LIBRARY_PATH=$LIBRARY_PATH:$LHAPDF/lib
ENV LD_LIBRARY_PATH=$LD_LIBRARY_PATH:$LHAPDF/lib
ENV PYTHONPATH=$PYTHONPATH:$LHAPDF/lib/python3.8/site-packages/LHAPDF-$LHAPDF_VERSION-py3.8-linux-x86_64.egg/
RUN python -c "import lhapdf;print(lhapdf.__version__)"
RUN for n in cteq6l1 CT14lo CT14nlo MSTW2008lo68cl MSTW2008nlo68cl NNPDF23_nlo_as_0119; do wget http://lhapdfsets.web.cern.ch/lhapdfsets/current/$n.tar.gz && tar xvfz "$n.tar.gz" --directory $LHAPDF/share/LHAPDF/ ; done