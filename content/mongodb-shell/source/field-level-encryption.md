# Field Level Encryption

When working with a [MongoDB Enterprise](http://www.mongodb.com/products/mongodb-enterprise-advanced)
or MongoDB Atlas cluster, you can use `mongosh` to configure \[Queryable Encryption\](<https://www.mongodb.com/docs/manual/core/queryable-encryption/>)
\[Client-Side Field Level Encryption\](<https://www.mongodb.com/docs/manual/core/security-client-side-encryption/>) and connect with encryption
support. Both Queryable Encryption and CSFLE use data encryption keys
for supporting encryption and decryption of field values, and store
this encryption key material in a Key Management Service (KMS).

`mongosh` supports the following KMS providers for use with
Queryable Encryption and CSFLE:

- Amazon Web Services KMS
- Azure Key Vault
- Google Cloud Platform KMS
- Locally Managed Keyfile

## Create a Data Encryption Key

The following procedure uses `mongosh` to create a data encryption key for
field level encryption.

Use the tabs below to select the KMS (Key Management Service)
appropriate for your deployment:

> **See also**
>
> - List of field level encryption shell methods
> - \[Queryable Encryption\](<https://www.mongodb.com/docs/manual/core/queryable-encryption/>)
>
> \- \[Client-Side Field Level Encryption\](<https://www.mongodb.com/docs/manual/core/security-client-side-encryption/>)
