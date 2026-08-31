# Install or Update the Atlas CLI

Install the Atlas CLI to quickly provision and manage Atlas database deployments from the terminal. To verify packages before installation, see verify-packages.

## Install the Atlas CLI

Select one of the following installation methods and follow the steps to install the Atlas CLI. To check whether your operating system is compatible with the Atlas CLI, see compatibility-atlas-cli.

**Homebrew**

### Complete the Prerequisites

To install the Atlas CLI using Homebrew, you must:

1. Use a MacOS or Linux operating system.
2. Install [Homebrew](https://brew.sh/).
---

**Yum**

---

**Apt**

### Complete the Prerequisites

To install the Atlas CLI using Apt, you must install `gnupg` and `curl`:

```sh
sudo apt-get install gnupg curl
```

---

**Chocolatey**

### Complete the Prerequisites

To install the Atlas CLI using Chocolatey, you must do the following:

1. Ensure that your system meets the [requirements](https://docs.chocolatey.org/en-us/choco/setup#requirements) for installing Chocolatey.
2. Install Chocolatey using `cmd.exe` or `PowerShell.exe`. To learn more, see [Installing Chocolatey](https://docs.chocolatey.org/en-us/choco/setup#installing-chocolatey).
---

**Docker**

### Complete the Prerequisites

To install the Atlas CLI using Docker, install the [Docker engine](https://docs.docker.com/engine/install/) or [Docker desktop](https://docs.docker.com/desktop/).
---

**Download Binary**

---

### Follow These Steps

**Homebrew**

1. **Install the Atlas CLI and `mongosh`.**

   Invoke the following `brew` command to install both the Atlas CLI and `mongosh`:

   ```sh
   brew install mongodb-atlas
   ```

   > **Note:**
   > You can also use the `brew install mongodb-atlas-cli` command to install both the Atlas CLI and `mongosh`. You can't install the Atlas CLI alone on Homebrew.

*[Contenido incluido desde: /includes/steps-verify-atlas-cli.rst]*

---

**Yum**

1. **Configure `yum` for your edition of MongoDB.**

   **MongoDB Community Edition**

   Create a `/etc/yum.repos.d/mongodb-org-7.0.repo` file so that you can install Atlas CLI directly using `yum`. Replace `7.0` with your edition of MongoDB.

   **RHEL**

   ```text
   [mongodb-org-7.0]
   name=MongoDB Repository
   baseurl=https://repo.mongodb.org/yum/redhat/$releasever/mongodb-org/7.0/x86_64/
   gpgcheck=1
   enabled=1
   gpgkey=https://pgp.mongodb.com/server-7.0.asc
   ```

   ---

   **Amazon Linux 2023**

   ```text
   [mongodb-org-7.0]
   name=MongoDB Repository
   baseurl=https://repo.mongodb.org/yum/amazon/2023/mongodb-org/7.0/x86_64/
   gpgcheck=1
   enabled=1
   gpgkey=https://pgp.mongodb.com/server-7.0.asc
   ```

   ---

   ---

   **MongoDB Enterprise Edition**

   Create a `/etc/yum.repos.d/mongodb-enterprise-7.0.repo` file so that you can install Atlas CLI directly using `yum`. Replace `7.0` with your edition of MongoDB.

   **RHEL**

   ```text
   [mongodb-enterprise-7.0]
   name=MongoDB Repository
   baseurl=https://repo.mongodb.com/yum/redhat/$releasever/mongodb-enterprise/7.0/$basearch/
   gpgcheck=1
   enabled=1
   gpgkey=https://pgp.mongodb.com/server-7.0.asc
   ```

   ---

   **Amazon Linux 2023**

   ```text
   [mongodb-enterprise-7.0]
   name=MongoDB Enterprise Repository
   baseurl=https://repo.mongodb.com/yum/amazon/2023/mongodb-enterprise/7.0/$basearch/
   gpgcheck=1
   enabled=1
   gpgkey=https://pgp.mongodb.com/server-7.0.asc
   ```

   ---

   ---

1. **Install the Atlas CLI and `mongosh`.**

   Invoke the following `yum` command to install both the Atlas CLI and `mongosh`:

   ```sh
   sudo yum install -y mongodb-atlas
   ```

   If you don't want to install `mongosh`, invoke the following `yum` command instead to install the Atlas CLI only:

   ```sh
   sudo yum install -y mongodb-atlas-cli
   ```

*[Contenido incluido desde: /includes/steps-verify-atlas-cli.rst]*

---

**Apt**

1. **Import the public key used by `apt`.**

   From a terminal, issue the following command to import the MongoDB public GPG Key from `https://pgp.mongodb.com/server-7.0.asc`. Replace `7.0` with your edition of MongoDB.

   ```sh
   curl -fsSL https://pgp.mongodb.com/server-7.0.asc | \
      sudo gpg -o /usr/share/keyrings/mongodb-server-7.0.gpg \
      --dearmor
   ```

   A successful command returns an `OK`.

1. **Create a list file for your edition of MongoDB.**

   **MongoDB Community Edition**

   Select Ubuntu or Debian, then select your version.

   **Ubuntu**

   Create the list file `/etc/apt/sources.list.d/mongodb-org-7.0.list` for your version of Ubuntu. Replace `7.0` with your edition of MongoDB.

   **Ubuntu 22.04 (Jammy)**

   ```sh
   echo "deb [ arch=amd64,arm64 signed-by=/usr/share/keyrings/mongodb-server-7.0.gpg ] https://repo.mongodb.org/apt/ubuntu jammy/mongodb-org/7.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-7.0.list
   ```

   ---

   **Ubuntu 20.04 (Focal)**

   ```sh
   echo "deb [ arch=amd64,arm64 signed-by=/usr/share/keyrings/mongodb-server-7.0.gpg ] https://repo.mongodb.org/apt/ubuntu focal/mongodb-org/7.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-7.0.list
   ```

   ---

   **Ubuntu 18.04 (Bionic)**

   ```sh
   echo "deb [ arch=amd64,arm64 signed-by=/usr/share/keyrings/mongodb-server-7.0.gpg ] https://repo.mongodb.org/apt/ubuntu bionic/mongodb-org/7.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-7.0.list
   ```

   ---

   ---

   **Debian**

   Create the list file `/etc/apt/sources.list.d/mongodb-org-7.0.list` for your version of Debian. Replace `7.0` with your edition of MongoDB.

   **Debian 12 (Bookworm)**

   ```sh
   echo "deb http://repo.mongodb.org/apt/debian bookworm/mongodb-org/7.0 main" | sudo tee /etc/apt/sources.list.d/mongodb-org-7.0.list
   ```

   ---

   **Debian 11 (Bullseye)**

   ```sh
   echo "deb http://repo.mongodb.org/apt/debian bullseye/mongodb-org/7.0 main" | sudo tee /etc/apt/sources.list.d/mongodb-org-7.0.list
   ```

   ---

   ---

   ---

   **MongoDB Enterprise Edition**

   Select Ubuntu or Debian, then select your version.

   **Ubuntu**

   Create a `/etc/apt/sources.list.d/mongodb-enterprise.list` file for your version of Ubuntu. Replace `7.0` with your edition of MongoDB.

   **Ubuntu 22.04 (Jammy)**

   ```sh
   echo "deb [ arch=amd64,arm64 signed-by=/usr/share/keyrings/mongodb-server-7.0.gpg ] https://repo.mongodb.com/apt/ubuntu jammy/mongodb-enterprise/7.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-enterprise.list
   ```

   ---

   **Ubuntu 20.04 (Focal)**

   ```sh
   echo "deb [ arch=amd64,arm64 signed-by=/usr/share/keyrings/mongodb-server-7.0.gpg ] https://repo.mongodb.com/apt/ubuntu focal/mongodb-enterprise/7.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-enterprise.list
   ```

   ---

   **Ubuntu 18.04 (Bionic)**

   ```sh
   echo "deb [ arch=amd64,arm64 signed-by=/usr/share/keyrings/mongodb-server-7.0.gpg ] https://repo.mongodb.com/apt/ubuntu bionic/mongodb-enterprise/7.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-enterprise.list
   ```

   ---

   ---

   **Debian**

   Create a `/etc/apt/sources.list.d/mongodb-enterprise.list` file for your version of Debian. Replace `7.0` with your edition of MongoDB.

   **Debian 11 (Bullseye)**

   ```sh
   echo "deb http://repo.mongodb.com/apt/debian bullseye/mongodb-enterprise/7.0 main" | sudo tee /etc/apt/sources.list.d/mongodb-enterprise.list
   ```

   ---

   ---

   ---

1. **Refresh the package database.**

   Invoke the following `apt` command:

   ```sh
   sudo apt-get update
   ```

1. **Install the Atlas CLI and `mongosh`.**

   Invoke the following `apt` command to install both the Atlas CLI and `mongosh`:

   ```sh
   sudo apt-get install -y mongodb-atlas
   ```

   If you don't want to install `mongosh`, invoke the following `apt` command instead to install the Atlas CLI only:

   ```sh
   sudo apt-get install -y mongodb-atlas-cli
   ```

*[Contenido incluido desde: /includes/steps-verify-atlas-cli.rst]*

---

**Chocolatey**

1. **Install the Atlas CLI.**

   ```shell
   choco install mongodb-atlas
   ```

1. **When prompted, enter `A` to confirm installation.**

1. **Close and reopen your terminal after the installation to see the changes in your path.**

*[Contenido incluido desde: /includes/steps-verify-atlas-cli.rst]*

---

**Docker**

To pull the latest [Atlas CLI Docker image](https://hub.docker.com/repository/docker/mongodb/atlas/general), run the following command:

```
docker pull mongodb/atlas
```

If you run `docker pull mongodb/atlas` without specifying a version tag, Docker automatically pulls the latest version of the Docker image (`mongodb/atlas:latest`). To pull a specific version of the Docker image, run the following command, replacing `<tag>` with the version tag:

```
docker pull mongodb/atlas:<tag>
```

To learn how to run Atlas CLI commands with Docker after you pull the Docker image, see atlas-cli-docker.
---

**Download Binary**

1. **Install the Atlas CLI.**

   1. Download and extract the correct binary for your operating system:

      *[Contenido incluido desde: /includes/list-table-atlas-cli-binary-download.rst]*

      > **Note:**
      > Replace or remove any existing MongoDB CLI binaries to prevent conflicts between versions.

   2. Run the executable file.

      1. **(Optional) Add Atlas CLI to your `PATH`.**

         You can run the binary from any directory if you do one of the following:

         1. Add the location of the executable to your `PATH`.
         2. Move the executable to a directory in your `PATH`.

         You can accomplish this in several ways, depending on your personal settings and environment. Consult the documentation for your shell and operating system for more examples.

         > **Example:**
         > In the following example, the user downloads and extracts a binary for the MongoDB CLI to the `/atlascli_1.50.1-macOS_x86_64` directory. The user then moves the executable file to a directory already in their `PATH`:
         >
         > ```sh
         > cd atlascli_1.50.1-macOS_x86_64
         > mv atlas /usr/local/bin
         > ```
         >

*[Contenido incluido desde: /includes/steps-verify-atlas-cli.rst]*

---

## Update the Atlas CLI

To update the Atlas CLI, follow the procedure that corresponds with the method you used to install the Atlas CLI:

**Homebrew**

### Follow These Steps

1. **Update the Atlas CLI.**

   If you installed the Atlas CLI and `mongosh` together using the `mongodb-atlas` package, invoke the following `brew` command:

   ```sh
   brew update
   brew upgrade mongodb-atlas
   ```

   If you installed the Atlas CLI and `mongosh` together using the `mongodb-atlas-cli` package, invoke the following `brew` command:

   ```sh
   brew update
   brew upgrade mongodb-atlas-cli
   ```

*[Contenido incluido desde: /includes/steps-verify-update-atlas-cli.rst]*

---

**Yum**

### Follow These Steps

1. **Update the Atlas CLI.**

   If you installed the Atlas CLI and `mongosh` together using the `mongodb-atlas` package, invoke the following `yum` command:

   ```sh
   yum update mongodb-atlas
   ```

   If you installed the Atlas CLI only using the `mongodb-atlas-cli` package, invoke the following `yum` command:

   ```sh
   yum update mongodb-atlas-cli
   ```

*[Contenido incluido desde: /includes/steps-verify-update-atlas-cli.rst]*

---

**Apt**

### Follow These Steps

1. **Update the Atlas CLI.**

   If you installed the Atlas CLI and `mongosh` together using the `mongodb-atlas` package, invoke the following `apt` command:

   ```sh
   sudo apt-get install --only-upgrade mongodb-atlas
   ```

   If you installed the Atlas CLI only using the `mongodb-atlas-cli` package, invoke the following `apt` command:

   ```sh
   sudo apt-get install --only-upgrade mongodb-atlas-cli
   ```

*[Contenido incluido desde: /includes/steps-verify-update-atlas-cli.rst]*

---

**Chocolatey**

### Follow These Steps

1. **Install the Atlas CLI.**

   ```shell
   choco upgrade mongodb-atlas
   ```

*[Contenido incluido desde: /includes/steps-verify-update-atlas-cli.rst]*

---

**Download Binary**

### Follow These Steps

1. **Update the Atlas CLI.**

   1. Remove any existing Atlas CLI binaries to prevent version conflicts.
   2. Download and extract the correct binary for your operating system:

      *[Contenido incluido desde: /includes/list-table-atlas-cli-binary-download.rst]*

   3. Run the executable file.

*[Contenido incluido desde: /includes/steps-verify-update-atlas-cli.rst]*

---

## Take the Next Steps

connect-atlas-cli to start using the Atlas CLI commands.

- [Check Compatibility](/compatibility)
- [Verify Packages](/verify-packages)
