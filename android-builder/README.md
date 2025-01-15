# Android Builder Docker Image

This Docker image is configured for building Android applications. The base image is configured separately in a `base.env` file to keep sensitive repository information private.

## Setup

1. Create a `base.env` file in this directory with the following content:
```
BASE_IMAGE=your-registry/hardened/redhat/openjdk/openjdk11:1.11
```

## Building the Image

### Windows (PowerShell)
```powershell
docker build --build-arg BASE_IMAGE=$(Get-Content base.env | ForEach-Object { $_.Split('=')[1] }) -t android-builder .
```

### Unix (Bash)
```bash
docker build --build-arg BASE_IMAGE=$(cat base.env | cut -d'=' -f2) -t android-builder .
```

## Note
- The `base.env` file is ignored by Git to keep the base image information private
- Make sure you have access to the base image repository before building
