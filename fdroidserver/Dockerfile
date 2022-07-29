FROM registry.gitlab.com/fdroid/fdroidserver:buildserver as fdroidserver-base
RUN apt-get update -y -qq && apt-get install -y -qq  librsvg2-bin openjdk-11-jdk-headless wget build-essential && apt-get clean && update-alternatives --auto java

FROM apnpucky/fdroidserver-base as fdroidserver-ndk25
RUN wget https://dl.google.com/android/repository/android-ndk-r25-linux.zip && unzip android-ndk-r25-linux.zip -d /opt/android-sdk/ndk/ && rm android-ndk-r25-linux.zip
RUN printf '. /etc/profile\nexport PATH="$fdroidserver:$PATH" PYTHONPATH="$fdroidserver"\ncd /build\nfdroid readmeta\nfdroid rewritemeta $1\nfdroid checkupdates --allow-dirty $1\nfdroid lint $1\nfdroid build $1\n' > all.sh && chmod +x all.sh

