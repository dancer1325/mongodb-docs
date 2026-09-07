# {+csfle+}

## Introduction

{+csfle+} ({+csfle-abbrev+}) is a feature that enables you to encrypt data in your
application before you send it over the network to MongoDB. With {+csfle-abbrev+}
enabled, no MongoDB product has access to your data in an unencrypted form.

You can set up {+csfle-abbrev+} using the following mechanisms:

- Automatic Encryption: Enables you to perform encrypted read and
  write operations without having to add explicit calls to encrypt and decrypt
  fields.
- {+manual-enc-title+}: Enables you to perform encrypted read and write
  operations through your MongoDB driver's encryption library. You must
  specify the logic for encryption with this library throughout your
  application.

## Considerations

When implementing an application that uses {+csfle+}, consider the points listed in Security Considerations.

For limitations, see {+csfle-abbrev+} limitations.

### Compatibility

To learn which MongoDB server products and drivers support {+csfle-abbrev+}, see csfle-compatibility-reference.

## Features

To learn about the security benefits of {+csfle-abbrev+} for your
applications, see the csfle-features page.

## Installation

To learn what you must install to use {+csfle-abbrev+}, see
the csfle-install page.

## Quick Start

To start using {+csfle-abbrev+}, see the csfle-quick-start.

## Fundamentals

To learn how {+csfle-abbrev+} works and how to set it up, see the
csfle-fundamentals section.

The fundamentals section contains the following pages:

- csfle-fundamentals-automatic-encryption
- csfle-fundamentals-manual-encryption
- csfle-fundamentals-create-schema
- csfle-fundamentals-manage-keys
- csfle-reference-encryption-algorithms

## Tutorials

To learn how to perform specific tasks with {+csfle-abbrev+}, see the
csfle-tutorials section.

## Reference

To learn about encryption key management, read qe-reference-keys-key-vaults.

For more information about developing your {+csfle-abbrev+}-enabled applications,
see the csfle-reference section, which contains the following pages:

- csfle-reference-encryption-schemas
- csfle-reference-server-side-schema
- csfle-reference-automatic-encryption-supported-operations
- csfle-reference-mongo-client
- csfle-reference-encryption-components
- csfle-reference-decryption
- csfle-reference-cryptographic-primitives
- csfle-reference-mongocryptd
- csfle-reference-libmongocrypt
