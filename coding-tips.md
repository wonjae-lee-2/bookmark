# Coding Tips

[Back](index.md)

## Table of Contents

[Installation](#installation)

## Installation

### Shell

1. Update packages and create folders.

   ```Shell
   sudo yum update
   cd ~
   mkdir downloads github venv
   ```

### Python

1. Check the latest version of OpenSSL on <https://www.openssl.org/>.

2. Download and install OpenSSL.

   Python 3.10.x requires openssl 1.1.1 or higher. Amazon Linux 2 has openssl 1.1.1.

   ```Shell
   cd ~/downloads
   export OPENSSL_VERSION=3.0.2
   wget https://www.openssl.org/source/openssl-${OPENSSL_VERSION}.tar.gz
   tar -xf openssl-${OPENSSL_VERSION}.tar.gz
   cd openssl-${OPENSSL_VERSION}
   sudo yum install yum-utils
   sudo yum-builddep python3
   sudo yum install perl-IPC-Cmd
   sudo yum install perl-Test-Simple
   ./Configure --prefix=/opt/openssl-${OPENSSL_VERSION} --libdir=lib --openssldir=/opt/openssl-${OPENSSL_VERSION}/ssl '-Wl,-rpath,/opt/openssl-${OPENSSL_VERSION}/lib'
   make -j -s
   make test
   sudo make install
   /opt/openssl-${OPENSSL_VERSION}/bin/openssl version
   ```

4. Check the latest version of Python on <https://www.python.org/>.

5. Download and install Python.

   ```Shell
   cd ~/downloads
   export OPENSSL_VERSION=3.0.2
   export PYTHON_VERSION=3.10.4
   wget https://www.python.org/ftp/python/${PYTHON_VERSION}/Python-${PYTHON_VERSION}.tgz
   tar -xf Python-${PYTHON_VERSION}.tgz
   cd Python-${PYTHON_VERSION}
   ./configure --prefix=/opt/python-${PYTHON_VERSION} --enable-optimizations --with-lto --with-openssl=/opt/openssl-${OPENSSL_VERSION} --with-openssl-rpath=/opt/openssl-${OPENSSL_VERSION}/lib LDFLAGS=-Wl,-rpath,/opt/python-${PYTHON_VERSION}/lib
   make -j -s
   make test
   sudo make install
   /opt/python-${PYTHON_VERSION}/bin/python3.10 --version
   ```

6. Create a symlink to Python.

   ```Shell
   sudo ln -s /opt/python-${PYTHON_VERSION}/bin/python3.10 /usr/local/bin/python-${PYTHON_VERSION}
   # Delete obsolete symlinks to Python in /usr/local/bin
   ```

7. Create a virtual environment and install Jupyter Lab.

   ```Shell
   python-${PYTHON_VERSION} -m venv ~/venv/python-${PYTHON_VERSION}
   source ~/venv/python-${PYTHON_VERSION}/bin/activate
   pip install jupyterlab
   deactivate   
   ```

8. Update the VS Code settings file.

### Julia

1. Check the latest version of Julia on <https://julialang.org/>.

2. Download and install Julia.

   ```Shell
   cd ~/downloads
   export JULIA_VERSION=1.7.2
   wget https://julialang-s3.julialang.org/bin/linux/x64/1.7/julia-${JULIA_VERSION}-linux-x86_64.tar.gz
   sudo mkdir /opt/julia-${JULIA_VERSION}
   sudo tar -xf julia-${JULIA_VERSION}-linux-x86_64.tar.gz -C /opt/
   ```

3. Create a symlink to Julia.

   ```Shell
   sudo ln -s /opt/julia-${JULIA_VERSION}/bin/julia /usr/local/bin/julia-${JULIA_VERSION}
   # Delete obsolete symlinks to Julia in /usr/local/bin
   ```

4. Create a project environment.

   ```Shell
   julia-${JULIA_VERSION}
   # In Julia mode, enter ENV["JUPYTER"]="~/venv/python-${PYTHON_VERSION}/bin/jupyter". 
   # In package mode, enter "activate ~/venv/julia-${JULIA_VERSION}" and "add IJulia".
   ```
5. Update the IJulia kernel settings.

   ```Shell
   nano /home/ec2-user/.local/share/jupyter/kernels/julia-1.7/kernel.json
   # If the json file is not found, activate the python environment and type "jupyter kernelspec list" to find the folder.
   # Within "arvg", set --project to ~/venv/julia-${JULIA_VERSION}.
   ```

5. Update the VS Code settings file.

### R

1. Install and add the user to the docker group.

   ```Shell
   sudo yum install docker
   sudo groupadd docker
   sudo usermod -aG docker $USER
   sudo reboot
   ```

2. Start and test the docker

   ```Shell
   sudo systemctl start docker
   docker run --rm hello-world
   ```

3. Run the tidyverse container.

   ```Shell
   docker run --rm -p 8787:8787 --mount type=bind,src=/home/ec2-user/github,dst=/home/rstudio/github rocker/tidyverse:4.1.3
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
   make -j -s
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
   sudo adduser postgres
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
   # Type "ALTER USER postgres PASSWORD 'gloryvine';"
   logout
   ```

### Node.js

1. Download and install the Node.js binary.

   ```Shell
   cd ~/downloads
   export NODE_VERSION=16.14.2
   wget https://nodejs.org/dist/v${NODE_VERSION}/node-v${NODE_VERSION}-linux-x64.tar.xz
   sudo mkdir /opt/node-${NODE_VERSION}
   sudo tar -xf node-v${NODE_VERSION}-linux-x64.tar.xz --strip-components=1 -C /opt/node-${NODE_VERSION}
   ```

2. Add the Node.js directory to `$PATH`.

   ```Shell
   nano ~/.bash_profile
   # Add "/opt/node-${NODE_VERSION}/bin" to `$PATH`. Delete obsolete Node.js directories from `$PATH`.
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

### Git

1. Install Git.

   ```Shell
   sudo yum install git
   ```

2. Generate and copy a SSH key.

   ```Shell
   ssh-keygen -t ed25519
   nano ~/.ssh/id_ed25519.pub
   ```

3. Add the SSH key to GitHub.

4. Clone a repository.

   ```Shell
   cd ~/github
   git clone git@github.com:wonje-lee-2/islr2
   ```

### Rclone

1. Install Rclone.

   ```Shell
   sudo yum install rclone
   ```

2. Configure Rclone for OneDrive.

   ```Shell
   rclone config
   ```

3. Sync a folder to OneDrive.

   ```Shell
   cd ~/github/islr2
   rclone sync -i . remote:github/islr2
   ```

### Visual Studio Code

1. Install the following extensions on the local machine.

   - "Remote-SSH" from Microsoft
   - "Docker" from Microsoft
   - "Python" from Microsoft
   - "Jupyter" from Microsoft
   - "Julia" from julialang
   - "Rust" from the Rust Programming Language
   - "ESLint" from Microsoft
   - "Visual Studio IntelliCode" from Microsoft

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
   
