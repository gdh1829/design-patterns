# MAC - Mandatory Access Control

## Description
>
- Subjects and objects each have a set of security attributes.
- Whenever a subject attempts to access an object, an authorization rule enforced by the operating system kernel examines these security attributes and decides whether the access can take place.
- Any operation by any subject on any object is tested against the set of authorization rules (aka policy) to determine if the operation is allowed.
- With mandatory access control, this security policy is centrally controlled by a security policy administrator; users do not have the ability to override the policy and, for example, grant access to files that would otherwise be restricted.

- `Subject` can `Action` to `Object`
- `Object` can be `Action` by `Subject`
- Based on user and group

## Examples

1. Grant `personA` permission to `create` an `article`
    - Subject: personA
    - Action: create
    - Object: article
1. Let `article` could `be created` by `personA`
    - Subject: article
    - Action: be created
    - Object: personA
1. `personA` can `create` an `article` now.
