# Verify RPM Packages (RHEL)

This page describes how to verify `.rpm` packages on RHEL operating
systems.

## Before you Begin

## Steps

**Import the MongoDB Shell public key in gpg and rpm**

```sh
curl https://pgp.mongodb.com/mongosh.asc | gpg --import

rpm --import https://pgp.mongodb.com/mongosh.asc

```

**Verify the rpm file**

```sh
rpm --checksig <path_to_mongosh_rpm_file>

```

If the file is signed, the command returns:

```sh
<path_to_mongosh_rpm_file> digests signatures OK
```
