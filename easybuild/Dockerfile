# Based on containers from https://github.com/easybuilders/easybuild-tutorial
ARG IMG_REGISTRY=docker.io
ARG IMG_ORG=library
ARG IMG_NAME=centos
ARG IMG_TAG=7
# allow CI/CD to change all of the above
FROM ${IMG_REGISTRY}/${IMG_ORG}/${IMG_NAME}:${IMG_TAG} as centos7-easybuild
# set default easybuild version
ARG EB_VER=4.6.1
LABEL easybuild_version=${EB_VER}
# install required packages (EPEL is required for Lmod)
RUN yum install -y epel-release \
  && yum install -y bzip2 file gcc-c++ git Lmod make openssl openssl-devel patch python3-pip unzip vim which rdma-core-devel perl-Thread-Queue debianutils
# install EasBuild & archspec
RUN pip3 install easybuild==${EB_VER} archspec pep8
# add 'easybuild' user
RUN useradd -ms /bin/bash easybuild
# create /scratch & /easybuild directories
RUN mkdir /scratch && chown easybuild:easybuild /scratch \
  && mkdir /easybuild && chown easybuild:easybuild /easybuild
# disable output buffering in Python, so we see EasyBuild output as it progresses
ENV PYTHONUNBUFFERED=TRUE
# Define the ENTRYPOINT for subsequent commands (CMD)
ENTRYPOINT ["/bin/bash", "-l", "-c"]
# SHELL defines which login shell will be used within RUN steps
SHELL ["/bin/bash", "-l", "-c"]
# By default the container will start a bash shell
CMD ["bash"]
# switch to 'easybuild' user
USER easybuild
# install EasyBuild as a module (& clean up /scratch);
# need to specify EB_PYTHON to ensure python3 is used,
# since pip3 was used for installation and  /usr/bin/python also exists
RUN EB_PYTHON=/usr/bin/python3 eb --install-latest-eb-release --prefix /scratch --installpath /easybuild \
  && rm -rf /scratch/* \
  # load EasyBuild module through .bashrc startup script
  && echo 'ml use /easybuild/modules/all && ml EasyBuild' >> ~/.bashrc
# remove globally installed EasyBuild, we can use the module going forward
USER root
RUN pip3 uninstall -y easybuild easybuild-framework easybuild-easyblocks easybuild-easyconfigs
# switch to 'easybuild' user
USER easybuild
# print info on Lmod & EasyBuild
RUN ml --version \
  && which -a eb \
  && eb --version \
  && eb --show-system-info
# make sure we start in /home/easybuild rather than / when shell in started in container
WORKDIR /home/easybuild
RUN eb --prefix /scratch --installpath /easybuild --install-latest-eb-release --robot --trace   && rm -rf /scratch/* 
FROM apnpucky/centos7-easybuild as centos7-easybuild-ci
RUN eb --prefix /scratch --installpath /easybuild --install-latest-eb-release --robot --trace   && rm -rf /scratch/* 
RUN eb --prefix /scratch --installpath /easybuild GCCcore-11.3.0.eb gompi-2022a.eb Python-3.10.4-GCCcore-11.3.0.eb  --robot --trace   && rm -rf /scratch/*
# Some heavy load
RUN eb --prefix /scratch --installpath /easybuild Qt5-5.15.5-GCCcore-11.3.0.eb nodejs-16.15.1-GCCcore-11.3.0.eb Mesa-22.0.3-GCCcore-11.3.0.eb --robot --trace   && rm -rf /scratch/*
RUN eb --prefix /scratch --installpath /easybuild GCCcore-12.3.0.eb gompi-2023a.eb --robot --trace   && rm -rf /scratch/*


#####################
# Gentoo
#####################


# gentoo-easybuild
FROM apnpucky/gentoo-repo as gentoo-easybuild-pip
ARG EASYCONFIG=foss-2022a.eb
RUN echo 'sys-cluster/lmod ~amd64' >> /etc/portage/package.accept_keywords/zz-autoaccept
RUN emerge -q lmod dev-python/pip && rm -rf /var/cache/distfiles/*
RUN pip install easybuild --user
RUN emerge -q sys-cluster/rdma-core && rm -rf /var/cache/distfiles/*
ENV PATH="${PATH}:/root/.local/bin"
RUN ln -s /usr/share/Lmod/init/profile /etc/bash/bashrc.d/z00_lmod.sh 	
RUN ln -s /usr/share/Lmod/init/profile /etc/profile.d/z00_lmod.sh 	
RUN ln -s /usr/share/Lmod/init/cshrc /etc/profile.d/z00_lmod.csh 	
#RUN ln -s /usr/share/Lmod/init/profile.fish /etc/fish/conf.d/z00_lmod.fish
RUN source /usr/share/Lmod/init/profile && \
  eb --allow-use-as-root-and-accept-consequences --ignore-osdeps --install-latest-eb-release --prefix /scratch --installpath /easybuild   && rm -rf /scratch/*   && echo 'ml use /easybuild/modules/all && ml EasyBuild' >> ~/.bashrc
RUN source /usr/share/Lmod/init/profile && \
  eb --allow-use-as-root-and-accept-consequences --ignore-osdeps --prefix /scratch --installpath /easybuild ${EASYCONFIG} --robot --trace   && rm -rf /scratch/*
#RUN eb --allow-use-as-root-and-accept-consequences --prefix=/easybuild_prefix/ GCCcore-11.3.0.eb --ignore-osdeps --robot --installpath=/easybuild_install/

