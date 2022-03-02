# Coding Tips

[Back](index.md)

## Table of Contents

[Installation](#installation)

## Installation

### Shell

1. Update packages and create folders

   ```Shell
   sudo yum update
   cd ~
   mkdir downloads github venv
   ```

### Python

1. Check the latest version of OpenSSL on <https://www.openssl.org//>.

2. Download and install OpenSSL.

   Python 3.10.x requires openssl 1.1.1 or higher. Amazon Linux 2 has openssl 1.1.1.

   ```Shell
   cd ~/downloads
   export OPENSSL_VERSION=3.0.1
   wget https://www.openssl.org/source/openssl-${OPENSSL_VERSION}.tar.gz
   tar -xf openssl-${OPENSSL_VERSION}.tar.gz
   cd openssl-${OPENSSL_VERSION}
   sudo yum install yum-utils
   sudo yum-builddep python3
   sudo yum install perl-IPC-Cmd
   sudo yum install perl-Test-Simple
   ./Configure --prefix=/opt/openssl/${OPENSSL_VERSION} --libdir=lib --openssldir=/opt/openssl/${OPENSSL_VERSION}/ssl '-Wl,-rpath,/opt/openssl/${OPENSSL_VERSION}/lib'
   make -j -s
   make test
   sudo make install
   /opt/openssl/${OPENSSL_VERSION}/bin/openssl version
   ```

4. Check the latest version of Python on <https://www.python.org/>.

5. Download and install Python.

   ```Shell
   cd ~/downloads
   export OPENSSL_VERSION=3.0.1
   export PYTHON_VERSION=3.10.2
   wget https://www.python.org/ftp/python/${PYTHON_VERSION}/Python-${PYTHON_VERSION}.tgz
   tar -xf Python-${PYTHON_VERSION}.tgz
   cd Python-${PYTHON_VERSION}
   ./configure --prefix=/opt/python/${PYTHON_VERSION} --enable-optimizations --with-lto --with-openssl=/opt/openssl/${OPENSSL_VERSION} --with-openssl-rpath=/opt/openssl/${OPENSSL_VERSION}/lib LDFLAGS=-Wl,-rpath,/opt/python/${PYTHON_VERSION}/lib
   make -j -s
   make test
   sudo make install
   /opt/python/${PYTHON_VERSION}/bin/python3.10 --version
   ```

6. Create a virtual environment

   ```Shell
   /opt/python/${PYTHON_VERSION}/bin/python3.10 -m venv ~/venv/python-${PYTHON_VERSION}
   source ~/venv/python-${PYTHON_VERSION}/bin/activate
   deactivate   
   ```

### PostgreSQL

1. Check the latest version of PostgreSQL on <https://www.postgresql.org/>

2. Download and install PostgreSQL

   ```Shell
   cd ~/downloads
   export PGSQL_VERSION=14.2
   wget https://ftp.postgresql.org/pub/source/v${PGSQL_VERSION}/postgresql-${PGSQL_VERSION}.tar.gz
   tar -xf postgresql-${PGSQL_VERSION}.tar.gz
   cd postgresql-${PGSQL_VERSION}
   ./configure --prefix=/opt/pgsql/${PGSQL_VERSION}
   make -j -s
   make check
   sudo make install
   ```

3. Create the user `postgres` and a folder for a databse cluster.

   ```Shell
   sudo adduser postgres -p gloryvine
   sudo mkdir /opt/pgsql/${PGSQL_VERSION}/data
   sudo chown postgres /opt/pgsql/${PGSQL_VERSION}/data
   ```

4. Log in as the user `postgres` and initialize a database cluster.

   ```Shell
   su - postgres
   export PGSQL_VERSION=14.2
   /opt/pgsql/${PGSQL_VERSION}/bin/initdb -D /opt/pgsql/${PGSQL_VERSION}/data
   ```

5. Update connection settings.

   ```Shell
   nano /opt/pgsql/${PGSQL_VERSION}/data/pg_hba.conf
   # Add "host all all all md5" to IPv4 local connections.
   nano /opt/pgsql/${PGSQL_VERSION}/data/postgresql.conf
   # Set "listen_addresses" to '*' under connection strings.
   ```

6. Start a PostgreSQL server.

   ```Shell
   /opt/pgsql/${PGSQL_VERSION}/bin/pg_ctl -D /opt/pgsql/${PGSQL_VERSION}/data -l logfile start
   ```

7. Add a password to the user `postgres` and log out.

   ```Shell
   /opt/pgsql/${PGSQL_VERSION}/bin/psql
   # Type "ALTER USER postgres PASSWORD 'gloryvine';"
   logout
   ```

### Julia

1. Check the latest version of Julia on <https://julialang.org/>

2. Download and install Julia.

   ```Shell
   cd ~/downloads
   export JULIA_VERSION=1.7.2
   wget https://julialang-s3.julialang.org/bin/linux/x64/1.7/julia-${JULIA_VERSION}-linux-x86_64.tar.gz
   sudo mkdir -p /opt/julia/${JULIA_VERSION}
   sudo tar -xf julia-${JULIA_VERSION}-linux-x86_64.tar.gz --strip-components=1 -C /opt/julia/${JULIA_VERSION}
   ```

3. Create a project environment.

   ```Shell
   /opt/julia/${JULIA_VERSION}/bin/julia
   # In the package mode, enter "activate ~/venv/julia-${JULIA_VERSION}" and "add DataFrames".
   ```

### Rust

1. Install Rust.

   ```Shell
   curl --proto '=https' --tlsv1.2 https://sh.rustup.rs -sSf | sh
   sudo reboot
   rustc --version
   ```

2. Update Rust.

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

### R

1. Install and add the user to the docker group.

   ```Shell
   sudo yum install docker
   sudo groupadd docker
   sudo usermod -aG docker $USER
   sudo reboot
   ```

2. Start the docker and run the tidyverse container.

   ```Shell
   sudo systemctl start docker
   docker run --rm -p 8787:8787 --mount type=bind,src=/home/ec2-user/github/unhcr,dst=/home/rstudio/unhcr rocker/tidyverse
   ```

### Visual Studio Code

1. Install the following extensions.

   - "Remote-SSH" from Microsoft
   - "Docker" from Microsoft
   - "Python" from Microsoft
   - "Jupyter" from Microsoft
   - "Julia" from julialang
   - "Rust" from the Rust Programming Language
   - "ESLint" from Microsoft
   - "Visual Studio IntelliCode" from Microsoft

3. Open the VS Code settings file.

   ```Shell
   nano ~/.vscode-server/data/Machine/settings.json
   ```

4. Add the following settings.

   ```JSON
   {
      "python.defaultInterpreterPath": "/home/ec2-user/venv/python-3.10.2/bin/python",
      "julia.executablePath": "/opt/julia/1.7.2/bin/julia",
      "julia.environmentPath": "/home/ec2-user/venv/julia-1.7.2/",
      "rust-client.rustupPath": "/home/ec2-user/.cargo/bin/rustup"
   }
   ```
   
