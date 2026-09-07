# Role-Based Access Control in Self-Managed Deployments

MongoDB employs Role-Based Access Control (RBAC) to govern access to a
MongoDB system. A user is granted one or more roles that
determine the user's access to database resources and operations. Outside
of role assignments, the user has no access to the system.

## Enable Access Control

MongoDB does not enable access control by default. You can enable
authorization using the `--auth` or the
`security.authorization` setting. Enabling internal authentication also enables
client authorization.

Once access control is enabled, users must authenticate themselves.

## Roles

A role grants privileges to perform the specified actions on a resource. Each privilege is either specified
explicitly in the role or inherited from another role or both.

### Access

Roles never limit privileges. If a user has two roles, the role with the
greater access takes precedence.

For example, if you grant the `read` role on a database to
a user that already has the `readWriteAnyDatabase` role, the
`read` grant does **not** revoke write access on the database.

To revoke a role from a user, use the `revokeRolesFromUser`
command.

### Authentication Restrictions

Roles can impose authentication restrictions on users, requiring them to
connect from specified source and destination IP address ranges.

For more information, see create-role-auth-restrictions.

### Privileges

A privilege consists of a specified resource and the actions permitted on the
resource.

A resource is a database,
collection, set of collections, or the cluster. If the resource is the
cluster, the affiliated actions affect the state of the system rather
than a specific database or collection. For information on the resource
documents, see /reference/resource-document.

An action specifies the operation
allowed on the resource. For available actions see
/reference/privilege-actions.

### Inherited Privileges

A role can include one or more existing roles in its definition, in which case
the role inherits all the privileges of the included roles.

A role can inherit privileges from other roles in its database. A role created
on the `admin` database can inherit privileges from roles in any database.

### View Role's Privileges

You can view the privileges for a role by issuing the `rolesInfo`
command with the `showPrivileges` and `showBuiltinRoles` fields both set to
`true`.

## Users and Roles

You can assign roles to users during the user creation. You can also
update existing users to grant or revoke roles. For a full list of user
management methods, see user-management-methods

A user assigned a role receives all the privileges of that role. A user
can have multiple roles. By assigning to the user roles in various
databases, a user created in one database can have permissions to act on
other databases.

> **Note**
>
> The first user created in the database should be a user administrator
> who has the privileges to manage other users. See
> /tutorial/enable-authentication.

## Built-In Roles and User-Defined Roles

MongoDB provides built-in roles that
provide set of privileges commonly needed in a database system.

If these built-in-roles cannot provide the desired set of privileges,
MongoDB provides methods to create and modify user-defined roles.

## LDAP Authorization

MongoDB Enterprise supports querying an LDAP server for the LDAP groups the
authenticated user is a member of. MongoDB maps the Distinguished Names (DN)
of each returned group to roles on the `admin` database.
MongoDB authorizes the user based on the mapped roles and their associated
privileges. See LDAP Authorization for more
information.
