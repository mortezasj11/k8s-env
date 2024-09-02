
# FROM nvidia/cuda:12.1.0-devel-ubuntu22.04

    ## where to find:
    Docker Hub:
    Go to the Docker Hub website.
    Search for "nvidia/cuda" in the search bar.
    Click on the "nvidia/cuda" repository to view the list of available tags.
    In the tags section, you can look for 12.1.0-devel-ubuntu22.04 or any specific version you need.
    ## Difference between 
    1. docker pull nvidia/cuda:12.6.0-cudnn-devel-ubuntu20.04 
    2. FROM nvidia/cuda:12.6.0-cudnn-devel-ubuntu20.04
    docker pull is a standalone command used to download or update Docker images on your system.
    FROM in a Dockerfile specifies the base image for building a new Docker image and is part of a sequence of commands that define the environment and configuration of your container.

# set environment variables
    ENV DEBIAN_FRONTEND noninteractive
    ENV HDF5_USE_FILE_LOCKING FALSE
    ENV NUMBA_CACHE_DIR /tmp

    This setup is essential in environments where direct internet access is restricted, and a web proxy must be used to access external websites and services.
    The settings make sure that all outbound connections from the Docker container adhere to the organization's network policies and use the specified proxy server for internet access.

    Docker Container Setup:

    Base Image: A Docker container is set up using an image from the internet, for example, nvidia/cuda for running medical imaging software.
    Proxy Configuration: The hospital uses a proxy server for all internet traffic. 
    In the Dockerfile, environment variables like http_proxy are set to http://proxy.hospital.org:3128. This configuration ensures that:
    All internet requests from the Docker container (running the imaging software) must go through proxy.hospital.org on port 3128.
    The proxy checks and filters these requests to ensure they are safe and authorized under the hospital's policies before allowing them through.
    This setup is crucial because it ensures that all software updates, data queries, and internet interactions are monitored and controlled, reducing the risk of cyber threats.

# set environment variables
    ENV DEBIAN_FRONTEND noninteractive
    ENV HDF5_USE_FILE_LOCKING FALSE
    ENV NUMBA_CACHE_DIR /tmp

    1. Setting DEBIAN_FRONTEND to noninteractive tells the system to automatically use default answers to any prompts during package installations or upgrades, or to use non-interactive interfaces where possible.
    2. Purpose: This variable relates to the use of HDF5 files, a format widely used for storing large quantities of scientific data. In a Docker or cloud environment, file locking can cause problems when multiple processes try to access the same file simultaneously, especially on filesystems that do not support locking.
    Value: Setting HDF5_USE_FILE_LOCKING to FALSE disables HDF5's file locking mechanism, which can be necessary to prevent errors in environments where file locking isn't supported or is problematic.
    3. Purpose: Numba is a just-in-time compiler for Python that speeds up the execution of numerical and scientific code. 
    Numba can cache compiled functions to disk to avoid recompiling them every time a program is run, which speeds up startup after the first execution.
    Value: Setting NUMBA_CACHE_DIR to /tmp specifies that the cache directory for storing these compiled functions is /tmp. 
    This is useful in a container environment, as it avoids using the container's overlay filesystem for cache, which can be slow and consume unnecessary space. 
    By directing the cache to /tmp, which is often a tmpfs (temporary file storage in RAM), it ensures faster access and automatically clears cache when the container is restarted or deleted.


# configure folder permission
    .dgl: This is not a standard Linux directory and might be specific to your application, possibly related to a library called Deep Graph Library.
    .local: This directory is typically used in a user's home directory in Unix-like systems to store user-specific application data and configuration files.
    Applications use this directory to separate user data from system-wide data stored in directories like /usr.
    .cache: This is commonly used to store cache files by various applications.
    These are files that help speed up application processes but are not critical to application functionality, meaning they can be deleted without loss of data.

# bash at /App
    You switch to the /App directory where your application’s files might be and set the container to open a Bash shell when it starts.
    This means when you run the container, it will start with a command line interface waiting for your commands.