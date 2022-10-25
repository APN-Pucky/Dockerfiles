#FROM easybuilders/test:centos7-foss-2020a as centos-easybuild-foss-2020a
#ARG CPU_ARCH=x86_64-pc-linux-gnu
##ARG EASYCONFIG=--install-latest-eb-release
#RUN eb --prefix /scratch --installpath /easybuild --install-latest-eb-release --robot --trace   && rm -rf /scratch/*
##RUN eb --prefix /scratch --installpath /easybuild ${EASYCONFIG} --robot --trace   && rm -rf /scratch/*
#
#FROM apnpucky/centos7-easybuild-foss-2020a:latest as centos-easybuild-foss-2020a-x
#ARG EASYINSTALL=--install-latest-eb-release
##RUN eb --prefix /scratch --installpath /easybuild --install-latest-eb-release --robot --trace   && rm -rf /scratch/*
#RUN ml use /easybuild/modules/all && ml load EasyBuild &&  eb --allow-use-as-root-and-accept-consequences --prefix=/scratch  --ignore-osdeps --robot --trace --installpath=/easybuild ${EASYINSTALL}
##RUN ml use /easybuild/modules/all && ml load EasyBuild && eb --prefix /scratch --installpath /easybuild ${EASYCONFIG} --robot --trace   && rm -rf /scratch/*

# install EasyBuild and make sure it's available
FROM easybuilders/base:centos7-eb4.2.1 as centos7-easybuild
ARG CPU_ARCH=x86_64-pc-linux-gnu
ARG EASYINSTALL1=none
RUN if [ "$EASYINSTALL1" != "none" ]; then ; eb --prefix /scratch --installpath /easybuild ${EASYINSTALL1} --robot --trace   && rm -rf /scratch/* ; fi
ARG EASYINSTALL2=none
RUN if [ "$EASYINSTALL2" != "none" ]; then ; eb --prefix /scratch --installpath /easybuild ${EASYINSTALL2} --robot --trace   && rm -rf /scratch/* ; fi
ARG EASYINSTALL3=none
RUN if [ "$EASYINSTALL3" != "none" ]; then ; eb --prefix /scratch --installpath /easybuild ${EASYINSTALL3} --robot --trace   && rm -rf /scratch/* ; fi
ARG EASYINSTALL4=none
RUN if [ "$EASYINSTALL4" != "none" ]; then ; eb --prefix /scratch --installpath /easybuild ${EASYINSTALL4} --robot --trace   && rm -rf /scratch/* ; fi
ARG EASYINSTALL5=none
RUN if [ "$EASYINSTALL5" != "none" ]; then ; eb --prefix /scratch --installpath /easybuild ${EASYINSTALL5} --robot --trace   && rm -rf /scratch/* ; fi



## gentoo-easybuild
#FROM apnpucky/gentoo-repo as gentoo-easybuild
#ARG EASYCONFIG=foss-2022a.eb
#RUN echo 'sys-cluster/lmod ~amd64' >> /etc/portage/package.accept_keywords/zz-autoaccept
#RUN emerge -q lmod dev-python/pip && rm -rf /var/cache/distfiles/*
#RUN pip install easybuild --user
#RUN emerge -q sys-cluster/rdma-core && rm -rf /var/cache/distfiles/*
#ENV PATH="${PATH}:/root/.local/bin"
#RUN ln -s /usr/share/Lmod/init/profile /etc/bash/bashrc.d/z00_lmod.sh 	
#RUN ln -s /usr/share/Lmod/init/profile /etc/profile.d/z00_lmod.sh 	
#RUN ln -s /usr/share/Lmod/init/cshrc /etc/profile.d/z00_lmod.csh 	
##RUN ln -s /usr/share/Lmod/init/profile.fish /etc/fish/conf.d/z00_lmod.fish
#RUN source /usr/share/Lmod/init/profile && \
#	eb --allow-use-as-root-and-accept-consequences --ignore-osdeps --install-latest-eb-release --prefix /scratch --installpath /easybuild   && rm -rf /scratch/*   && echo 'ml use /easybuild/modules/all && ml EasyBuild' >> ~/.bashrc
#RUN source /usr/share/Lmod/init/profile && \
#	eb --allow-use-as-root-and-accept-consequences --ignore-osdeps --prefix /scratch --installpath /easybuild ${EASYCONFIG} --robot --trace   && rm -rf /scratch/*
##RUN eb --allow-use-as-root-and-accept-consequences --prefix=/easybuild_prefix/ GCCcore-11.3.0.eb --ignore-osdeps --robot --installpath=/easybuild_install/