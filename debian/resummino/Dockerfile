FROM apnpucky/docker-debian-lhapdf-looptools
MAINTAINER APN
RUN apt-get update -y -qq && apt-get install -y -qq git 
RUN git clone -b 3.1.1 --single-branch https://glpat-kXxsEN2wgp4XUV8tHJyN:glpat-kXxsEN2wgp4XUV8tHJyN@zivgitlab.uni-muenster.de/ag-klasen/resummino.git
RUN cd resummino && mkdir build && cd build && cmake .. -B . && make && make install