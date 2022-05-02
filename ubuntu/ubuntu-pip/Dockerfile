# name the portage image
FROM apnpucky/ubuntu-base
MAINTAINER APN
RUN apt-get update -y -qq && apt-get install -y -qq sudo git rsync pandoc curl python3 python3-pip python3-venv 
RUN python3 -m  pip install --upgrade setuptools wheel pytest  pytest-cov sympy  scipy numpy tqdm  ipython  uncertainties  pandas build matplotlib sphinx-math-dollar jupyterlab numpydoc sphinx nbsphinx sphinx-rtd-theme ipywidgets jupyter_sphinx sphinx-autoapi sphinxcontrib-napoleon