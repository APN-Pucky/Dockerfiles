FROM ubuntu as base
LABEL maintainer="APN-Pucky"
ENV TZ=Europe/Berlin
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime 
RUN echo $TZ > /etc/timezone

FROM apnpucky/ubuntu-base as dev
RUN apt-get update -y -qq && apt-get install -y -qq libffi-dev libboost-all-dev libgsl-dev lcov wget build-essential sudo git rsync pandoc curl python3 python3-pip python3-venv cmake python3-tk graphviz && apt-get clean
RUN update-alternatives --install /usr/bin/python python /usr/bin/python3 1

FROM apnpucky/ubuntu-base as apt
RUN apt-get update -y -qq && apt-get install -y -qq cmake python3-tqdm python3-pandas python3-sphinx python3-sympy python3-matplotlib libffi-dev python3-scipy python3-numpy python3-pytest  python3-pytest-cov  python3-setuptools python3-wheel python3 python3-pip git rsync pandoc python3-venv sudo && apt-get clean

FROM apnpucky/ubuntu-dev as pip
RUN python3 -m  pip install --no-cache-dir --upgrade --break-system-packages setuptools pytest pytest-cov pytest-profiling sympy  scipy numpy tqdm  ipython  uncertainties  pandas build matplotlib sphinx-math-dollar jupyterlab numpydoc sphinx nbsphinx sphinx-rtd-theme ipywidgets jupyter_sphinx sphinx-autoapi sphinxcontrib-napoleon


FROM apnpucky/ubuntu-pip as poetry
RUN apt-get update -y -qq && apt-get install -y -qq pipx && apt-get clean && pipx ensurepath
# Ensure pipx is on PATH
ENV PATH="/root/.local/bin:$PATH"
RUN pipx install poetry

FROM apnpucky/ubuntu-apt as hatch
RUN apt-get update -y -qq && apt-get install -y -qq pipx && apt-get clean && pipx ensurepath
# Ensure pipx is on PATH
ENV PATH="/root/.local/bin:$PATH"
RUN pipx install hatch

FROM apnpucky/ubuntu-hatch as hatch-latexmk
RUN apt-get update -y -qq && apt-get install -y -qq latexmk texlive-full texlive-latex-extra texlive-formats-extra texlive-fonts-extra texlive-science aspell aspell-en aspell-de texlive-bibtex-extra biber texlive-latex-extra python3-pygments && apt-get clean
RUN apt-get update -y -qq && apt-get install -y -qq libmagickwand-dev && apt-get clean
RUN sed -i 's/<policy domain="coder" rights="none" pattern="PDF" \/>/<policy domain="coder" rights="read|write" pattern="PDF" \/>/g' /etc/ImageMagick-**/policy.xml


FROM apnpucky/ubuntu-poetry as python
RUN apt-get update -y -qq && apt-get install -y -qq pipx && apt-get clean && pipx ensurepath
ENV PATH="/root/.local/bin:$PATH"
RUN pipx install pipenv

FROM apnpucky/ubuntu-python as python-lhapdf
ARG LHAPDF_VERSION=6.5.4
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
RUN python -c "import lhapdf;print(lhapdf.__version__)"
RUN lhapdf install cteq6l1 CT14lo CT14nlo MSTW2008lo68cl MSTW2008nlo68cl NNPDF23_nlo_as_0119

FROM apnpucky/ubuntu-python-lhapdf as python-lhapdf-looptools
RUN apt-get update -y -qq && apt-get install -y -qq looptools && apt-get clean

FROM apnpucky/ubuntu-python as python-latexmk
RUN apt-get update -y -qq && apt-get install -y -qq latexmk texlive-full texlive-latex-extra texlive-formats-extra texlive-fonts-extra texlive-science aspell aspell-en aspell-de texlive-bibtex-extra biber texlive-latex-extra python3-pygments && apt-get clean
RUN apt-get update -y -qq && apt-get install -y -qq libmagickwand-dev && apt-get clean
RUN sed -i 's/<policy domain="coder" rights="none" pattern="PDF" \/>/<policy domain="coder" rights="read|write" pattern="PDF" \/>/g' /etc/ImageMagick-**/policy.xml

FROM apnpucky/ubuntu-python-latexmk as python-latexmk-form
RUN wget https://github.com/vermaseren/form/releases/download/v4.3.1/form-4.3.1-x86_64-linux.tar.gz
RUN tar -xzf form-4.3.1-x86_64-linux.tar.gz
ENV PATH=$PATH:$PWD/form-4.3.1-x86_64-linux/

FROM apnpucky/ubuntu-python as python-form
RUN wget https://github.com/vermaseren/form/releases/download/v4.3.1/form-4.3.1-x86_64-linux.tar.gz
RUN tar -xzf form-4.3.1-x86_64-linux.tar.gz
ENV PATH=$PATH:$PWD/form-4.3.1-x86_64-linux/
