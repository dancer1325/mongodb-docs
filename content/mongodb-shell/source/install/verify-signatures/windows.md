# Verify Windows Packages

This page describes how to verify Windows `.exe` and `.msi`
packages.

## Before you Begin

## Steps

To verify the MongoDB Shell package on Windows, you can use one of these
methods:

- mongosh-verify-signatures-windows-command-line
- mongosh-verify-signatures-windows-check-properties

### Verify Packages with PowerShell

To verify Windows packages with PowerShell, run:

```sh
powershell Get-AuthenticodeSignature -FilePath <path_to_mongosh_exe_or_msi>

```

If the package is signed, the command returns:

```sh
SignerCertificate     Status     Path                               
-----------------     ------     ----
F2D7C28591847B...     Valid      <path_to_mongosh_exe_or_msi>

```

### Verify Packages by Checking Properties

**Open the properties for your MongoDB Shell package**

**Check the package's digital signatures**

In the properties window, open the Digital Signatures
tab.

If the package is properly signed, the Digital Signatures show
these properties:

- - Name of signer
  - Digest algorithm
  - Timestamp
- - MONGODB, INC.
  - sha256
  - \<Timestamp\>
