# Simulation GEMC Installation

# ---------------------------------------------------------

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

# Install cvmfs

To add the CVMFS repository and install CVMFS run : 

```console
sudo apt install lsb-release
```
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

Create `/etc/cvmfs/default.local` with `sudo nano /etc/cvmfs/default.local` and write in :

```vim
CVMFS_QUOTA=10000
CVMFS_REPOSITORIES=oasis.opensciencegrid.org,singularity.opensciencegrid.org
CVMFS_HTTP_PROXY=DIRECT
```

Set up cvmfs 

```console
sudo cvmfs_config setup
```

Verify if the two folder are create : 
```console
cvmfs_config probe
```





# Install clas12software docker

Create folder : `sudo mkdir ~/mywork`

Download source and detectors from Viktoriya github : 
   ```console
   git clone https://github.com/Viktoriya89/source ~/mywork/
   ```
   ```console
   git clone https://github.com/Viktoriya89/detectors ~/mywork/
   ```
   
   ```console
   sudo docker run -it --rm v ~/mywork:/jlab/work/mywork jeffersonlab/gemc:devel bash
   ```
   
   Build gemc from source : `cd ~/mywork/source`
   ```console
   scons -j4 OPT=1
   ```

   ```console
   sudo docker run -it --rm -p 6080:6080 -v /cvmfs:/cvmfs -v ~/mywork:/jlab/work/mywork jeffersonlab/clas12software:production
   ```
   
   ``console
   sudo docker run -it --rm -v /cvmfs:/cvmfs jeffersonlab/clas12software:production bash
   ```
   
   pour quitter un docker en interactive crtl p + crtl q 
   


