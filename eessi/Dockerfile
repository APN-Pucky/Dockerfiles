FROM debian:bookworm as eessi
LABEL maintainer="APN-Pucky"
RUN apt-get update -y -qq && apt-get install -y -qq wget git gpg curl rsync sudo zip && apt-get clean
RUN wget -qO - https://cvmrepo.web.cern.ch/cvmrepo/apt/cernvm.gpg | gpg --dearmor -o /usr/share/keyrings/repo-key.gpg
RUN echo 'deb [signed-by=/usr/share/keyrings/repo-key.gpg] http://cvmrepo.web.cern.ch/cvmrepo/apt/ bookworm-prod main' | sudo tee /etc/apt/sources.list.d/cernvm.list
RUN apt-get update -y -qq && apt-get install -y -qq cvmfs && apt-get clean
RUN echo 'OPTIONS="--timeout 0"' >> /etc/default/autofs
RUN echo 'CVMFS_CLIENT_PROFILE="single"' >> /etc/cvmfs/default.local
RUN cvmfs_config setup
RUN systemctl enable autofs