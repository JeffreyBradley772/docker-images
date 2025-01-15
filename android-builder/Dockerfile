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
    && dnf clean all

# Create Android SDK directory
RUN mkdir -p ${ANDROID_HOME}

# Download and install Android Command Line Tools
RUN wget -q https://dl.google.com/android/repository/commandlinetools-linux-9477386_latest.zip -O cmdline-tools.zip && \
    mkdir -p ${ANDROID_HOME}/cmdline-tools && \
    unzip -q cmdline-tools.zip -d ${ANDROID_HOME}/cmdline-tools && \
    mv ${ANDROID_HOME}/cmdline-tools/cmdline-tools ${ANDROID_HOME}/cmdline-tools/latest && \
    rm cmdline-tools.zip

# Accept licenses
RUN yes | sdkmanager --licenses

# Install basic Android SDK packages
RUN sdkmanager \
    "platform-tools" \
    "build-tools;34.0.0" \
    "platforms;android-34"

# Switch back to the non-root user
USER android-builder

# Set working directory
WORKDIR /app
