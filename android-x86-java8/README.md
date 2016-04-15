# android-x86-java8

This docker is to build Android Gradle project with Java 7.
It is available on Docker Hub https://registry.hub.docker.com/u/jacekmarchwicki/android/ .

Contains:

* Android SDK: r24.4.1
* Build tools: 23.0.2, 23.0.3
* Android API: 22, 23
* Support maven repository
* Google maven repository
* x86 emulator: v22
* Platform tools

## Build image

```bash
docker build -t antygravity/android-x86 .
```

If building fail you can debug via where `1b372b1f76f2` is partial build

```bash
docker run --tty --interactive --rm 1b372b1f76f2 /bin/bash
```

## Push build version to repository

```bash
docker push antygravity/android-x86
```

## Usage
Change directory to your project directory, than run:

```bash
docker run --privileged --interactive --volume=$(pwd)/opt/workspace --workdir=/opt/workspace --rm antygravity/android-x86 /bin/sh -c 'emulator-x86 -avd test -no-skin -no-audio -no-window & /opt/tools/android-wait-for-emulator.sh && gradle clean uninstallAll connectedCheck build'
```

