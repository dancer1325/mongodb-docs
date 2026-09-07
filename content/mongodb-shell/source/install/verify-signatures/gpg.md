# Verify Packages with GPG (Linux and macOS)

This page describes how to use GPG to verify Linux and macOS packages.

## Before you Begin

## Steps

**Import the MongoDB Shell public key**

```sh
curl https://pgp.mongodb.com/mongosh.asc | gpg --import

```

**Download the MongoDB Shell public signature**

To download the MongoDB Shell public signature, go to the [mongosh
Releases](https://github.com/mongodb-js/mongosh/releases) page
on GitHub and download the corresponding `.sig` file for your
version and variant.

For example, if you are running
`mongodb-mongosh_2.5.10_amd64.deb`, download
`mongodb-mongosh_2.5.10_amd64.deb.sig`

> **Note**
>
> Make sure that you select the correct version in the GitHub
> releases page when you download the signature.

**Verify the package**

```sh
gpg --verify <path_to_signature_file> <path_to_mongosh_executable>

```

If the package is signed by MongoDB, the command returns:

```sh
gpg: Signature made Mon Jan 22 10:22:53 2024 CET
gpg:                using RSA key AB1B92FFBE0D3740425DAD16A8130EC3F9F5F923
gpg: Good signature from "Mongosh Release Signing Key <packaging@mongodb.com>" [unknown]

```

If the package is signed but the signing key is not added to your
local `trustdb`, the command returns:

```sh
gpg: WARNING: This key is not certified with a trusted signature!
gpg:          There is no indication that the signature belongs to the owner.

```

If the package is not properly signed, the command returns an
error message:

```sh
gpg: Signature made Mon Jan 22 10:22:53 2024 CET
gpg:                using RSA key AB1B92FFBE0D3740425DAD16A8130EC3F9F5F923
gpg: BAD signature from "Mongosh Release Signing Key <packaging@mongodb.com>" [unknown]
```
