# Coding Tips

[Back](index.md)

## Table of Contents

[Installation](#installation)

## Installation

### Shell

1. Update packages and create folders.

   ```Shell
   sudo apt update
   sudo apt upgrade
   sudo apt autoremove
   mkdir ~/downloads ~/github ~/venv
   ```

### Git

1. Install Git.

   ```Shell
   sudo apt install git
   ```

2. Generate and copy a SSH key.

   ```Shell
   ssh-keygen -t ed25519
   cat ~/.ssh/id_ed25519.pub
   ```

3. Add the SSH key to GitHub.

4. Clone a repository.

   ```Shell
   cd ~/github
   git clone git@github.com:wonjae-lee-2/docker
   ```

### Python

1. Check the latest version of Python on <https://www.python.org/>.

2. Install build dependencies. # https://devguide.python.org/setup/#install-dependencies

   ```Shell
   sudo apt install \
      build-essential \
      gdb \
      lcov \
      pkg-config \
      libbz2-dev \
      libffi-dev \
      libgdbm-dev \
      libgdbm-compat-dev \
      liblzma-dev \
      libncurses-dev \
      libreadline-dev \
      libsqlite3-dev \
      libssl-dev \
      lzma \
      lzma-dev \
      tk-dev \
      uuid-dev \
      zlib1g-dev
   ```

3. Download and install Python.

   ```Shell
   cd ~/downloads
   export PYTHON_VERSION=3.10.4
   wget https://www.python.org/ftp/python/${PYTHON_VERSION}/Python-${PYTHON_VERSION}.tgz
   tar -xf Python-${PYTHON_VERSION}.tgz
   cd Python-${PYTHON_VERSION}
   ./configure --prefix=/opt/python-${PYTHON_VERSION} --enable-optimizations --with-lto LDFLAGS=-Wl,-rpath,/opt/python-${PYTHON_VERSION}/lib
   make -j -s
   make test
   sudo make install
   /opt/python-${PYTHON_VERSION}/bin/python3.10 --version
   ```

3. Create a symlink to Python.

   ```Shell
   sudo ln -s /opt/python-${PYTHON_VERSION}/bin/python3.10 /usr/local/bin/python-${PYTHON_VERSION}
   # Delete obsolete symlinks to Python in /usr/local/bin
   ```

4. Create a virtual environment and install packages.

   ```Shell
   python-${PYTHON_VERSION} -m venv ~/venv/python-${PYTHON_VERSION}
   . ~/venv/python-${PYTHON_VERSION}/bin/activate
   pip install -U -r ~/github/docker/python/requirements.txt
   deactivate   
   ```

5. Update the VS Code settings file.

### Julia

1. Check the latest version of Julia on <https://julialang.org/>.

2. Download and install Julia.

   ```Shell
   cd ~/downloads
   export JULIA_VERSION_SHORT=1.7
   export JULIA_VERSION_LONG=1.7.2
   wget https://julialang-s3.julialang.org/bin/linux/x64/${JULIA_VERSION_SHORT}/julia-${JULIA_VERSION_LONG}-linux-x86_64.tar.gz
   sudo mkdir /opt/julia-${JULIA_VERSION_LONG}
   sudo tar -xf julia-${JULIA_VERSION_LONG}-linux-x86_64.tar.gz -C /opt/
   ```

3. Create a symlink to Julia.

   ```Shell
   sudo ln -s /opt/julia-${JULIA_VERSION_LONG}/bin/julia /usr/local/bin/julia-${JULIA_VERSION_LONG}
   # Delete obsolete symlinks to Julia in /usr/local/bin
   ```

4. Create a project and install packages.

   ```Shell
   julia-${JULIA_VERSION_LONG} --project=~/venv/julia-${JULIA_VERSION_LONG} ~/github/docker/julia/requirements.jl
   ```

5. Update the VS Code settings file.

### R

1. Check the latest version of R on <https://www.r-project.org/>.

2. Install build dependencies. # https://cran.r-project.org/doc/manuals/r-release/R-admin.html#Useful-libraries-and-programs

   ```Shell
   sudo apt install \
      gfortran \
      libbz2-dev \
      libcairo2-dev \
      fontconfig \
      libfreetype-dev \
      libfribidi-dev \
      libglib2.0-dev \
      libharfbuzz-dev \
      libx11-dev \
      libxext-dev \
      libxt-dev \
      libcurl4-openssl-dev \
      libicu-dev \
      libjpeg-dev \
      libpng-dev \
      libtiff-dev \
      libtirpc-dev \
      libcrypt-dev \
      libncurses-dev \
      libpango1.0-dev \
      libpkgconf-dev \
      libpcre2-dev \
      libreadline-dev \
      tcl-dev \
      tk-dev \
      liblzma-dev \
      zlib1g-dev \
      libblas-dev \
      liblapack-dev \
      gdebi-core
   ```
 
 3. Download and install R.
 
   ```Shell
   cd ~/downloads
   export R_VERSION_SHORT=4
   export R_VERSION_LONG=4.2.0
   wget https://cran.r-project.org/src/base/R-${R_VERSION_SHORT}/R-${R_VERSION_LONG}.tar.gz
   tar -xf R-${R_VERSION_LONG}.tar.gz
   cd R-${R_VERSION_LONG}
   ./configure \
      --prefix=/opt/r-${R_VERSION_LONG} \
      --enable-memory-profiling \
      --enable-R-shlib \
      --with-blas \
      --with-lapack
   make -j -s
   sudo make install
   /opt/r-${R_VERSION_LONG}/bin/R --version
   ```

4. Create a symlink to R.

   ```Shell
   sudo ln -s /opt/r-${R_VERSION_LONG}/bin/R /usr/local/bin/R
   sudo ln -s /opt/r-${R_VERSION_LONG}/bin/Rscript /usr/local/bin/Rscript
   # Delete obsolete symlinks to R and Rscript in /usr/local/bin
   ```

5. Download and install RStudio Server. (Ubuntu 22.04 daily builds from https://dailies.rstudio.com/)

   ```Shell
   cd ~/downloads
   wget https://s3.amazonaws.com/rstudio-ide-build/server/jammy/amd64/rstudio-server-2022.06.0-daily-343-amd64.deb
   sudo gdebi rstudio-server-2022.06.0-daily-343-amd64.deb
   ```

6. Set a password for the default user.

   ```Shell
   sudo passwd ubuntu
   ```

7. Install package dependencies.

   ```Shell
   sudo apt install \
      libcurl4-openssl-dev \
      libxml2-dev \
      libpq-dev \
      libssl-dev
   ```

7. Type `~C` and then `-L 8787:localhost:8787` to request local forward.

8. Type `localhost:8787` in the browser and run `~/github/docker/r/requirements.r` in Rstudio Server.

9. Type `rstudio-server stop` to stop Rstudio Server.

### Node.js

1. Download and install the Node.js binary.

   ```Shell
   cd ~/downloads
   export NODE_VERSION=16.15.0
   wget https://nodejs.org/dist/v${NODE_VERSION}/node-v${NODE_VERSION}-linux-x64.tar.xz
   sudo mkdir /opt/node-${NODE_VERSION}
   sudo tar -xf node-v${NODE_VERSION}-linux-x64.tar.xz --strip-components=1 -C /opt/node-${NODE_VERSION}
   ```

2. Add the Node.js directory to `$PATH`.

   ```Shell
   nano ~/.profile
   # Add PATH="/opt/node-${NODE_VERSION}/bin:$PATH". Delete obsolete Node.js directories from .profile.
   ```

3. Log out of EC2 and then SSH into EC2 again.

4. Check the version of Node.js and npm.

   ```Shell
   node -v
   npm -v
   ```

### Rust

1. Install Rust.

   ```Shell
   curl --proto '=https' --tlsv1.2 https://sh.rustup.rs -sSf | sh
   ```

2. Log out of EC2 and then SSH into EC2 again.

3. Check the version of Rust.

   ```Shell
   rustc --version
   ```

4. Update Rust.

   ```Shell
   rustup check
   rustup update
   ```

### PostgreSQL

1. Check the latest version of PostgreSQL on <https://www.postgresql.org/>.

2. Download and install PostgreSQL.

   ```Shell
   cd ~/downloads
   export PGSQL_VERSION=14.2
   wget https://ftp.postgresql.org/pub/source/v${PGSQL_VERSION}/postgresql-${PGSQL_VERSION}.tar.gz
   tar -xf postgresql-${PGSQL_VERSION}.tar.gz
   cd postgresql-${PGSQL_VERSION}
   ./configure --prefix=/opt/pgsql-${PGSQL_VERSION}
   make -s
   make check
   sudo make install
   ```

3. Create a symlink to PostgreSQL.

   ```Shell
   sudo ln -s /opt/pgsql-${PGSQL_VERSION}/bin/psql /usr/local/bin/psql-${PGSQL_VERSION}
   # Delete obsolete symlinks to PostgreSQL in /usr/local/bin
   ```

4. Create the user `postgres` and a folder for a databse cluster.

   ```Shell
   sudo useradd -m postgres
   sudo passwd postgres
   sudo mkdir /opt/pgsql-${PGSQL_VERSION}/data
   sudo chown postgres /opt/pgsql-${PGSQL_VERSION}/data
   ```

5. Log in as the user `postgres` and initialize a database cluster.

   ```Shell
   su - postgres
   export PGSQL_VERSION=14.2
   /opt/pgsql-${PGSQL_VERSION}/bin/initdb -D /opt/pgsql-${PGSQL_VERSION}/data
   ```

6. Update connection settings.

   ```Shell
   nano /opt/pgsql-${PGSQL_VERSION}/data/pg_hba.conf
   # Add "host all all all md5" to IPv4 local connections.
   nano /opt/pgsql-${PGSQL_VERSION}/data/postgresql.conf
   # Set "listen_addresses" to '*' under connection strings.
   ```

7. Start a PostgreSQL server.

   ```Shell
   /opt/pgsql-${PGSQL_VERSION}/bin/pg_ctl -D /opt/pgsql-${PGSQL_VERSION}/data -l logfile start
   ```

8. Add a password to the user `postgres` and log out.

   ```Shell
   psql-${PGSQL_VERSION}
   # Type "ALTER USER postgres PASSWORD '${PASSWORD}';"
   logout
   ```

### Rclone

1. Install Rclone.

   ```Shell
   sudo apt install rclone
   ```

2. Configure Rclone for OneDrive.

   ```Shell
   rclone config
   # Set the name of the remote as ondrive.
   ```

3. Sync a folder to OneDrive.

   ```Shell
   rclone sync -P ~/github/docker onedrive:backup/github/docker
   ```

4. Install AWS CLI and configure it with credentials in the personal vault.

   ```Shell
   sudo apt install awscli
   aws configure
   ```

5. Configure Rclone for S3.

   ```Shell
   rclone config
   # Set the name of the remote as s3. Choose 'Get AWS credentials from the environment'.
   ```

6. List buckets in S3.

   ```Shell
   rclone lsd s3:
   ```

### Docker

1. Install Docker and add the user to the docker group.

   ```Shell
   sudo apt install docker.io
   sudo groupadd docker
   sudo usermod -aG docker $USER
   sudo reboot
   ```

2. Start and test the docker

   ```Shell
   sudo systemctl start docker
   docker run --rm hello-world
   ```

### Visual Studio Code

1. Install the following extensions on the local machine.

   - "Docker" from Microsoft
   - "ESLint" from Microsoft
   - "IntelliCode" from Microsoft
   - "Julia" from julialang
   - "Jupyter" from Microsoft
   - "Python" from Microsoft
   - "Remote-Containers" from Microsoft
   - "Remote-SSH" from Microsoft
   - "Rust" from The Rust Programming Language
   
2. Connect to the virtual machine and install the extensions.

3. Open the VS Code settings file.

   ```Shell
   nano ~/.vscode-server/data/Machine/settings.json
   ```

4. Add the following settings.

   ```JSON
   {
      "python.defaultInterpreterPath": "/home/ec2-user/venv/python-${PYTHON_VERSION}/bin/python",
      "julia.executablePath": "/opt/julia-${JULIA_VERSION}/bin/julia",
      "julia.environmentPath": "/home/ec2-user/venv/julia-${JULIA_VERSION}/",
      "rust-client.rustupPath": "/home/ec2-user/.cargo/bin/rustup"
   }
   ```
   
