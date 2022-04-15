# name the portage image
FROM ubuntu:latest
MAINTAINER APN
RUN apt-get update -y -qq && apt-get install -y -qq sudo git rsync pandoc curl python3 python3-pip python3-venv 
RUN python3 -m  pip install --upgrade setuptools wheel pytest  pytest-cov sympy  scipy numpy tqdm  ipython  uncertainties 
RUN python3 -m  pip install --upgrade sphinx-math-dollar jupyterlab numpydoc sphinx nbsphinx sphinx-rtd-theme matplotlib pandas build 
RUN python3 -m  pip install --upgrade ipywidgets
RUN python3 -m  pip install --upgrade jupyter_sphinx
