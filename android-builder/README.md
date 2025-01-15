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

## Running the Image

If you want to run the image to do something, you can run the following command:

```bash
docker run -it --rm android-builder
```

To run the image and use Gradle, you'll need to mount your project directory. For example:

```bash
docker run -it --rm -v ${PWD}:/workspace -w /workspace android-builder
```

This will:
- Mount your current directory to `/workspace` in the container
- Set the working directory to `/workspace`
- Give you an interactive shell where you can run Gradle commands

Example Gradle commands you can run:
```bash
gradle --version  # Check gradle version
./gradlew build  # Build your project (if using Gradle wrapper)
gradle build     # Build your project (using container's Gradle)
```

## Note
- The `base.env` file is ignored by Git to keep the base image information private
- Make sure you have access to the base image repository before building
