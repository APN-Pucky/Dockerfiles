FROM apnpucky/docker-debian-lhapdf
MAINTAINER APN
RUN apt-get update -y -qq && apt-get install -y -qq sudo git rsync pandoc curl python3-venv python3-numpy
RUN python3 -m  pip install --upgrade setuptools wheel pytest  pytest-cov sympy  scipy tqdm  ipython pandas build uncertainties matplotlib
RUN python3 -m  pip install --upgrade sphinx-math-dollar jupyterlab numpydoc sphinx nbsphinx sphinx-rtd-theme ipywidgets jupyter_sphinx  sphinx-autoapi