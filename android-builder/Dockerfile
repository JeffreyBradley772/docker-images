# Pull base image from Azure Container Registry
ARG BASE_IMAGE
FROM ${BASE_IMAGE}

# Set maintainer label
LABEL maintainer="Higher Ground"

# Set environment variables
ENV ANDROID_HOME=/opt/android-sdk
ENV PATH=${PATH}:${ANDROID_HOME}/cmdline-tools/latest/bin:${ANDROID_HOME}/platform-tools

# Switch to root user
USER root

# Install necessary packages
RUN dnf update -y && \
    dnf install -y \
    wget \
    unzip \
    which \
    git \
    gcc \
    gcc-c++ \
    make \
    openssl-devel \
    && dnf clean all

# Install CMake 3.22.1
RUN wget https://github.com/Kitware/CMake/releases/download/v3.22.1/cmake-3.22.1.tar.gz && \
    tar -zxvf cmake-3.22.1.tar.gz && \
    cd cmake-3.22.1 && \
    ./bootstrap && \
    make && \
    make install && \
    cd .. && \
    rm -rf cmake-3.22.1 cmake-3.22.1.tar.gz

# Create Android SDK directory
RUN mkdir -p ${ANDROID_HOME}

# Download and install Android Command Line Tools
RUN wget -q https://dl.google.com/android/repository/commandlinetools-linux-9477386_latest.zip -O cmdline-tools.zip && \
    mkdir -p ${ANDROID_HOME}/cmdline-tools && \
    unzip -q cmdline-tools.zip -d ${ANDROID_HOME}/cmdline-tools && \
    mv ${ANDROID_HOME}/cmdline-tools/cmdline-tools ${ANDROID_HOME}/cmdline-tools/latest && \
    rm cmdline-tools.zip

# Download and install Gradle
# Using Gradle 7.5.1 which is compatible with Android Gradle Plugin 7.4.2
RUN mkdir -p /opt/gradle && \
    wget https://services.gradle.org/distributions/gradle-7.5.1-bin.zip -P /tmp && \
    unzip -d /opt /tmp/gradle-7.5.1-bin.zip && \
    ln -s /opt/gradle-7.5.1 /opt/gradle && \
    rm /tmp/gradle-7.5.1-bin.zip

# Set Gradle environment variables
ENV GRADLE_HOME=/opt/gradle/gradle-7.5.1
ENV PATH=${PATH}:${GRADLE_HOME}/bin

# Accept licenses
RUN yes | sdkmanager --licenses

# Install Android SDK components
RUN sdkmanager \
    "build-tools;34.0.0" \
    "platforms;android-34" \
    "extras;google;m2repository" \
    "extras;android;m2repository"

# Create a non-root user with a home directory
RUN useradd -m -d /home/android-builder -s /bin/bash android-builder

# Switch to the non-root user
USER android-builder

# Set working directory
WORKDIR /app
