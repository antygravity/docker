#based on https://hub.docker.com/r/jacekmarchwicki/android/

FROM ubuntu:14.04.4

MAINTAINER Marcin L "antygravity@wp.pl"

# Install java8
RUN apt-get install -y software-properties-common && add-apt-repository -y ppa:webupd8team/java && apt-get update
RUN echo oracle-java8-installer shared/accepted-oracle-license-v1-1 select true | /usr/bin/debconf-set-selections
RUN apt-get install -y oracle-java8-installer

# Install Deps
RUN apt-get update && apt-get install -y --force-yes expect git unzip wget  python curl lib32z1 lib32ncurses5 lib32bz2-1.0 lib32stdc++6 qemu-kvm libvirt-bin ubuntu-vm-builder bridge-utils

# Install Android SDK
RUN cd /opt  && wget --output-document=android-sdk.tgz --quiet http://dl.google.com/android/android-sdk_r24.4.1-linux.tgz  && tar xzf android-sdk.tgz && rm -f android-sdk.tgz

# Setup environment
ENV ANDROID_HOME /opt/android-sdk-linux
ENV PATH ${PATH}:${ANDROID_HOME}/tools:${ANDROID_HOME}/platform-tools

# Install sdk elements
COPY tools /opt/tools
ENV PATH ${PATH}:/opt/tools
ENV adb kill-server
RUN ["/opt/tools/android-accept-licenses.sh", "android update sdk --all --force --no-ui --filter platform-tools,build-tools-23.0.2,build-tools-23.0.3,android-23,android-22,addon-google_apis_x86-google-23,extra-android-support,extra-android-m2repository,extra-google-m2repository,extra-google-google_play_services,sys-img-x86-android-22"]

RUN which adb
RUN which android

# Create emulator
RUN echo "no" | android create avd --force -n test -t android-22 --abi x86

#KVM
RUN /opt/tools/kvm-mknod.sh

#Gradle
RUN wget https://services.gradle.org/distributions/gradle-2.12-bin.zip && unzip gradle-2.12-bin.zip && mv gradle-2.12 /opt/
ENV GRADLE_HOME /opt/gradle-2.12
ENV PATH ${PATH}:${GRADLE_HOME}/bin
ENV gradle -version

# Cleaning
RUN apt-get clean && apt-get autoremove -y

# GO to workspace
RUN mkdir -p /opt/workspace
WORKDIR /opt/workspace
