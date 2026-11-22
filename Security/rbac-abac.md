# RBAC and ABAC – Access Control

## 1. What is Access Control?

Access control determines who can access a system, what resources they can access, and what operations they can perform.

The three main concepts are:

- Authentication: Verifies identity.
- Authorization: Determines permissions.
- Access Control: Enforces rules governing access to resources.

## 2. What is RBAC?

RBAC stands for Role-Based Access Control.

It assigns permissions to roles rather than directly assigning permissions to individual users.

Users are assigned roles, and roles contain permissions.

### Example

Consider an application with three roles:

| Role | Permissions |
|---|---|
| Admin | Create, Read, Update, Delete |
| Manager | Create, Read, Update |
| User | Read |

### How RBAC works

1. User logs into the application.
2. System identifies the user's role.
3. System retrieves permissions associated with that role.
4. User requests a resource.
5. Authorization layer checks whether the role has the required permission.
6. Access is granted or denied.

### Example

```python
roles = {
    "admin": ["read", "write", "delete"],
    "manager": ["read", "write"],
    "user": ["read"]
}

def check_permission(role, permission):
    return permission in roles.get(role, [])

print(check_permission("admin", "delete"))  # True
print(check_permission("user", "delete"))   # False
```

## 3. What is ABAC?

ABAC stands for Attribute-Based Access Control.

It makes access decisions based on attributes associated with:

- User
- Resource
- Action
- Environment

### Example

A user can access a document only if:

- User department = Finance
- Document classification = Finance
- User location = US
- Action = Read

The authorization engine evaluates these attributes against an access policy.

### Example policy

```python
def can_access(user, document):
    return (
        user["department"] == document["department"]
        and user["clearance"] >= document["classification"]
        and user["region"] == document["region"]
    )
```

## 4. RBAC vs ABAC

| Feature | RBAC | ABAC |
|---|---|---|
| Full form | Role-Based Access Control | Attribute-Based Access Control |
| Decision based on | User role | Multiple attributes |
| Complexity | Relatively simple | More complex |
| Flexibility | Role dependent | Highly flexible |
| Management | Role and permission mapping | Policies and attributes |
| Example | Admin can delete | Finance user can read US finance documents |

## 5. What is the Principle of Least Privilege?

The Principle of Least Privilege means users and services should receive only the permissions necessary to perform their tasks.

Example:

A reporting service needs read-only access to a database.

It should not receive:
- Database administrator permissions.
- Delete permissions.
- Schema modification permissions.

Benefits:
- Reduces unauthorized access.
- Limits damage from compromised accounts.
- Improves security.
- Reduces unnecessary privileges.

## 6. What is the difference between RBAC and ACL?

ACL stands for Access Control List.

RBAC assigns permissions to roles.

ACL defines which users or groups can access a particular resource and what operations they can perform.

Example:

A document ACL:

| User | Permission |
|---|---|
| User A | Read |
| User B | Read, Write |
| User C | No access |

## 7. What is ABAC policy evaluation?

A Policy Decision Point (PDP) evaluates the policy and determines whether access should be granted.

A Policy Enforcement Point (PEP) enforces that decision.

Typical flow:

1. User requests a resource.
2. PEP intercepts the request.
3. PEP sends relevant attributes to PDP.
4. PDP evaluates applicable policies.
5. PDP returns Permit or Deny.
6. PEP enforces the decision.

## 8. What is Multi-Tenant Authorization?

Multi-tenancy means multiple customers or organizations share the same application infrastructure while keeping their data isolated.

Authorization must ensure that one tenant cannot access another tenant's data.

Example:

```python
def can_access_resource(user, resource):
    return user["tenant_id"] == resource["tenant_id"]
```

Tenant isolation should also be enforced at the database or data-access layer, not only in the frontend.

## 9. What is the difference between RBAC and OAuth scopes?

RBAC determines permissions through assigned roles.

OAuth scopes represent permissions granted to a client or token.

Example:

- Role: Administrator
- Scope: accounts:read

A system can combine roles and scopes to enforce authorization.

## 10. Security Best Practices

1. Follow the principle of least privilege.
2. Use centralized authorization policies.
3. Enforce authorization on the server side.
4. Validate tenant ownership for every resource request.
5. Review and revoke unused permissions.
6. Audit access to sensitive resources.
7. Avoid hardcoding permissions throughout application code.
8. Apply separation of duties for sensitive operations.

## Summary

- RBAC = Access based on roles.
- ABAC = Access based on attributes.
- ACL = Permissions attached to resources.
- Least Privilege = Minimum required permissions.
- PDP = Makes access decisions.
- PEP = Enforces access decisions.
- Multi-Tenant Security = Prevents cross-tenant access.
