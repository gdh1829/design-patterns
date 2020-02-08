# RBAC - Role Based Access Control

## Abbreviations in this doc

- ACL: Access Control List
- DAC: Discretionary Access Control
- MAC: Mandatory Access Control

## Description
>
BRAC differs from ACL, used in traditional DAC systems, in that __it assings permissions to specific operations with meaning in the organization__, rather than to low level data objects.  
For example, an ACL could be used to grant or deny write access to a particular system file, but it would not dictate how that file could be changed.  
In an RBAC-based system, an operation might be to 'create a credit account' transaction in a financial application or to 'populate a blood sugar level test' record in a medical application.

- `Subject` is a `Role` which has `Permission` of `Action` to `Object`
- Can implement MAC or DAC
- (User or group)-role-permission-object
- Concept
  - Subject
  - Role
  - Permission
  - Operation

## Group vs Role

- Group: a collection of users
  - PersonA, B and C are memebers of Developement Division.
- Role: a collection of permissions
  - Writer is a role, which can create, update articles
  - Role can be applied to user and group.

## Example

1. Set permissions named `write article` and `manage article`

```yml
Permissions:
  - Name: write article
    Operations:
      - Object: Article
        Action: Created
      - Object: Article
        Action: Updated
      - Object: Article
        Action: Read
  - Name: manage article
    Operations:
      - Object: Article
        Action: Delete
      - Object: Article
        Action: Read
```

1. Set a Role named `Writer`, give it `write article` permission, and a Role named `Manager`, give it `manage article` permission. CEO has all permissions.

```yml
Role:
  - Name: Writer
    Permissions:
      - write article
  - Name: Manager
    Permissions:
      - manage article
  - Name: CEO
    Permissions:
      - write article
      - manage article
```

1. Give PersonA `Writer` role

```yml
Subject: PersonA
Roles:
  - Writer
```

1. PersonA can create an article now.
1. Give PersonB `Writer` and `Manager` roles.

```yml
Subject: PersonB
Roles:
  - Writer
  - Manager
```

1. PersonB can create and delete article now.
