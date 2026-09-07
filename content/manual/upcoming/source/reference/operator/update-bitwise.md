# Bitwise Update Operator

- - Name
  - Description

- - `\$bit`

  - Performs bitwise `AND`, `OR`, and `XOR` (exclusive OR)
    updates of integer values. The `$bit` operator performs a
    bitwise update of a field.

    The `$bit` operator can only be used with integer and long
    values. The integers must be 32-bit integer or 64-bit integer
    values.

    All numbers in `mongosh` are double precision
    floating point numbers, not integers. To define integers in
    `mongosh`, use the `Int32()` or `Long()`
    constructors. For example, `Int32(5)`, `Long(23455)`.

    To learn more about integers and long numbers, see
    shell-type-int and shell-type-long.
