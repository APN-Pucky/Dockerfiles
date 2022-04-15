# name the portage image
FROM ubuntu:latest
MAINTAINER APN
RUN apt-get update -y -qq && apt-get install -y -qq python3 python3-pip git rsync pandoc python3-venv sudo
RUN apt-get install -y -qq python3-setuptools python3-wheel 
RUN apt-get install  -y -qq python3-pytest  python3-pytest-cov
RUN apt-get install  -y -qq python3-sympy  python3-scipy python3-numpy
RUN apt-get install  -y -qq  python3-tqdm  python3-uncertainties
RUN python3 -m pip install --upgrade nbconvert notebook
RUN python3 -m pip install --upgrade sphinx-math-dollar numpydoc sphinx nbsphinx sphinx-rtd-theme matplotlib pandas build