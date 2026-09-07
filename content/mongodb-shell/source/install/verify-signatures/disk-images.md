# Verify Packages with Disk Image Verification (macOS)

This page describes how to verify `.dmg` packages on macOS.

## Before you Begin

## Steps

To verify the MongoDB Shell package, run:

```sh
codesign -dv --verbose=4 <path_to_mongosh_executable>

```

If the package is signed by MongoDB, the output includes the following
information:

```sh
Authority=Developer ID Application: MongoDB, Inc. (4XWMY46275)
Authority=Developer ID Certification Authority
Authority=Apple Root CA
```
