# Install mongosh

This section describes how to install `mongosh` on Windows
through the MSI installer.

## Prerequisites

## Supported Operating Systems

You can install MongoDB Shell on these Windows operating systems:

- Microsoft Windows Server 2016+
- Microsoft Windows 10+

## Procedure

> **Note**
>
> On Windows, `mongosh` preferences and configuration options
> are stored in the `%APPDATA%/mongodb/mongosh` directory.

This section describes how to install `mongosh` on Windows
through the .zip archive.

## Prerequisites

## Supported Operating Systems

You can install MongoDB Shell on these Windows operating systems:

- Microsoft Windows Server 2016+
- Microsoft Windows 10+

## Procedure

> **Note**
>
> On Windows, `mongosh` preferences and configuration options
> are stored in the `%APPDATA%/mongodb/mongosh` directory.

This section describes how to install `mongosh` on macOS through
the Homebrew package manager. To learn how to manually install
`mongosh` from an archive instead, select .zip file
from the Installation Method menu at the top of this
page.

> **Important**
>
> In versions of `mongosh` older than 2.5.8, if you're using
> `Node.js` versions ^20.19.5, ^22.18.0, or ^24.3.0, the tab
> autocomplete may execute code up to and including potentially
> destructive operations. For example,
> `db.collection.deleteMany({}).<tab>`.
>
> If you already have `mongosh` installed, to check your
> `mongosh` and Node.js versions, run:
>
> \<path-to-mongosh-binary\> --build-info
> {
> "version": "2.5.8",
> "distributionKind": "packaged",
> "buildArch": "x64",
> "buildPlatform": "linux",
> "buildTarget": "unknown",
> "buildTime": "2025-09-09T10:18:30.614Z",
> "gitVersion": "7b6bd25a97ce617f8d5bfa26c36db6d99dc92419",
> "nodeVersion": "v24.9.0",
> "opensslVersion": "3.6.0",
> "sharedOpenssl": true,
> "runtimeArch": "x64",
> "runtimePlatform": "darwin",
> "runtimeGlibcVersion": "N/A",
> "deps": {
> "nodeDriverVersion": "6.19.0",
> "kerberosVersion": "2.2.2"
> }
> }
> To upgrade to the latest `mongosh` and Node.js versions, see
> mdb-shell-upgrade.
>
> `mongosh` installed with Homebrew does not provide the same
> stability and feature completeness guarantees as the official
> builds. For example, Homebrew `mongosh` installations don't
> support \[automatic client-side field level encryption\](<https://www.mongodb.com/docs/manual/core/security-automatic-client-side-encryption/>).

## Prerequisites

### Homebrew Requirements

To view the complete list of system requirements for Homebrew,
see the
[Homebrew Website](https://docs.brew.sh/Installation).

## Supported Operating Systems

You can install MongoDB Shell on macOS 11 or greater (x64 or
ARM64).

## Procedure

To install `mongosh` with Homebrew:

This section describes how to install `mongosh` on macOS using a
downloaded `.zip` file.

## Supported Operating Systems

You can install MongoDB Shell on macOS 11 or greater (x64 or
ARM64).

## Procedure

To manually install `mongosh` using a downloaded `.zip`
file:

This section describes how to install `mongosh` on Ubuntu 24.04
(Noble).

## Supported Operating Systems

## Procedure

**Import the public key used by the package management system**

From a terminal, run the following command to import
the MongoDB public GPG key from
<https://www.mongodb.org/static/pgp/server-8.0.asc>:

```sh
wget -qO- https://www.mongodb.org/static/pgp/server-8.0.asc | sudo tee /etc/apt/trusted.gpg.d/server-8.0.asc

```

The previous command writes the GPG key to your
system's `/etc/apt/trusted.gpg.d` folder and
displays the key in your terminal. You do not need to
copy or save the key.

If you receive an error indicating that `gnupg` is
not installed, perform the following steps:

1.  Install `gnupg` and its required libraries using
    the following command:

```sh
sudo apt-get install gnupg
   
```

1.  Retry importing the key:

```sh
wget -qO- https://www.mongodb.org/static/pgp/server-8.0.asc | sudo tee /etc/apt/trusted.gpg.d/server-8.0.asc
```

**Create a list file for MongoDB**

Create the list file
`/etc/apt/sources.list.d/mongodb-org-8.2.list`
for your version of Ubuntu:

```sh
echo "deb [ arch=amd64,arm64 ] https://repo.mongodb.org/apt/ubuntu noble/mongodb-org/8.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-8.0.list
```

**Reload the local package database**

```sh
sudo apt-get update
```

**Install the mongosh package**

To install the latest stable version of `mongosh` with the included
OpenSSL libraries:

```sh
sudo apt-get install -y mongodb-mongosh

```

To install `mongosh` with your OpenSSL 1.1 libraries:

```sh
sudo apt-get install -y mongodb-mongosh-shared-openssl11

```

To install `mongosh` with your OpenSSL 3.0 libraries:

```sh
sudo apt-get install -y mongodb-mongosh-shared-openssl3
```

**Confirm that mongosh installed successfully**

To confirm that `mongosh` installed successfully,
run the following command:

```sh
mongosh --version

```

The command returns the version of `mongosh` you
installed.
This section describes how to install `mongosh` on Ubuntu 22.04
(Jammy).

## Supported Operating Systems

## Procedure

**Import the public key used by the package management system**

From a terminal, run the following command to import
the MongoDB public GPG key from
<https://www.mongodb.org/static/pgp/server-8.0.asc>:

```sh
wget -qO- https://www.mongodb.org/static/pgp/server-8.0.asc | sudo tee /etc/apt/trusted.gpg.d/server-8.0.asc

```

The previous command writes the GPG key to your
system's `/etc/apt/trusted.gpg.d` folder and
displays the key in your terminal. You do not need to
copy or save the key.

If you receive an error indicating that `gnupg` is
not installed, perform the following steps:

1.  Install `gnupg` and its required libraries using
    the following command:

```sh
sudo apt-get install gnupg
   
```

1.  Retry importing the key:

```sh
wget -qO- https://www.mongodb.org/static/pgp/server-8.0.asc | sudo tee /etc/apt/trusted.gpg.d/server-8.0.asc
```

**Create a list file for MongoDB**

Create the list file
`/etc/apt/sources.list.d/mongodb-org-8.2.list`
for your version of Ubuntu:

```sh
echo "deb [ arch=amd64,arm64 ] https://repo.mongodb.org/apt/ubuntu jammy/mongodb-org/8.2 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-8.2.list
```

**Reload the local package database**

```sh
sudo apt-get update
```

**Install the mongosh package**

To install the latest stable version of `mongosh` with the included
OpenSSL libraries:

```sh
sudo apt-get install -y mongodb-mongosh

```

To install `mongosh` with your OpenSSL 1.1 libraries:

```sh
sudo apt-get install -y mongodb-mongosh-shared-openssl11

```

To install `mongosh` with your OpenSSL 3.0 libraries:

```sh
sudo apt-get install -y mongodb-mongosh-shared-openssl3
```

**Confirm that mongosh installed successfully**

To confirm that `mongosh` installed successfully,
run the following command:

```sh
mongosh --version

```

The command returns the version of `mongosh` you
installed.
This section describes how to install `mongosh` on Ubuntu 20.04
(Focal).

## Supported Operating Systems

## Procedure

**Import the public key used by the package management system**

From a terminal, run the following command to import
the MongoDB public GPG key from
<https://www.mongodb.org/static/pgp/server-8.0.asc>:

```sh
wget -qO- https://www.mongodb.org/static/pgp/server-8.0.asc | sudo tee /etc/apt/trusted.gpg.d/server-8.0.asc

```

The previous command writes the GPG key to your
system's `/etc/apt/trusted.gpg.d` folder and
displays the key in your terminal. You do not need to
copy or save the key.

If you receive an error indicating that `gnupg` is
not installed, perform the following steps:

1.  Install `gnupg` and its required libraries using
    the following command:

```sh
sudo apt-get install gnupg
   
```

1.  Retry importing the key:

```sh
wget -qO- https://www.mongodb.org/static/pgp/server-8.0.asc | sudo tee /etc/apt/trusted.gpg.d/server-8.0.asc
```

**Create a list file for MongoDB**

Create the list file
`/etc/apt/sources.list.d/mongodb-org-8.2.list`
for your version of Ubuntu:

```sh
echo "deb [ arch=amd64,arm64 ] https://repo.mongodb.org/apt/ubuntu focal/mongodb-org/8.2 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-8.2.list
```

**Reload the local package database**

```sh
sudo apt-get update
```

**Install the mongosh package**

To install the latest stable version of `mongosh` with the included
OpenSSL libraries:

```sh
sudo apt-get install -y mongodb-mongosh

```

To install `mongosh` with your OpenSSL 1.1 libraries:

```sh
sudo apt-get install -y mongodb-mongosh-shared-openssl11

```

To install `mongosh` with your OpenSSL 3.0 libraries:

```sh
sudo apt-get install -y mongodb-mongosh-shared-openssl3
```

**Confirm that mongosh installed successfully**

To confirm that `mongosh` installed successfully,
run the following command:

```sh
mongosh --version

```

The command returns the version of `mongosh` you
installed.
This section describes how to install `mongosh` on Ubuntu 18.04
(Bionic).

> **Important**
>
> Support for Ubuntu 18.04 (Bionic) is deprecated.

## Supported Operating Systems

## Procedure

**Import the public key used by the package management system**

From a terminal, run the following command to import
the MongoDB public GPG key from
<https://www.mongodb.org/static/pgp/server-8.0.asc>:

```sh
wget -qO- https://www.mongodb.org/static/pgp/server-8.0.asc | sudo tee /etc/apt/trusted.gpg.d/server-8.0.asc

```

The previous command writes the GPG key to your
system's `/etc/apt/trusted.gpg.d` folder and
displays the key in your terminal. You do not need to
copy or save the key.

If you receive an error indicating that `gnupg` is
not installed, perform the following steps:

1.  Install `gnupg` and its required libraries using
    the following command:

```sh
sudo apt-get install gnupg
   
```

1.  Retry importing the key:

```sh
wget -qO- https://www.mongodb.org/static/pgp/server-8.0.asc | sudo tee /etc/apt/trusted.gpg.d/server-8.0.asc
```

**Create a list file for MongoDB**

Create the list file
`/etc/apt/sources.list.d/mongodb-org-8.2.list`
for your version of Ubuntu:

```sh
echo "deb [ arch=amd64,arm64 ] https://repo.mongodb.org/apt/ubuntu bionic/mongodb-org/8.2 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-8.2.list
```

**Reload the local package database**

```sh
sudo apt-get update
```

**Install the mongosh package**

To install the latest stable version of `mongosh` with the included
OpenSSL libraries:

```sh
sudo apt-get install -y mongodb-mongosh

```

To install `mongosh` with your OpenSSL 1.1 libraries:

```sh
sudo apt-get install -y mongodb-mongosh-shared-openssl11

```

To install `mongosh` with your OpenSSL 3.0 libraries:

```sh
sudo apt-get install -y mongodb-mongosh-shared-openssl3
```

**Confirm that mongosh installed successfully**

To confirm that `mongosh` installed successfully,
run the following command:

```sh
mongosh --version

```

The command returns the version of `mongosh` you
installed.
This section describes how to install `mongosh` on Debian 12
(Bookworm).

## Supported Operating Systems

MongoDB Shell is supported on Debian versions 11 and greater.

## Procedure

**Import the public key used by the package management system**

From a terminal, run the following command to import
the MongoDB public GPG key from
<https://www.mongodb.org/static/pgp/server-8.0.asc>:

```sh
wget -qO- https://www.mongodb.org/static/pgp/server-8.0.asc | sudo tee /etc/apt/trusted.gpg.d/server-8.0.asc

```

The previous command writes the GPG key to your
system's `/etc/apt/trusted.gpg.d` folder and
displays the key in your terminal. You do not need to
copy or save the key.

If you receive an error indicating that `gnupg` is
not installed, perform the following steps:

1.  Install `gnupg` and its required libraries using
    the following command:

```sh
sudo apt-get install gnupg
   
```

1.  Retry importing the key:

```sh
wget -qO- https://www.mongodb.org/static/pgp/server-8.0.asc | sudo tee /etc/apt/trusted.gpg.d/server-8.0.asc
```

**Create a list file for MongoDB**

Create the list file
`/etc/apt/sources.list.d/mongodb-org-8.2.list`
for your version of Debian:

```sh
echo "deb [ arch=amd64,arm64 ] https://repo.mongodb.org/apt/debian bookworm/mongodb-org/8.0 main" | sudo tee /etc/apt/sources.list.d/mongodb-org-8.0.list
```

**Reload the local package database**

```sh
sudo apt-get update
```

**Install the mongosh package**

- Starting in Debian 12 (Bookworm), the default  
  OpenSSL version is 3.0.

- In Debian 11 (Bullseye) and older versions, the  
  default OpenSSL version is 1.1.

To install the latest stable version of `mongosh` with the included
OpenSSL libraries:

```sh
sudo apt-get install -y mongodb-mongosh

```

To install `mongosh` with OpenSSL 3.0 libraries:

```sh
sudo apt-get install -y mongodb-mongosh-shared-openssl3

```

To install `mongosh` with OpenSSL 1.1 libraries:

```sh
sudo apt-get install -y mongodb-mongosh-shared-openssl11
```

**Confirm that mongosh installed successfully**

To confirm that `mongosh` installed successfully,
run the following command:

```sh
mongosh --version

```

The command returns the version of `mongosh` you
installed.
This section describes how to install `mongosh` on Debian 11
(Bullseye).

## Supported Operating Systems

MongoDB Shell is supported on Debian versions 11 and greater.

## Procedure

**Import the public key used by the package management system**

From a terminal, run the following command to import
the MongoDB public GPG key from
<https://www.mongodb.org/static/pgp/server-8.0.asc>:

```sh
wget -qO- https://www.mongodb.org/static/pgp/server-8.0.asc | sudo tee /etc/apt/trusted.gpg.d/server-8.0.asc

```

The previous command writes the GPG key to your
system's `/etc/apt/trusted.gpg.d` folder and
displays the key in your terminal. You do not need to
copy or save the key.

If you receive an error indicating that `gnupg` is
not installed, perform the following steps:

1.  Install `gnupg` and its required libraries using
    the following command:

```sh
sudo apt-get install gnupg
   
```

1.  Retry importing the key:

```sh
wget -qO- https://www.mongodb.org/static/pgp/server-8.0.asc | sudo tee /etc/apt/trusted.gpg.d/server-8.0.asc
```

**Create a list file for MongoDB**

Create the list file
`/etc/apt/sources.list.d/mongodb-org-8.2.list`
for your version of Debian:

```sh
echo "deb [ arch=amd64,arm64 ] https://repo.mongodb.org/apt/debian bullseye/mongodb-org/8.0 main" | sudo tee /etc/apt/sources.list.d/mongodb-org-8.0.list
   
```

**Reload the local package database**

```sh
sudo apt-get update
```

**Install the mongosh package**

- Starting in Debian 12 (Bookworm), the default  
  OpenSSL version is 3.0.

- In Debian 11 (Bullseye) and older versions, the  
  default OpenSSL version is 1.1.

To install the latest stable version of `mongosh` with the included
OpenSSL libraries:

```sh
sudo apt-get install -y mongodb-mongosh

```

To install `mongosh` with OpenSSL 3.0 libraries:

```sh
sudo apt-get install -y mongodb-mongosh-shared-openssl3

```

To install `mongosh` with OpenSSL 1.1 libraries:

```sh
sudo apt-get install -y mongodb-mongosh-shared-openssl11
```

**Confirm that mongosh installed successfully**

To confirm that `mongosh` installed successfully,
run the following command:

```sh
mongosh --version

```

The command returns the version of `mongosh` you
installed.
This section describes how to install `mongosh` on Red Hat
Enterprise Linux 9 or Oracle Linux 9 using the `yum` package
manager.

## Supported Operating Systems

`mongosh` is available as `yum` package for the
following RHEL (Red Hat Enterprise Linux) platforms:

- RHEL (Red Hat Enterprise Linux) 8+ (x64, ARM64, ppc64le,
  and s390x)

- Oracle Linux 8+ running the Red Hat Compatible Kernel (RHCK).

  MongoDB Shell does not support the Unbreakable Enterprise Kernel
  (UEK).

## Procedure

**Configure the package management system (yum)**

Create a
`/etc/yum.repos.d/mongodb-org-8.2.repo` file
with the following contents:

```none
[mongodb-org-8.2]
name=MongoDB Repository
baseurl=https://repo.mongodb.org/yum/redhat/9/mongodb-org/8.2/$basearch/
gpgcheck=1
enabled=1
gpgkey=https://www.mongodb.org/static/pgp/server-8.0.asc

```

You can also download the `.rpm` files directly from the
[MongoDB repository](https://repo.mongodb.org/yum/redhat/). Downloads are
organized in the following order:

1.  Red Hat or CentOS version (for example, `8`)
2.  MongoDB edition (for example, `mongodb-enterprise`)
3.  MongoDB \[release version\](<https://www.mongodb.com/docs/manual/reference/versioning/>)
    (for example, `8.2`)
4.  Architecture (for example, `x86_64`)

**Install mongosh**

To install the latest stable version of `mongosh` with the
included OpenSSL libraries:

```sh
sudo yum install -y mongodb-mongosh

```

To install `mongosh` with your OpenSSL 1.1 libraries:

```sh
sudo yum install -y mongodb-mongosh-shared-openssl11

```

To install `mongosh` with your OpenSSL 3.0 libraries:

```sh
sudo yum install -y mongodb-mongosh-shared-openssl3
```

This section describes how to install `mongosh` on Red Hat
Enterprise Linux 8 or Oracle Linux 8 using the `yum` package
manager.

## Supported Operating Systems

`mongosh` is available as `yum` package for the
following RHEL (Red Hat Enterprise Linux) platforms:

- RHEL (Red Hat Enterprise Linux) 8+ (x64, ARM64, ppc64le,
  and s390x)

- Oracle Linux 8+ running the Red Hat Compatible Kernel (RHCK).

  MongoDB Shell does not support the Unbreakable Enterprise Kernel
  (UEK).

## Procedure

**Configure the package management system (yum)**

Create a
`/etc/yum.repos.d/mongodb-org-8.2.repo` file
with the following contents:

```none
[mongodb-org-8.2]
name=MongoDB Repository
baseurl=https://repo.mongodb.org/yum/redhat/8/mongodb-org/8.2/$basearch/
gpgcheck=1
enabled=1
gpgkey=https://www.mongodb.org/static/pgp/server-8.0.asc

```

You can also download the `.rpm` files directly from the
[MongoDB repository](https://repo.mongodb.org/yum/redhat/). Downloads are
organized in the following order:

1.  Red Hat or CentOS version (for example, `8`)
2.  MongoDB edition (for example, `mongodb-enterprise`)
3.  MongoDB \[release version\](<https://www.mongodb.com/docs/manual/reference/versioning/>)
    (for example, `8.2`)
4.  Architecture (for example, `x86_64`)

**Install mongosh**

To install the latest stable version of `mongosh` with the
included OpenSSL libraries:

```sh
sudo yum install -y mongodb-mongosh

```

To install `mongosh` with your OpenSSL 1.1 libraries:

```sh
sudo yum install -y mongodb-mongosh-shared-openssl11

```

To install `mongosh` with your OpenSSL 3.0 libraries:

```sh
sudo yum install -y mongodb-mongosh-shared-openssl3
```

This section describes how to install `mongosh` on Amazon Linux
2023 using the `yum` package manager.

## Supported Operating Systems

`mongosh` is available as `yum` package for the
following Amazon Linux platforms:

- Amazon Linux 2023 (x64 and ARM64)
- Amazon Linux 2 (x64 and ARM64)

## Procedure

**Configure the package management system (yum)**

Create a
`/etc/yum.repos.d/mongodb-org-8.2.repo` file
with the following contents:

```none
[mongodb-org-8.2]
name=MongoDB Repository
baseurl=https://repo.mongodb.org/yum/amazon/2023/mongodb-org/8.2/$basearch/
gpgcheck=1
enabled=1
gpgkey=https://www.mongodb.org/static/pgp/server-8.0.asc 

```

You can also download the `.rpm` files directly from the
[MongoDB repository](https://repo.mongodb.org/yum/amazon/). Downloads are
organized in the following order:

1.  Amazon Linux version (for example, `2023`)
2.  MongoDB \[release version\](<https://www.mongodb.com/docs/manual/reference/versioning/>)
    (for example, `8.2`)
3.  Architecture (for example, `x86_64`)

**Install mongosh**

To install the latest stable version of `mongosh` with the
included OpenSSL libraries:

```sh
sudo yum install -y mongodb-mongosh

```

To install `mongosh` with your OpenSSL 1.1 libraries:

```sh
sudo yum install -y mongodb-mongosh-shared-openssl11

```

To install `mongosh` with your OpenSSL 3.0 libraries:

```sh
sudo yum install -y mongodb-mongosh-shared-openssl3
```

This section describes how to install `mongosh` on Amazon Linux
2 using the `yum` package manager.

## Supported Operating Systems

`mongosh` is available as `yum` package for the
following Amazon Linux platforms:

- Amazon Linux 2023 (x64 and ARM64)
- Amazon Linux 2 (x64 and ARM64)

## Procedure

**Configure the package management system (yum)**

Create a
`/etc/yum.repos.d/mongodb-org-8.2.repo` file
with the following contents:

```none
[mongodb-org-8.2]
name=MongoDB Repository
baseurl=https://repo.mongodb.org/yum/amazon/2/mongodb-org/8.2/$basearch/
gpgcheck=1
enabled=1
gpgkey=https://www.mongodb.org/static/pgp/server-8.0.asc 

```

You can also download the `.rpm` files directly from the
[MongoDB repository](https://repo.mongodb.org/yum/amazon/). Downloads are
organized in the following order:

1.  Amazon Linux version (for example, `2023`)
2.  MongoDB \[release version\](<https://www.mongodb.com/docs/manual/reference/versioning/>)
    (for example, `8.2`)
3.  Architecture (for example, `x86_64`)

**Install mongosh**

To install the latest stable version of `mongosh` with the
included OpenSSL libraries:

```sh
sudo yum install -y mongodb-mongosh

```

To install `mongosh` with your OpenSSL 1.1 libraries:

```sh
sudo yum install -y mongodb-mongosh-shared-openssl11

```

To install `mongosh` with your OpenSSL 3.0 libraries:

```sh
sudo yum install -y mongodb-mongosh-shared-openssl3
```

This section describes how to install `mongosh` on Linux
distributions using a `.tgz` archive.

## Supported Operating Systems

## Procedure

## Next Steps

After you successfully install `mongosh`, learn how to
connect to your MongoDB deployment.

MongoDB provides a programmatically accessible list of `mongosh`
[downloads](https://downloads.mongodb.com/compass/mongosh.json) that
can be accessed through your application.
