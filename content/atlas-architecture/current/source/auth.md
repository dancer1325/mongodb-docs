# Guidance for Atlas Authorization and Authentication

.. meta: :description: Learn about the different authorization and authentication mechanisms that Atlas supports.

Authentication is the process of verifying the identity of a user. Authorization is the process of assigning permissions to an authenticated user. Atlas uses a deny-by-default security model. Users and machine accounts must authenticate and be assigned permissions before they can access any resources. For Authentication, Atlas provides robust authentication mechanisms that seamlessly integrate with your existing identity systems, providing secure access to the UI, database, and API (Application Programming Interface)\s. For Authorization, Atlas provides Role-Based Access Control (RBAC) to govern access to Atlas. You must grant a user one or more roles to determine the user's access resources and operations. Outside of role assignments, the user has no access to the system.

- [Authentication](/auth/authentication)
- [Authorization](/auth/authorization)
- [Auth Examples](/auth/auth-examples)
