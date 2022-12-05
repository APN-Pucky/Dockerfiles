FROM ubuntu:20.04 as base
LABEL maintainer="APN-Pucky"
ENV TZ=Europe/Berlin
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

FROM apnpucky/ubuntu-base as dev
RUN apt-get update -y -qq && apt-get install -y -qq libffi-dev libboost-all-dev libgsl-dev lcov wget build-essential sudo git rsync pandoc curl python3 python3-pip python3-venv cmake python3-tk graphviz && apt-get clean
RUN update-alternatives --install /usr/bin/python python /usr/bin/python3 1

FROM apnpucky/ubuntu-base as apt
RUN apt-get update -y -qq && apt-get install -y -qq python3-tqdm  python3-uncertainties python3-sympy libffi-dev python3-scipy python3-numpy python3-pytest  python3-pytest-cov  python3-setuptools python3-wheel python3 python3-pip git rsync pandoc python3-venv sudo && apt-get clean
RUN python3 -m pip install --no-cache-dir --upgrade nbconvert notebook sphinx-math-dollar numpydoc sphinx nbsphinx sphinx-rtd-theme matplotlib pandas build

FROM apnpucky/ubuntu-dev as pip
RUN python3 -m  pip install --no-cache-dir --upgrade setuptools wheel pytest pytest-cov pytest-profiling sympy  scipy numpy tqdm  ipython  uncertainties  pandas build matplotlib sphinx-math-dollar jupyterlab numpydoc sphinx nbsphinx sphinx-rtd-theme ipywidgets jupyter_sphinx sphinx-autoapi sphinxcontrib-napoleon

FROM apnpucky/ubuntu-pip as poetry
RUN python3 -m pip install --no-cache-dir --upgrade poetry poetry-dynamic-versioning

FROM apnpucky/ubuntu-poetry as python
RUN python3 -m pip install --no-cache-dir --upgrade pipenv

FROM apnpucky/ubuntu-python as python-lhapdf
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
ENV PYTHONPATH=$PYTHONPATH:$LHAPDF/lib/python3.8/site-packages/LHAPDF-$LHAPDF_VERSION-py3.8-linux-x86_64.egg/:/usr/lib/python3.8/site-packages/
#RUN python -c "import lhapdf;print(lhapdf.__version__)"
RUN lhapdf install cteq6l1 CT14lo CT14nlo MSTW2008lo68cl MSTW2008nlo68cl NNPDF23_nlo_as_0119

FROM apnpucky/ubuntu-python-lhapdf as python-lhapdf-looptools
RUN apt-get update -y -qq && apt-get install -y -qq looptools && apt-get clean


FROM apnpucky/ubuntu-python as python-latexmk
RUN apt-get update -y -qq && apt-get install -y -qq latexmk texlive-full texlive-latex-extra texlive-formats-extra texlive-fonts-extra texlive-science aspell aspell-en aspell-de texlive-bibtex-extra biber texlive-latex-extra python3-pygments && apt-get clean
RUN apt-get update -y -qq && apt-get install -y -qq libmagickwand-dev && apt-get clean
ARG imagemagic_config=/etc/ImageMagick-7/policy.xml
RUN sed -i 's/<policy domain="coder" rights="none" pattern="PDF" \/>/<policy domain="coder" rights="read|write" pattern="PDF" \/>/g' $imagemagic_config
