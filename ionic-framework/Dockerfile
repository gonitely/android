FROM     ubuntu:14.04.4
MAINTAINER bitard [dot] michael [at] gmail [dot] com

ENV DEBIAN_FRONTEND=noninteractive \
    ANDROID_HOME=/opt/android-sdk-linux \
    NODE_VERSION=5.6.0 \
    NPM_VERSION=3.9.3 \
    IONIC_VERSION=1.7.16 \
    BOWER_VERSION=1.7.7 \
    CORDOVA_VERSION=6.0.0 \
    GRUNT_VERSION=0.1.13 \
    GULP_VERSION=3.9.1

# Install basics
RUN apt-get -qq update &&  \
    apt-get -qq install -y git wget curl unzip ruby && \

    curl --retry 3 -SLO "http://nodejs.org/dist/v$NODE_VERSION/node-v$NODE_VERSION-linux-x64.tar.gz" && \
    tar -xzf "node-v$NODE_VERSION-linux-x64.tar.gz" -C /usr/local --strip-components=1 && \
    rm "node-v$NODE_VERSION-linux-x64.tar.gz" && \
    npm install -g --silent npm@"$NPM_VERSION" && \
    npm install -g --silent grunt-cli@"$GRUNT_VERSION" bower@"$BOWER_VERSION"  cordova@"$CORDOVA_VERSION" ionic@"$IONIC_VERSION" gulp@"$GULP_VERSION" && \
    npm cache clear && \
    # gem install fastlane supply \
    gem install -q sass

#ANDROID
#JAVA

# install python-software-properties (so you can do add-apt-repository)
RUN apt-get -qq update && apt-get -qq install -y -q python-software-properties software-properties-common  && \

    add-apt-repository ppa:webupd8team/java -y && \
    echo oracle-java8-installer shared/accepted-oracle-license-v1-1 select true | /usr/bin/debconf-set-selections && \
    apt-get update && apt-get -y install oracle-java7-installer && \


#ANDROID STUFF
    echo ANDROID_HOME="${ANDROID_HOME}" >> /etc/environment && \
    dpkg --add-architecture i386 && \
    apt-get -qq update && \
    apt-get -qq install -y --force-yes expect ant wget libc6-i386 lib32stdc++6 lib32gcc1 lib32ncurses5 lib32z1 qemu-kvm kmod && \
    apt-get clean && \
    apt-get autoclean && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* && \

# Install Android SDK
    cd /opt && \
    wget --output-document=android-sdk.tgz --quiet http://dl.google.com/android/android-sdk_r22.0.1-linux.tgz && \
    tar xzf android-sdk.tgz && \
    rm -f android-sdk.tgz && \
    chown -R root. /opt

# Setup environment

ENV PATH ${PATH}:${ANDROID_HOME}/tools:${ANDROID_HOME}/platform-tools:/opt/tools

# Install sdk elements
COPY tools /opt/tools

RUN ["/opt/tools/android-accept-licenses.sh", "android update sdk --all --no-ui --filter platform-tools"]
RUN ["/opt/tools/android-accept-licenses.sh", "android update sdk --all --no-ui --filter tools"]
RUN unzip -qq -o ${ANDROID_HOME}/temp/*.zip -d ${ANDROID_HOME}
RUN ["/opt/tools/android-accept-licenses.sh", "android update sdk --all --no-ui --filter android-22"]
RUN ["/opt/tools/android-accept-licenses.sh", "android update sdk --all --no-ui --filter build-tools-22.0.1"]

RUN add-apt-repository ppa:brightbox/ruby-ng -y && \
    apt-get -qq update && \
    apt-get -qq install -y build-essential ruby2.0-dev ruby2.0 && \
    gem install fastlane supply
