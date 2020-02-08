DAC - Discretionary Access Control
---

## Description
The controls are discretionary in the sense that a subject with a certain access permission is capable of passing that permission (perhaps indirectly) on to any other subject.

- `Subject` can `Action` to `Object`
- `Subject` can `Grant` other `Subject`
- Base on user and group

## Examples
1. `PersonA` has right to `create` an `article`
2. `PersonA` can `create` an `article` now and give this permission(to create a article) to others.
    - `PersonA` grants `PersonB` to `create` an `article`
3. Now that PersonA grants it like the above, `PersonB` can `create` an `article`.
