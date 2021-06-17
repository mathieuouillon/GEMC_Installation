# Install Docker Engine on Ubuntu

## Prerequisites

### OS requirements

To install Docker Engine, you need the 64-bit version of one of these Ubuntu
versions:

- Ubuntu Hirsute 21.04
- Ubuntu Groovy 20.10
- Ubuntu Focal 20.04 (LTS)
- Ubuntu Bionic 18.04 (LTS)
- Ubuntu Xenial 16.04 (LTS)

Docker Engine is supported on `x86_64` (or `amd64`), `armhf`, and `arm64` architectures.

### Install using the repository

Before you install Docker Engine for the first time on a new host machine, you need
to set up the Docker repository. Afterward, you can install and update Docker
from the repository.

#### Set up the repository

1. Update the `apt` package index and install packages to allow `apt` to use a repository over HTTPS:

   ```console
   sudo apt update && sudo apt upgrade
   ```
   ```console
   sudo apt install apt-transport-https ca-certificates curl gnupg lsb-release
   ```

3. Add Docker's official GPG key:

   ```console
   curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
   ```

4. Use the following command to set up the **stable** repository. To add the
   **nightly** or **test** repository, add the word `nightly` or `test` (or both)
   after the word `stable` in the commands below.

   ```console
   echo "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
   ```

#### Install Docker Engine

1. Update the `apt` package index, and install the _latest version_ of Docker
   Engine and containerd, or go to the next step to install a specific version:

   ```console
   sudo apt update
   ```
   ```console
   sudo apt install docker-ce docker-ce-cli containerd.io
   ```

2. Verify that Docker Engine is installed correctly by running the `hello-world`
   image.

   ```console
   sudo docker run hello-world
   ```

This command downloads a test image and runs it in a container. When the
container runs, it prints an informational message and exits.

Docker Engine is installed and running. The `docker` group is created but no users
are added to it. You need to use `sudo` to run Docker commands.

## Manage Docker as a non-root user

The Docker daemon binds to a Unix socket instead of a TCP port. By default
that Unix socket is owned by the user `root` and other users can only access it
using `sudo`. The Docker daemon always runs as the `root` user.

If you don't want to preface the `docker` command with `sudo`, create a Unix
group called `docker` and add users to it. When the Docker daemon starts, it
creates a Unix socket accessible by members of the `docker` group.

> Warning
>
> The `docker` group grants privileges equivalent to the `root`
> user.

To create the `docker` group and add your user:

1.  Create the `docker` group.

    ```console
    $ sudo groupadd docker
    ```

2.  Add your user to the `docker` group.

    ```console
    $ sudo usermod -aG docker $USER
    ```

3.  Log out and log back in so that your group membership is re-evaluated.
    
    On Linux, you can also run the following command to activate the changes to groups:
    
    ```console 
    $ newgrp docker 
    ```

4.  Verify that you can run `docker` commands without `sudo`.

    ```console
    $ docker run hello-world
    ```

# Install cvmfs

## Getting the software 

To add the CVMFS repository and install CVMFS run : 

```console
wget https://ecsft.cern.ch/dist/cvmfs/cvmfs-release/cvmfs-release-latest_all.deb
```
```console
sudo dpkg -i cvmfs-release-latest_all.deb
```
```console
rm -f cvmfs-release-latest_all.deb
```
```console
sudo apt update  
```
```console
sudo apt install cvmfs
```

## Setting up the Software

1. Create default.local

   Create `/etc/cvmfs/default.local` with `sudo nano /etc/cvmfs/default.local` and write in :

   ```vim
   CVMFS_QUOTA=10000
   CVMFS_REPOSITORIES=oasis.opensciencegrid.org,singularity.opensciencegrid.org
   CVMFS_HTTP_PROXY=DIRECT
   ```

2. Configure AutoFS

   ```console
   sudo cvmfs_config setup
   ```

3. Verify the file system
   Check if CernVM-FS mounts the specified repositories by : 
   ```console
   cvmfs_config probe
   ```
   If the probe fails, try to restart autofs with `sudo systemctl restart autofs`

# Download `clas12software` docker

Create folder : `mkdir ~/mywork` and `cd ~/mywork`

```console
sudo docker run -it --rm -v ~/mywork:/jlab/work/mywork jeffersonlab/clas12software:production bash
```

For having an interactive windows :

```console
sudo docker run -it --rm -p 6080:6080 -v ~/mywork:/jlab/work/mywork jeffersonlab/clas12software:production bash
```

For quit interactive docker : `crtl p + crtl q`

# Generate ALERT geometry
 ```console 
git clone https://github.com/gemc/detectors 
```

```console 
cd detectors/clas12
```
```console 
git clone https://github.com/JeffersonLab/clas12-offline-software
```
```console 
cd clas12-offline-software
```
```console 
git checkout Alert
```
```console 
./build-coatjava.sh
```
```console 
cp coatjava/lib/clas/* ..
```
```console 
cd .. 
```
```console 
./clas12-offline-software/coatjava/bin/run-groovy alert/AHDC_geom/factory_ahdc.groovy --variation rga_fall2018 --runnumber 11
```
```console 
cp ahdc__* alert/AHDC_geom/
```
```console 
./clas12-offline-software/coatjava/bin/run-groovy alert/ATOF_geom/factory_atof.groovy --variation rga_fall2018 --runnumber 11
```
```console 
cp atof__* alert/ATOF_geom/
```