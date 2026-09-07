# Security Checklist for Self-Managed Deployments

This document provides a list of security measures that you should
implement to protect your MongoDB installation. The list is not meant
to be exhaustive.

## Pre-production Checklist/Considerations

### ➤ Enable Access Control and Enforce Authentication

- Enable access control and specify an authentication mechanism.

  MongoDB Community supports a number of authentication mechanisms that clients can use to verify
  their identity:

  - authentication-scram (*Default*)
  - X.509 Certificate Authentication.

  In addition to the preceding mechanisms, MongoDB Atlas and MongoDB
  Enterprise support the following mechanisms:

  - LDAP proxy authentication, and
  - Kerberos authentication.

  These mechanisms allow MongoDB to integrate into your
  existing authentication system.

> **See also**
>
> - /core/authentication
>
> \- /tutorial/enable-authentication

### ➤ Configure Role-Based Access Control

- Create a user administrator **first**, then
  create additional users. Create a unique MongoDB user for each
  person/application that accesses the system.
- Follow the principle of least privilege. Create roles that define the
  exact access rights required by a set of users. Then create
  users and assign them only the roles they need to perform their
  operations. A user can be a person or a client application.

> **Note**
>
> A user can have privileges across different databases. If a user
> requires privileges on multiple databases, create a single user
> with roles that grant applicable database privileges instead of
> creating the user multiple times in different databases.

> **See also**
>
> - /core/authorization
> - /tutorial/create-users
>
> \- /tutorial/manage-users-and-roles

### ➤ Encrypt Communication (TLS/SSL)

- Configure MongoDB to use TLS/SSL for all incoming and outgoing
  connections. Use TLS/SSL to encrypt communication between
  `mongod` and `mongos` components of a
  MongoDB deployment as well as between all applications and
  MongoDB.

> **See also**
>
> /tutorial/configure-ssl.

### ➤ Encrypt and Protect Data

- You can encrypt data in the storage layer with the WiredTiger storage
  engine's native /core/security-encryption-at-rest.
- If you are not using WiredTiger's encryption at rest, MongoDB
  data should be encrypted on each host using file-system, device,
  or physical encryption (for example dm-crypt). You should also protect
  MongoDB data using file-system permissions. MongoDB data includes data
  files, configuration files, auditing logs, and key files.
- You can use qe-manual-feature-qe or manual-csfle-feature
  to encrypt fields in documents application-side prior to transmitting data
  over the wire to the server.
- Collect logs to a central log store. These logs contain database
  authentication attempts including source IP addresses.

### ➤ Limit Network Exposure

- Ensure that MongoDB runs in a trusted network environment and
  configure firewall or security groups to control inbound and
  outbound traffic for your MongoDB instances.
- Disable direct SSH root access.
- Allow only trusted clients to access the network interfaces and
  ports on which MongoDB instances are available.

> **See also**
>
> - /core/security-hardening
> - the `net.bindIp` configuration setting
> - the `security.clusterIpSourceAllowlist` configuration
>   setting
> - the authenticationRestrictions field to the
>   `db.createUser()` command to specify a per-user IP
>   allow list.

### ➤ Audit System Activity

- Track access and changes to database configurations and data.
  [MongoDB Enterprise](http://www.mongodb.com/products/mongodb-enterprise-advanced)
  includes a system auditing facility that can record
  system events (including user operations and connection events) on a
  MongoDB instance. These audit records permit forensic analysis
  and allow administrators to exercise proper controls. You can set
  up filters to record only specific events, such as authentication
  events.

> **See also**
>
> - /core/auditing
>
> \- /tutorial/configure-auditing

### ➤ Run MongoDB with a Dedicated User

- Run MongoDB processes with a dedicated operating system user
  account. Ensure that the account has permissions to access data
  but no unnecessary permissions.

> **See also**
>
> /installation

### ➤ Run MongoDB with Secure Configuration Options

- MongoDB supports the execution of JavaScript code for certain
  server-side operations: `mapReduce`, `\$where`,
  `\$accumulator`, and `\$function`. If you do
  not use these operations, disable server-side scripting by using
  the `--noscripting` option.
- Keep input validation enabled. MongoDB enables input validation
  by default through the `net.wireObjectCheck` setting.
  This ensures that all documents stored by the
  `mongod` instance are valid BSON.

### ➤ Request a Security Technical Implementation Guide (where applicable)

- The Security Technical Implementation Guide (STIG) contains
  security guidelines for deployments within the United States
  Department of Defense. MongoDB Inc. provides its STIG, upon
  [request](http://www.mongodb.com/lp/contact/stig-requests).

### ➤ Consider Security Standards Compliance

- For applications requiring HIPAA or PCI-DSS compliance, please
  refer to the [MongoDB Security Reference Architecture](https://www.mongodb.com/collateral/mongodb-security-architecture)
  to learn more about how you can use MongoDB's key security
  capabilities to build compliant application infrastructure.

## Antivirus and Endpoint Detection and Response Scanning

## Periodic/Ongoing Production Checks

- Periodically check for [MongoDB Product CVE](https://www.mongodb.com/alerts) and upgrade your products .
- Consult the [MongoDB end of life dates](https://www.mongodb.com/support-policy) and upgrade your
  MongoDB installation as needed. In general, try to stay on the latest
  version.
- Ensure that your information security management system policies
  and procedures extend to your MongoDB installation, including
  performing the following:
  - Periodically apply patches to your machine.
  - Review policy/procedure changes, especially changes to your
    network rules to prevent inadvertent MongoDB exposure to the
    Internet.
  - Review MongoDB database users and periodically rotate them.
- If you're using {+qe+}, compact the metadata collections as directed in
  Scheduling Metadata Compaction to reduce
  storage space.

## Report Suspected Security Bugs

If you suspect that you have identified a security bug in any MongoDB products,
please report the issue through the MongoDB [Bug Submission Form](https://www.mongodb.com/security).
