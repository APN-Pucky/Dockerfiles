FROM debian as debian-gentoo-prefix
RUN apt-get update && apt-get upgrade -y && apt-get install -y build-essential wget
RUN wget https://gitweb.gentoo.org/repo/proj/prefix.git/plain/scripts/bootstrap-prefix.sh && chmod +x bootstrap-prefix.sh && sed -i 's/UID/NOUID/g' bootstrap-prefix.sh && ./bootstrap-prefix.sh /gentoo-prefix noninteractive
# set entry point to start gentoo prefix
ENTRYPOINT ["/gentoo-prefix/startprefix"]
