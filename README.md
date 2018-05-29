# BeeGFS Parallel File System
BeeGFS (formerly FhGFS) is the leading parallel cluster file system,
developed with a strong focus on performance and designed for very easy
installation and management.
If I/O intensive workloads are your problem, BeeGFS is the solution.

Homepage: https://www.beegfs.io

# Build Instructions

## Prerequisites
Before building BeeGFS, install the following dependency packages:

### Red Hat / CentOS
```
$ yum install libuuid-devel libibverbs-devel librdmacm-devel libattr-devel redhat-rpm-config \
  rpm-build xfsprogs-devel zlib-devel openssl-devel sqlite sqlite-devel ant gcc-c++ gcc \
  redhat-lsb-core java-devel unzip libcurl-devel elfutils-libelf-devel kernel-devel
```

The `elfutils-libelf-devel` and `kernel-devel` packages can be omitted if you don't intend to
build the client module.

The additional `devtoolset-7` package is also required,
which provides a newer compiler version. The installation steps are outlined here.
Please consult the documentation of your distribution for details.

  1. Install a package with repository for your system:
   - On CentOS, install package centos-release-scl available in CentOS repository:
     ```
     $ sudo yum install centos-release-scl
     ```
   - On RHEL, enable RHSCL repository for you system:
     ```
     $ sudo yum-config-manager --enable rhel-server-rhscl-7-rpms
     ```
  2. Install the collection:
     ```
     $ sudo yum install devtoolset-7
     ```

  3. Start using software collections:
     ```
     $ scl enable devtoolset-7 bash
     ```
  4. Follow the instructions below to build BeeGFS.

### Debian
On Debian or Ubuntu based systems run this command to install the required packages:
```
$ sudo apt install build-essential autoconf automake pkg-config devscripts debhelper \
  libtool libattr1-dev xfslibs-dev lsb-release kmod librdmacm-dev libibverbs-dev \
  default-jdk ant dh-systemd zlib1g-dev libssl-dev sqlite3 libsqlite3-dev \
  libcurl4-openssl-dev
```
Note: If you have an older Debian system you might have to install the
`module-init-tools` package instead of `kmod`.
You also have the choice between
the openssl, nss, or gnutls version of `libcurl-dev`. Choose the one you prefer.

## Building Packages
BeeGFS comes with a shell script that can build all BeeGFS packages for the
system on which it is executed.
The packages include all services, the client module and utilities.

Go to the `auto_package` subdirectory:
```
 $ cd auto_package
```

To build RPM packages, run
```
 $ ./make-rpms.sh
```

For DEB packages use this command:
```
 $ ./make-debs.sh
```
This will generate individual packages for search service (management, meta-data, storage)
as well as the client kernel module and administration tools.
In both cases you can append `-j <n>` to enable parallel execution, where
`<n>` is the number of processes to run at the same time.
Additional options are described in the help message of both scripts (`-h` option).

To generate individual packages instead of building the whole system,
run
```
$ make opentk_lib-all thirdparty common-all
```
to prepare common parts required by the services and tools.
The packages can then be built by running the `make-deb` or `make-rpm` script
in the corresponding sub-project folders. For example:
```
$ cd storage/build
$ ./make-deb
```

## Building without packaging
To build the complete project without generating any packages,
simply run
```
$ make
```

The sub-projects have individual make targets, for example `storage-all`,
`meta-all`, etc.

To speed things you can use the `-j` option of `make`.
Additionally, the build system supports `distcc`:
```
$ make DISTCC=distcc
```

# Setup Instructions
A detailed guide on how to configure a BeeGFS system can be found in
the BeeGFS wiki: https://www.beegfs.io/wiki/

# Share your thoughts
Of course, we are curious about what you are doing with the BeeGFS sources, so
don't forget to drop us a note...
