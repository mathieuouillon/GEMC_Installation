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

   ```bash
   $ sudo apt update & sudo apt upgrade
   ```
   ```bash
   $ sudo apt install apt-transport-https ca-certificates curl gnupg lsb-release
   ```

3. Add Docker's official GPG key:
   
   ```bash
   $ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
   ```

4. Use the following command to set up the **stable** repository. To add the
   **nightly** or **test** repository, add the word `nightly` or `test` (or both)
   after the word `stable` in the commands below.
   
   ```bash
   $ echo "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
   ```

#### Install Docker Engine

1. Update the `apt` package index, and install the _latest version_ of Docker
   Engine and containerd, or go to the next step to install a specific version:
   
   ```bash
   $ sudo apt update
   ```
   ```bash
   $ sudo apt install docker-ce docker-ce-cli containerd.io
   ```

2. Verify that Docker Engine is installed correctly by running the `hello-world`
   image.

   ```bash
   $ sudo docker run hello-world
   ```

This command downloads a test image and runs it in a container. When the
container runs, it prints an informational message and exits.

Docker Engine is installed and running. The `docker` group is created but no users
are added to it. You need to use `sudo` to run Docker commands.

# Install cvmfs

To add the CVMFS repository and install CVMFS run : 

```bash
$ sudo apt install lsb-release

$ wget https://ecsft.cern.ch/dist/cvmfs/cvmfs-release/cvmfs-release-latest_all.deb

$ sudo dpkg -i cvmfs-release-latest_all.deb

$ rm -f cvmfs-release-latest_all.deb

$ sudo apt update  

$ sudo apt install cvmfs
```

Set up cvmfs 

```bash
sudo cvmfs_config setup
```

Create `/etc/cvmfs/default.local`  and write in :

```vim
CVMFS_QUOTA=10000
CVMFS_REPOSITORIES=oasis.opensciencegrid.org, singularity.opensciencegrid.org
CVMFS_HTTP_PROXY=DIRECT
```

Create these two directories 

```bash
$ sudo mkdir /cvmfs/singularity.openscience.org
$ sudo mkdir /cvmfs/oasis.openscience.org
```

Mount these two directories 

```bash
sudo mount -t cvmfs singularity.opensciencegrid.org /cvmfs/singularity.opensciencegrid.org
sudo mount -t cvmfs oasis.opensciencegrid.org /cvmfs/oasis.opensciencegrid.org
```

# Install clas12software docker
