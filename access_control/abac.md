# ABAC - Attribute-Based Access Control

## Abbreviations

- RBAC: Role Based Access Control
- XACML: eXtensible Access Control Markup Language

## Description
>
Unlike RBAC, which employs pre-defined roles that carry a specific set of privileges associated with them and to which subjects are assigned.  
__the key difference with ABAC is the concept of policies that express a complex Boolean rule set that can evaluate many different attributes.__

- `Subject` who is xxx can `Action` to `Object` which is xxx in `Environment`.
- Concept
  - `Policies`: bring attributes together to express what can happen and is not allowed.
  - `Attributes`
    - Subject
      - age, clearance, department, role, job title
    - Action
      - read, write, update, delete
    - Resource
      - the obejct type (medical record, bank account...), the department, the classification or sensitivity, the location
    - Contextual (environment)
      - attributes that deal with time, location or dynamic aspects of the access control scenario
- Standard
  - XACML

## Examples

- PersonA who in Product Department as a Writer could create and update an article, which tag is technology and software in draft mode, and the connection is from Taiwan between 2020-02-10 and 2021-02-10.

```yml
Subject:
  Name: PersonA
  Department: Product
  Role: Writer
Action:
  - create
  - update
Resource:
  Type: Article
  Tag:
    - technology
    - software
  Mode:
    - draft
Contextual:
  Location: Taiwan
  StartTime: 2020-02-10
  EndTime: 2021-02-10
```

## AWS Resource-Based Policies is a kind of ABAC

- Limits Terminating EC2 instances to an IP Address Range

```json
{
  "Version": "2012-10-17",
  "Statement": [
      {
          "Effect": "Allow",
          "Action": [
              "ec2:TerminateInstances"
          ],
          "Resource": [
              "*"
          ]
      },
      {
          "Effect": "Deny",
          "Action": [
              "ec2:TerminateInstances"
          ],
          "Condition": {"NotIpAddress": {"aws:SourceIp": [
              "192.0.2.0/24",
              "203.0.113.0/24"
              ]}},
          "Resource": [
              "arn:aws:ec2:<REGION>:<ACCOUNTNUMBER>:instance/*"
          ]
      }
  ]
}
```